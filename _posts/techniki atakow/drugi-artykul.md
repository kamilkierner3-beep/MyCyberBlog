---
title: "ClickFix (fake CAPTCHA) — jak działa ta technika i jak ją wykrywać (SOC/Blue Team)"
description: "Praktyczny opis ClickFix: łańcuch ataku, typowe LOLBiny, ślady w RunMRU, detekcje i hardening. Gotowe do wklejenia na GitHub."
tags: [cybersecurity, soc, detection, windows, phishing, malware, infostealer]
---

## TL;DR

**ClickFix** to technika socjotechniczna, w której ofiara sama uruchamia złośliwą komendę (często przez Win+R / PowerShell / Terminal), bo strona udaje „weryfikację człowieka” (CAPTCHA/Turnstile/reCAPTCHA) albo drobny problem techniczny do „naprawy”. [page:1]  
Microsoft opisywał wzrost popularności ClickFix oraz to, że kampanie potrafią trafiać w tysiące urządzeń dziennie i często dowożą ładunki typu infostealer (np. Lumma) prowadzące do kradzieży informacji i eksfiltracji danych. [page:1]

---

## Co to jest ClickFix (w 1 minucie)

Atakujący tworzy landing page (lub HTML/URL w phishingu), która wygląda jak legitny komunikat (np. „Verify you are human”) i prowadzi użytkownika krok po kroku do uruchomienia komendy. [page:1]  
Częstym trikiem jest kopiowanie komendy do schowka „w tle” po kliknięciu przycisku „Verify/How to fix”, a potem pokazanie instrukcji „wklej i uruchom”. [page:1]  
To działa, bo element „manualnego” działania użytkownika potrafi ominąć część automatycznych zabezpieczeń, które polegają na blokowaniu samodzielnie uruchamianych plików/załączników. [page:1]

---

## Łańcuch ataku (attack chain)

Typowy przebieg wygląda tak: [page:1]

1. **Dostarczenie (arrival vector)**: phishing, malvertising (reklamy), drive-by z przejętych stron. [page:1]  
2. **Wabik wizualny**: strona podszywa się pod reCAPTCHA/Cloudflare Turnstile albo znaną markę/platformę. [page:1]  
3. **Instrukcje „naprawy”**: ofiara dostaje polecenie użycia Win+R, Windows Terminal lub PowerShell i uruchomienia wklejonej komendy. [page:1]  
4. **Pobranie payloadu**: po wykonaniu komendy malware/loader jest pobierany na hosta. [page:1]  
5. **Dalsze etapy**: infostealer / RAT / loader / rootkit, a potem kradzież danych, zdalny dostęp, persistence i kolejne ładunki. [page:1]

Microsoft wskazuje, że ClickFix bywa używany do dostarczania m.in. infostealerów (np. Lumma), RAT-ów (np. AsyncRAT/Xworm/NetSupport), loaderów (np. Latrodectus/MintsLoader) oraz nawet rootkitów (np. modyfikacje r77). [page:1]  
Część ładunków bywa „fileless” (ładowanie w pamięci) i korzysta z LOLBinów, a kod może być wstrzykiwany w procesy takie jak `msbuild.exe`, `regasm.exe` czy `powershell.exe`. [page:1]

---

## Jak wygląda przynęta (TTPs)

### Najczęstsze „maski”
ClickFix lury często udają: reCAPTCHA, Cloudflare Turnstile, a czasem platformy społecznościowe (np. Discord) lub strony instytucji. [page:1]  
Microsoft opisał, że landing page może używać JS/iframe/CSS, a kliknięcie checkboxa „Verify you are human” potrafi uruchomić logikę kopiowania komendy do schowka (Clipboard API). [page:1]

### Co ofiara faktycznie robi
W wariantach Windows ofiara jest prowadzona do uruchomienia poleceń w Win+R/Terminal/PowerShell. [page:1]  
W wariantach macOS Microsoft opisał kampanię, gdzie przynęta potrafiła kopiować komendę, która m.in. wyłudza hasło użytkownika i pobiera payload przez `curl`, a potem próbuje obejść zabezpieczenia atrybutem kwarantanny. [page:1]

> W artykule na GitHub **nie publikuj** działających złośliwych komend ani obfuskacji 1:1 — skup się na objawach, logach i detekcjach.

---

## Co wykrywać na Windows (SOC/DFIR)

### 1) Ślady po Win+R: RunMRU (ważne!)
Microsoft wskazuje, że użycie Windows Run dialog zostawia ślady w kluczu rejestru **RunMRU** (historia uruchomień) i że wpis nie pojawi się, jeśli uruchomienie procesu się nie uda. [page:1]  
W kontekście ClickFix warto polować na ciągi sugerujące LOLBiny zdolne pobrać/uruchomić kod, np. `powershell`, `mshta`, `rundll32`, `wscript`, `curl`, `wget`. [page:1]  
Microsoft podkreśla też, że PowerShell jest często nadużywany z cmdletami typu `iwr`/`irm`/`iex` (Invoke-WebRequest/Invoke-RestMethod/Invoke-Expression). [page:1]

**Przykładowe „red flags” w RunMRU / command line (opisowo, bez payloadu):** [page:1]
- URL skracacze, dziwne TLD, bezpośrednie IP, CDN/paste.
- Pobieranie plików typu `.hta`, `.vbs`, `.ps1`, `.msi`, `.bat`, `.zip` lub „udawanie” multimediów przez rozszerzenia `.mp3/.mp4/.png` albo podwójne rozszerzenia.
- Frazy udające CAPTCHA/Turnstile w treści poleceń („I am not a robot”, „Verification ID”, itp.).

### 2) Procesy i parent-child
W typowym ClickFix ofiara uruchamia procesy „native”, więc szukaj anomalii w drzewach procesów (np. `explorer.exe` -> `powershell.exe` / `mshta.exe` / `wscript.exe`). [page:1]  
Microsoft opisuje też scenariusze z wieloetapowym łańcuchem (np. skrypty VBS, zadania harmonogramu, `rundll32.exe`) w kampaniach ClickFix. [page:1]

### 3) Telemetria, którą warto mieć
- Logi PowerShell (np. Script Block Logging) dla widoczności obfuskacji i poleceń, co Microsoft rekomenduje jako element ograniczania ryzyka i poprawy detekcji. [page:1]  
- EDR/XDR: alerty na nietypowe uruchomienia LOLBinów i podejrzane command line. [page:1]  
- DNS/HTTP: blokady domen/C2 i anomalia „pierwszego połączenia” zaraz po wykonaniu komendy (Microsoft wspomina znaczenie blokowania domen na wczesnym etapie). [page:1]

---

## Detekcje (pomysły do reguł) — bez „weaponization”

Poniżej są pomysły na detekcje, które nie wymagają publikowania złośliwych komend:

- Wpisy RunMRU zawierające `powershell`/`mshta`/`rundll32`/`wscript` + `http`/`https` lub nietypowe TLD. [page:1]  
- `explorer.exe` uruchamia `powershell.exe` z podejrzanie długą linią poleceń, znakami obfuskacji, Base64 lub „łańcuchem” wielu interpreterów (Microsoft opisuje obfuskacje: Base64, konkatenacje, escape, nested chains). [page:1]  
- `mshta.exe`/`wscript.exe`/`cmd.exe` uruchamiane interaktywnie bez typowej ścieżki biznesowej (np. brak znanego installera, brak znanego parenta). [page:1]  
- Korelacja: „phishing click” → w krótkim oknie czasowym uruchomienie Win+R/PowerShell → nowy outbound do świeżej domeny. [page:1]

---

## Jak ograniczać ryzyko (hardening + user awareness)

Microsoft rekomenduje połączenie edukacji użytkowników (żeby nie wykonywali „napraw” z internetu) z twardymi politykami środowiska. [page:1]  
Przykłady twardych kontroli, które Microsoft wymienia: ograniczenie/wyłączenie Run dialog (jeśli zbędny), App Control blokujący uruchamianie natywnych binarek z Run oraz ustawienia Windows Terminal ostrzegające o wklejaniu wielu linii. [page:1]  
Microsoft zaleca też m.in. SmartScreen/web protection/network protection, oraz w warstwie poczty — mocniejsze filtrowanie i ochronę linków/załączników (Defender for Office 365). [page:1]

---

## Mini-checklista dla SOC L1

- Czy mamy widoczność RunMRU (DFIR/EDR/telemetria)? [page:1]  
- Czy w ostatnich incydentach pojawia się wzorzec `explorer.exe -> powershell/mshta/wscript/rundll32`? [page:1]  
- Czy wykrywamy/polujemy na obfuskacje i nadużycia LOLBinów (nested chains, Base64, string splitting)? [page:1]  
- Czy użytkownicy wiedzą, że „CAPTCHA, która każe uruchomić Win+R” to czerwony alarm? [page:1]

---

## Źródła

- Microsoft Security Blog — *Think before you Click(Fix): Analyzing the ClickFix social engineering technique* (Microsoft Threat Intelligence, 2025-08-21). [page:1]
