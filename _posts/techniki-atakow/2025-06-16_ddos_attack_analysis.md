---
title: "DDoS — Mechanizm ataku"
date: 2026-06-16
categories: techniki-atakow
---

## 1. Na czym polega atak DDoS
DDoS (Distributed Denial of Service) to skoordynowany atak polegający na przeciążeniu usługi ogromną liczbą żądań wysyłanych jednocześnie z tysięcy rozproszonych urządzeń (botnetów). Celem jest wyczerpanie zasobów — pasma, CPU, pamięci lub limitów połączeń — aż do całkowitej niedostępności aplikacji, API lub infrastruktury. Ataki często prowadzone są falami, aby utrudnić reakcję obronną.

## 2. Przygotowanie ataku przez cyberprzestępców
Atakujący budują lub wynajmują botnet (IoT, VPS, urządzenia mobilne), następnie wykonują rekonesans: skanują porty, analizują limity połączeń, identyfikują brak WAF/CDN. Kolejnym krokiem jest wybór wektora ataku i przygotowanie infrastruktury do synchronizacji ruchu. Źródła są maskowane przez proxy, VPN, TOR lub spoofing IP, co utrudnia identyfikację sprawców.

## 3. Techniki wykorzystywane w atakach DDoS
### Wolumetryczne
- UDP Flood  
- ICMP Flood  
- DNS/NTP/CLDAP Amplification (wzmocnienie ×50–1000)

### Protokołowe
- SYN Flood  
- ACK/RST Flood  
- Fragmentacja pakietów  
- Ping of Death

### Aplikacyjne (L7)
- HTTP GET/POST Flood  
- Slowloris / Slow POST  
- Ataki na endpointy o wysokiej złożoności (np. wyszukiwarki, generatory PDF)

### Multi‑vector
Łączenie kilku technik w jednej kampanii, co zwiększa skuteczność i utrudnia obronę.

## 4. Zapobieganie i obrona
Skuteczna ochrona wymaga warstwowego podejścia: rate‑limiting, filtrowanie ruchu, WAF + CDN z analizą anomalii, Anycast i rozproszenie infrastruktury, ochrona na poziomie ISP (scrubbing), twarde limity zasobów, izolacja usług oraz stały monitoring ruchu. Kluczowe jest szybkie wykrywanie nagłych skoków wolumenu i automatyczne blokowanie anomalii.

## 5. Skutki udanego ataku DDoS
Udanego ataku nie da się zignorować — prowadzi do niedostępności usług, strat finansowych, przerw operacyjnych i utraty reputacji. W sektorach krytycznych może sparaliżować procesy biznesowe, a często służy jako zasłona dymna dla włamań, ransomware lub manipulacji systemami. DDoS bywa pierwszym etapem większej kampanii cyberataków.
