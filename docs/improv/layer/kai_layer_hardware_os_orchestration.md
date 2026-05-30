---
layout: default
title: Layer Hardware, OS & Orchestration
parent: Live Rehearsal Improvisation
nav_order: 1
---


# Hardware, OS & Orchestration

**Authors:** Malte Hillebrand, Philip Gerdes

File Change History:

| Date       | Change                  | Author |
| ---------- | ----------------------- | ------ |
| 2026-05-11 | Gemeinsame Überlegungen | Philip |
| 2026-05-22 | Spark                   | Philip |


### Hardware Anschaffungsempfehlungen

#### Config 00 (Nicht umsetzbar)

> Diese Konfiguration dient nur zur Veranschaulichung der aktuellen RAM Preise

| Bauteil      | Detail                                 | Anzahl | Preis (€) |
| ------------ | -------------------------------------- | ------ | --------- |
| Mainboard    | GigaByte TRX50 AI Top                  | 1      | 870       |
| CPU          | AMD Ryzen Threadripper 7960X (24 Core) | 1      | 1200      |
| GPU          | NVIDIA RTX PRO 4500 Blackwell          | 2      | 5500      |
| RAM          | Samsung 64GB DDR5-4800 ECC             | 1      | 2700      |
| Power        | be quiet! Straight Power 12 1200W      | 1      | 200       |
| Storage      | Samsung 9100 Pro 2TB                   | 1      | 360       |
| CPU Cooling  | NH-D9 TR5-SP6 4U                       | 1      | 120       |
| Case         | Inter-Tech IPC 4U K-439L               | 1      | 105       |
| Case Cooling | Noctua NF-A8 PWM                       | 2      | 38        |

> ~ 11.100 €

#### Config 01

| Bauteil      | Detail                                                                                                                                     | Anzahl | Preis (€) |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------ | ------ | --------- |
| Mainboard    | [ASUS ProArt X670E-CREATOR](https://www.idealo.de/preisvergleich/OffersOfProduct/202115535_-proart-x670e-creator-wifi-asus.html)           | 1      | 350       |
| CPU          | [AMD Ryzen 9 9950X](https://www.idealo.de/preisvergleich/OffersOfProduct/204581108_-ryzen-9-9950x-boxed-amd.html)                          | 1      | 480       |
| GPU          | [NVIDIA RTX PRO 4500 Blackwell](https://www.idealo.de/preisvergleich/OffersOfProduct/207327484_-rtx-pro-4500-blackwell-nvidia.html)        | 2      | 5500      |
| RAM          | [Crucial 32GB DDR5-5600](https://www.idealo.de/preisvergleich/OffersOfProduct/203077469_-32gb-ddr5-5600-cl46-ct32g56c46s5-crucial.html)    | 4      | 1280      |
| Power        | [be quiet! Straight Power 12 1200W](https://www.idealo.de/preisvergleich/OffersOfProduct/202925369_-straight-power-12-1200w-be-quiet.html) | 1      | 200       |
| Storage      | [Samsung 9100 Pro 2TB](https://www.idealo.de/preisvergleich/OffersOfProduct/206077447_-9100-pro-2tb-samsung.html)                          | 1      | 360       |
| CPU Cooling  | [Noctua NH-D12L](https://www.idealo.de/preisvergleich/OffersOfProduct/201943702_-nh-d12l-beige-brown-noctua.html)                          | 1      | 110       |
| Case         | [Inter-Tech IPC 4U K-439L](https://www.idealo.de/preisvergleich/OffersOfProduct/203250046_-ipc-4u-k-439l-inter-tech.html)                  | 1      | 105       |
| Case Cooling | [Noctua NF-A8 PWM](https://www.idealo.de/preisvergleich/OffersOfProduct/4706613_-nf-a8-pwm-80mm-noctua.html)                               | 2      | 38        |

> ~ 8.500 €

#### Config 02

| Bauteil      | Detail                                                                                                                                              | Anzahl | Preis (€) |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | --------- |
| Mainboard    | [ASUS ProArt X870E-Creator WiFi](https://www.idealo.de/preisvergleich/OffersOfProduct/204861254_-proart-x870e-creator-wifi-asus.html)               | 1      | 420       |
| CPU          | [AMD Ryzen 9 9950X](https://www.idealo.de/preisvergleich/OffersOfProduct/204581108_-ryzen-9-9950x-boxed-amd.html)                                   | 1      | 480       |
| GPU          | [NVIDIA RTX PRO 4500 Blackwell](https://www.idealo.de/preisvergleich/OffersOfProduct/207327484_-rtx-pro-4500-blackwell-nvidia.html)                 | 2      | 5500      |
| RAM          | [Crucial 64GB DDR5-5600 CL46](https://www.idealo.de/preisvergleich/OffersOfProduct/206040262_-128gb-kit-ddr5-5600-cl46-ct2k64g56c46s5-crucial.html) | 2      | 1300      |
| Power        | [be quiet! Straight Power 12 1200W](https://www.idealo.de/preisvergleich/OffersOfProduct/202925369_-straight-power-12-1200w-be-quiet.html)          | 1      | 200       |
| Storage      | [Samsung 9100 Pro 2TB](https://www.idealo.de/preisvergleich/OffersOfProduct/206077447_-9100-pro-2tb-samsung.html)                                   | 1      | 360       |
| CPU Cooling  | [ARCTIC Liquid Freezer III Pro 360](https://www.idealo.de/preisvergleich/OffersOfProduct/206182027_-liquid-freezer-iii-pro-360-arctic-cooling.html) | 1      | 100       |
| Case         | [SilverStone RM44 (4U Rackmount)](https://www.idealo.de/preisvergleich/OffersOfProduct/202326252_-rm44-silverstone-technology.html)                 | 1      | 400       |
| Case Cooling | [Noctua NF-A12x25 PWM, 120mm](https://www.idealo.de/preisvergleich/OffersOfProduct/6153280_-nf-a12x25-pwm-120mm-noctua.html)                        | 3      | 60        |

> ~ 8.900 €

#### Config 03

> [NVIDIA DGX Spark](https://www.nvidia.com/de-de/products/workstations/dgx-spark/)

[`Aktuelles Angebot`](https://www.idealo.de/preisvergleich/OffersOfProduct/208146353_-dgx-spark-founders-edition-940-54242-0005-000-nvidia.html)

Es handelt sich hierbei um spezialisierte KI Hardware auf der eine NVIDIA spezifische Linux Distro vorinstalliert ist. Es stehen ca. ~ 96 GB VRAM pro Einheit zur Verfügung allerdings ist der Datendurchsatz geringer (273 GB/s vs 1,000 GB/s) als bei herkömmlichen Gaming GPUs von NVIDIA. Die gesamte STT -> LLM -> TTS Pipeline sollte in der Theorie in diesem Rahmen lauffähig sein.

> ~ 4.900 €

Im Rahmen des Budgets könnten 2 Einheiten angeschafft werde. Diese sind auch über das k.ai Projekt hinaus für Finetuning und andere interaktive Projekte die auf lokale KI setzen möchten im Rahmen von Creative Technologies sehr interessant.

Nachteile:

- Unreal benötigt separate Recheneinheit aufgrund von Hardware-Inkompatibilität
- Die Inferenz insgesamt ist langsamer als auf Gaming Hardware (Datendurchsatz)