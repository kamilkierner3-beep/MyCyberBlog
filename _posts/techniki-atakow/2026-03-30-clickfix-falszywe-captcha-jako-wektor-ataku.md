---
layout: post
date: 2026-03-30
categories: techniki-atakow
---

# ClickFix: falszywa CAPTCHA jako wektor ataku

ClickFix to **technika socjotechniczna**, w której ofiara widzi fałszywą CAPTCHA lub „test weryfikacyjny” i zostaje poproszona o skopiowanie komendy z przeglądarki do Win+R, Terminala lub PowerShell. Atak nie wykorzystuje klasycznej luki w systemie, tylko przyzwyczajenie użytkownika do „naprawiania problemów” zgodnie z instrukcją na ekranie, co pozwala obejść część typowych filtrów maila i AV. W efekcie użytkownik sam staje się instalatorem malware.

## Jak wygląda atak ClickFix?

Użytkownik trafia na stronę z fałszywą CAPTCHA (zwykle z maila phishingowego, reklamy lub przekierowania z podejrzanej strony), klika „Verify” i strona automatycznie kopiuje złośliwą komendę do schowka, pokazując instrukcję „naciśnij Win+R, wklej i uruchom”.Komenda uruchamia PowerShell lub inny LOLBin, który pobiera i odpala payload – najczęściej infostealery kradnące hasła, cookies, dane logowania do poczty, VPN i przeglądarek.Tak zdobyte dane mogą zostać użyte do przejęcia kont, dalszego ruchu bocznego i kolejnych ataków (np. ransomware) w sieci ofiary. 

## Najczęstsze techniki używane w ClickFix

W kampaniach ClickFix wykorzystywane są legalne narzędzia systemowe (LOLBins), takie jak 'powershell.exe', 'wscript.exe', 'mshta.exe' czy 'rundll32.exe', które pobierają i uruchamiają złośliwy kod z internetu.Atakujący stosują obfuskację komend (Base64, dzielenie stringów, zagnieżdżone wywołania) i hostują pliki na pozornie zaufanych usługach (CDN, Google, kalendarze, paste serwisy), żeby utrudnić analizę i detekcję. Coraz częściej widzimy też kampanie cross‑platform (Windows/macOS), w których mechanika socjotechniki jest taka sama, zmienia się tylko docelowa komenda.

## Jak się bronić i jak ograniczyć ryzyko?

Organizacje powinny szkolić użytkowników, że żadna prawdziwa CAPTCHA ani znana usługa nie wymaga otwierania Win+R ani wklejania komend z internetu – to zawsze czerwony alarm.Po stronie technicznej warto ograniczyć lub wyłączyć Win+R tam, gdzie nie jest potrzebny, wdrożyć Application Control dla PowerShell/LOLBins, włączyć logowanie poleceń i reguły EDR wykrywające nietypowe uruchomienia tych procesów (długie, zakodowane command line, nietypowe parent‑procesy).Dodatkowo filtry poczty i web proxy powinny blokować znane domeny kampanii, strony o niskiej reputacji oraz kategorie serwisów, gdzie często hostowane są fałszywe CAPTCHA, co zmniejsza szansę, że użytkownik w ogóle zobaczy przynętę. 








