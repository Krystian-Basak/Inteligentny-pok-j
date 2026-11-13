# IoT Monitorowanie parametrów mikroklimatu

**Projekt IoT-owego systemu telemetrii i automatycznej regulacji mikroklimatu komory regeneracji ze szczególnym uwzględnieniem parametrów jakości snu**

## 1. Opis projektu

Celem projektu jest zaprojektowanie i realizacja systemu IoT umożliwiającego monitorowanie parametrów środowiskowych oraz, w późniejszym etapie, automatyczną regulację mikroklimatu zamkniętej komory testowej o pojemności 21 litrów.

System koncentruje się na pomiarach:

- Stężenia dwutlenku węgla (CO₂)
- Temperatury powietrza
- Wilgotności względnej
- Ciśnienia atmosferycznego

Projekt realizowany jest w ramach studiów na kierunku Automatyka i Robotyka na Politechnice Poznańskiej.

## 2. Etapy realizacji

### Etap I — System telemetryczny (projekt/projekt przejściowy)

Pierwszy etap obejmuje budowę układu pomiarowego opartego na mikrokontrolerze ESP32 oraz systemu zdalnego monitorowania parametrów środowiskowych.

**Główne funkcjonalności:**

- Odczyt parametrów z czujników przez magistralę I²C.
- Wyświetlanie aktualnych pomiarów na wyświetlaczu OLED.
- Transmisja danych przez Wi-Fi z wykorzystaniem protokołu MQTT.
- Odbiór danych przez serwer Raspberry Pi.
- Archiwizacja pomiarów w bazie danych.
- Wizualizacja danych w postaci wykresów i dashboardów.

### Etap II — Automatyczna regulacja mikroklimatu (planowana praca inżynierska)

Rozszerzenie projektu o fizyczną komorę testową, elementy wykonawcze oraz algorytmy sterowania, po zatwierdzeniu zakresu pracy inżynierskiej.

**Planowane funkcjonalności:**

- Sterowanie wentylacją w celu regulacji stężenia CO₂.
- Sterowanie matą grzewczą w celu regulacji temperatury.
- Implementacja regulatora PID i sterowania z histerezą.
- Rejestracja odpowiedzi dynamicznych obiektu.
- Porównanie jakości regulacji w różnych warunkach pracy.
- Opcjonalnie: implementacja algorytmu sterowania predykcyjnego.

## 3. Wykorzystywane technologie

### Hardware

| Komponent | Zastosowanie |
|---|---|
| ESP32 DevKit | Jednostka pomiarowa i komunikacyjna |
| Sensirion SCD30 | Pomiar CO₂, temperatury i wilgotności |
| DFRobot BMP280 | Pomiar ciśnienia atmosferycznego |
| OLED 0.96" | Lokalna wizualizacja pomiarów |
| Raspberry Pi 3B+ | Serwer lokalny MQTT i systemu monitorowania |

### Software

| Technologia | Zastosowanie |
|---|---|
| C++ / Arduino Framework | Oprogramowanie ESP32 |
| PlatformIO / VS Code | Środowisko programistyczne |
| I²C | Komunikacja z czujnikami i OLED |
| MQTT / Mosquitto | Bezprzewodowa transmisja danych |
| Telegraf | Przekazywanie danych pomiarowych |
| InfluxDB | Baza danych szeregów czasowych |
| Grafana | Wizualizacja danych pomiarowych |
| Raspberry Pi OS Lite | System operacyjny serwera |
| Git / GitHub | Kontrola wersji i dokumentacja |

Dobór oraz kompatybilność oprogramowania serwerowego zostaną zweryfikowane podczas implementacji.

## 4. Architektura systemu

```text
           SCD30       BMP280
              \         /
               \  I2C  /
                ESP32
                  |
        +---------+---------+
        |                   |
    OLED (I2C)          Wi-Fi / MQTT
                            |
                      Raspberry Pi
                            |
                        Mosquitto
                            |
                         Telegraf
                            |
                         InfluxDB
                            |
                         Grafana
                            |
                    Dashboard WWW
```

Wyświetlacz OLED prezentuje bieżące pomiary niezależnie od połączenia z serwerem.

## 5. Struktura repozytorium

```text
Inteligentny-pokoj/
├── raspberry_pi/
│   ├── mqtt/          # Konfiguracja i dokumentacja MQTT
│   ├── telegraf/      # Przetwarzanie danych pomiarowych
│   ├── grafana/       # Dashboardy i wizualizacje
│   └── scripts/       # Skrypty pomocnicze i testowe
├── esp32/             # Oprogramowanie mikrokontrolera
├── documentation/     # Schematy i dokumentacja techniczna
├── README.md
└── .gitignore
```

## 6. Komunikacja MQTT

Węzeł ESP32 będzie cyklicznie publikował dane pomiarowe w formacie JSON.

**Temat:** `komora/telemetry`

Przykładowa wiadomość:

```json
{
  "co2_ppm": 650,
  "temperature_c": 23.5,
  "humidity_pct": 45.2,
  "pressure_hpa": 1013.2
}
```

Szczegóły komunikacji opisano w pliku [`topics.md`](raspberry_pi/mqtt/topics.md).

## 7. Aktualny stan projektu

**Status: projektowanie i przygotowanie infrastruktury — Etap I.**

- [x] Opracowanie założeń i architektury systemu
- [x] Przygotowanie repozytorium GitHub
- [x] Przygotowanie dokumentacji komunikacji MQTT
- [ ] Instalacja Raspberry Pi OS Lite
- [ ] Konfiguracja brokera MQTT Mosquitto
- [ ] Testowanie przesyłania danych pomiarowych
- [ ] Uruchomienie ESP32 i czujników
- [ ] Implementacja obsługi OLED
- [ ] Integracja ESP32 z serwerem
- [ ] Konfiguracja bazy danych i dashboardu
- [ ] Testy kompletnego systemu telemetrycznego

## 8. Dalszy rozwój

Po zakończeniu pierwszego etapu i zatwierdzeniu kontynuacji projekt może zostać rozbudowany o automatyczną regulację temperatury i stężenia CO₂ w komorze testowej.

Dodatkowym kierunkiem rozwoju jest analiza danych pomiarowych oraz porównanie klasycznych i predykcyjnych metod sterowania.

---

**Politechnika Poznańska — Automatyka i Robotyka**

Projekt studencki | IoT · Embedded Systems · Telemetria · Automatyka
