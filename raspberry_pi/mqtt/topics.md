# MQTT — Dokumentacja komunikacji

## 1. Cel

Komunikacja MQTT służy do przesyłania danych pomiarowych
z ESP32 do serwera Raspberry Pi 3B+.

Broker MQTT: Eclipse Mosquitto  
Protokół: MQTT  
Port: 1883 (sieć lokalna)

## 2. Architektura

ESP32 (Publisher)
    |
    | Wi-Fi / MQTT
    v
Raspberry Pi — Mosquitto (Broker)
    |
    v
Telegraf (Subscriber)
    |
    v
InfluxDB (baza danych)
    |
    v
Grafana (wizualizacja)

## 3. Tematy MQTT (Topics)

| Topic | Opis |
|---|---|
| komora/telemetry | Dane pomiarowe z ESP32 |
| komora/status | Informacja o dostępności ESP32 |

## 4. Format danych pomiarowych

Topic: `komora/telemetry`

Format wiadomości: JSON

Przykładowa wiadomość:

```json
{
  "co2_ppm": 650,
  "temperature_c": 23.5,
  "humidity_pct": 45.2,
  "pressure_hpa": 1013.2
}
```

Znaczenie pól:

- `co2_ppm` — stężenie CO2 [ppm]
- `temperature_c` — temperatura [°C]
- `humidity_pct` — wilgotność względna [%]
- `pressure_hpa` — ciśnienie atmosferyczne [hPa]

## 5. Parametry transmisji

- MQTT QoS: 0 (początkowo)
- Częstotliwość publikacji: co 5 sekund
- Retain dla pomiarów: false
- Timestamp: nadawany podczas zapisu do bazy danych

## 6. Status urządzenia

Topic: `komora/status`

Możliwe wartości:

- `online` — ESP32 jest połączone
- `offline` — ESP32 utraciło połączenie

Docelowo status będzie obsługiwany przez mechanizm
MQTT Last Will and Testament (LWT).

## 7. Bezpieczeństwo

- Broker dostępny w zaufanej sieci lokalnej.
- Połączenie ESP32 z brokerem wymaga uwierzytelnienia.
- Hasła i dane dostępowe nie są przechowywane w repozytorium.
- Port MQTT nie jest udostępniany bezpośrednio do internetu.

## 8. Status implementacji

Etap 1 — projekt przejściowy.

Aktualny stan: projektowanie komunikacji.

Do wykonania:
- Instalacja i konfiguracja Mosquitto.
- Test przesyłania danych MQTT.
- Integracja ESP32.
- Zapis danych i wizualizacja.