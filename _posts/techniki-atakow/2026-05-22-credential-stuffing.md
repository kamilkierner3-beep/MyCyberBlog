---
title: "Credential Stuffing – atak na Twoje hasła w 2025/2026"
date: 2026-05-22
tags: [cybersecurity, credential-stuffing, ataki, hasła, AI, brute-force]
---

# Credential Stuffing – atak na Twoje hasła w 2025/2026

Credential Stuffing to zautomatyzowany atak, w którym przestępcy masowo testują skradzione pary login/hasło z wycieków danych na wielu serwisach jednocześnie.[web:35] Wykorzystują tzw. combo listy (np. z wycieków typu RockYou2024) oraz narzędzia takie jak OpenBullet2 czy Sentry MBA, wysyłając tysiące prób logowania równolegle i często omijając CAPTCHA dzięki usługom opartym na AI (CapSolver, nCaptcha).[web:35][web:40]

Skuteczność takich kampanii wynosi zwykle 0,5–2%, co oznacza, że z miliona skradzionych zestawów danych można w ciągu kilku godzin przejąć nawet kilkanaście tysięcy kont.[web:35] Dane pochodzą z dużych wycieków (np. LinkedIn 2021 – setki milionów rekordów), phishingu, malware typu infostealer (Raccoon, RedLine) oraz z darknetowych forów, gdzie listy są sprzedawane za niewielkie kwoty.[web:35] Serwis Have I Been Pwned gromadzi obecnie ponad 14 miliardów skompromitowanych logonów z publicznie znanych incydentów, co dobrze pokazuje skalę problemu.[web:35]

W 2025 roku głośnym przykładem był atak na Roku, w wyniku którego przejęto około 576 tysięcy kont klientów, a przestępcy wykonywali na nich zakupy sprzętu RTV.[web:16] Według CERT Polska liczba ataków typu credential stuffing wymierzonych w polskie instytucje finansowe wzrosła o około 40% rok do roku, a platformy streamingowe odnotowały wyraźny skok po ograniczeniu współdzielenia kont, co ujawniło skalę ponownie używanych haseł.[web:31][web:16]

Najważniejszą linią obrony dla użytkownika jest stosowanie unikalnych haseł do każdego serwisu oraz włączenie MFA (TOTP lub klucz FIDO2), najlepiej z pomocą menedżera haseł, który ułatwia zarządzanie złożonymi kombinacjami.[web:35] Dla organizacji kluczowe są mechanizmy rate limiting, wykrywanie anomalii logowań, blokowanie haseł znanych z wycieków (np. przez HaveIBeenPwned API), nowoczesne CAPTCHA oraz device fingerprinting wykrywający boty i nietypowe środowiska.[web:12][web:35] W frameworku MITRE ATT&CK credential stuffing klasyfikowany jest w taktykach Credential Access i Initial Access jako technika T1110.004, co podkreśla jego rolę jako jednego z głównych wektorów wejścia do systemów.[web:35]
