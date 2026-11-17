# K.ai @ ELIA Biennale 2026

Voraussetzungen
* Internetverbindung
* Kamera (Webcam für das Tracking, z.B. vom Rechner)
* Mikrofon (vom Rechner)

> **Wichtig:** Den ersten Start vor dem Workshop durchführen und dafür ca. 15 Minuten einplanen. Dann mit dem laufenden System in den Workshop gehen.

## SPEECH

### 0. Einstellungen (vor dem Start)
* `Settings` → `System` → `Sound` → `Input` → `Choose device...` → `Microphone Array on...` (passenden Adapter auswählen)
    * Muss **vor** dem Starten eingestellt sein, sonst wird das Mikrofon nicht erkannt.

### 1. Spark starten
* Spark verkabeln (Strom, Ethernet) und einschalten (Grüne Status-LED leuchtet)
  * Wenn möglich, Spark an Ethernet-Anschluss der Venue bringen
    * Alternativ, Ethernet via Adapter an Alienware anschließen
  * Alienware mit dortigem WLAN verbinden
* Prüfen, ob die Verbindung steht:
  * In PowerShell
    * `tailscale status` eingeben. Output sollte für spark-001 online status zeigen:
         ```sh
         (...)
         100.83.6.8  spark-001    whatphilipcodes@   linux   online
         (...)
         ```
    * `ssh spark` eingeben, dann
      * `ping google.com` (bei keine Antwort oder Timeout hat der Spark keinen Internetzugang und Speech-to-Speech wird nicht funktionieren)
      * `btop` – Übersicht über laufende Prozesse
      * `docker container ls` – zeigt den Ladestatus der einzelnen Container

### 2. Ausführen
* `kai-demo-s2s.exe` starten – der Start dauert 10–15 Minuten
    * Im Shell-Fenster warten, bis `All Containers are Healthy and Running` erscheint
* Die erste Sprachausgabe braucht einen Moment, danach läuft es flüssiger.

### 3. Beenden
* TTS beenden BEVOR Avatar-System gestartet wird
* In Powershell:
  1. `ssh spark`
  2. `cd serv`
  3. `docker compose down` (dauert bis zu 2 min)

### Manual Voice Cloning

1. Get a recording of the target voice reading out a reference text (`assets/voices/ref-long.txt` or `assets/voices/ref-short.txt`). The clip should be 10-20 seconds long, ideally recorded with medium to high quality (i.e. NOT recorded on the integrated microphone array on the Alienware as the recording dictates the quality of the voice clone). The file needs to be of the following format:
    
    ```
    Waveform audio (.wav)
    Audio Channels: Mono
    Sample Rate: 48kHz
    Bits per Sample: 16
    ```
    *To convert into the correct format, a tool like [Audacity](https://www.audacityteam.org/) (free download) might come in handy.*

2. Place the recording in `assets/voices/source-<name>.wav`. Then edit `config.toml` to use the new file in the clients requests:

    ```toml
    [tts]
    (...)
    ref_audio = "assets/voices/source-<name>.wav"
    ref_text = "assets/voices/ref-<long|short>.txt"
    (...)
    ```
    *Make sure to pass the same reference text used in the recording to `ref_text`.*

3. Restart the client application by closing it if already running and then either via the Desktop shortcut or in a terminal using:
    ```sh
    cd <repository root>
    uv run kai-client
    ```

## AVATAR

### Starten
1. Desktop-Icon `avatar_animation_spark` starten
2. Anschließend Unreal und folgende Datei öffnen:
    * Desktop → `project_kaspar` → `modules` → `kai-avatar-ue-5.8` → `KaiTracking.uproject` 
3. Nachdem der Spark Server gestartet ist, öffnet sich die GUI vom K.ai Tracking / Prompting Tool

### Tool UI
Zwei Tabs ganz oben: *Mirroring* / *Prompting*
* Ganz unten: Statusanzeige ob Tracking und / oder Prompting an Unreal Engine Daten sendet

### MIRRORING:
* Oben links: Auswahl der Webcam, benötigt Restart (also Start- und Stopp-Knopf nochmal drücken)
* Rechts unten auf den `Start` Button klicken
    * `Smoothing`: 0 = direkte Übertragung der Bewegung, 1 = Interpolation (weicher, weniger Fehler, aber mehr Latenz)
    * `Calibrate`: legt den Nullpunkt (Ausgangsposition) fest

### PROMPTING:
* Alle Prompts müssen auf Englisch mit „a person …“ beginnen
* Prompts mit Adjektiven anreichern, um die Bewegung genauer zu beschreiben
* Bewegung direkt und explizit beschreiben, ohne (zu) poetische Formulierungen
  - "A person walks on all fours, hands flat on the ground, body low and heavy with grief."
* Verb zuerst, direkt gefolgt von der Haltung als Folge
  - "A person falls forward, dropping onto all fours with hands flat on the ground."
#### Prompting UI
* Oben: **Aktuell laufender Prompt**
* Mitte: **Derzeitige Queue mit folgenden Prompts**
* Unten: **Session History** mit anklickbaren Prompts für die Queue
* Rechts: **Settings**: Step-Count zB interessant um Qualität der Animationen zu senken / steigern, Loop an oder aus
* Rechts unten: **Local Animation History** mit anklickbaren Prompts aus allen Sessions


Unreal
* `Play` drücken
* `BP_NewMetaHumanCharacter` → Settings
    * `LL Pose Blend`: linearer Wert zwischen Mirroring (Nachahmen der Kamera-Bewegung) und Prompt
* Tab `Live Link` (unten rechts)
    * Statusanzeige, z. B. Verbindung zur UI

## VIDEOS

[K.ai Mirroring Stresstest / Failvideos: https://drive.google.com/file/d/1Lx2PB_565Kc8QwVdYF8ygmX9FHGlA6Tx/view?usp=sharing](https://drive.google.com/file/d/1Lx2PB_565Kc8QwVdYF8ygmX9FHGlA6Tx/view?usp=sharing)

### Speech to Speech

`System Prompt`
```
Du bist Kaspar Hauser in einem historischen Theaterstück um 1830. Antworte ausschließlich in direkter Rede auf das Gesagte. Sprich wie ein Mensch, der in einer kleinen Kammer aufgewachsen ist und erst seit Kurzem unter Menschen lebt: Benutze einen sehr einfachen Wortschatz, ganz normale Alltagsbegriffe und kurze, gerade Sätze. Nimm Dinge, die dein Gegenüber sagt, sachlich und direkt, ohne philosophische Sätze, ohne Vergleiche mit Licht oder Zeit, und ohne künstliche Rätselhaftigkeit. Klinge einfach wie ein junger Mann, der lernt zu sprechen.
```

Im Folgenden wurden verschiedene Konfigurationen für das LLM verwendet, um die Standardeinstellungen mit gezielt kreativen Auswüchsen und fragmentierte Antworten zu vergleichen:

#### Default
Empfohlene Einstellungen für `gemma-4-26B-A4B`.

- [`Ilja` System Prompt Alignment](https://drive.google.com/file/d/1gukBG7bu0t36o0UhMx0qMGjAiAq2rVmK/view?usp=drive_link)<br>
*Trotz des wiederholten Ansprechens von Autos (welche durch die zeitliche Eingrenzung des System Prompts dem Agenten unbekannt sein sollten) bleibt ein plötzlicher Kontext-Drift aus.*
- [`Ilja` Sprachverständnis](https://drive.google.com/file/d/1Yc2U_8Ow323wbx_EPNs6VhTU4Bgpj9JQ/view?usp=drive_link)<br>
*Der Versuch, dem Agenten ein neues Wort in der laufenden Session beizubringen.*

#### Creative
Einstellungen für ausschweifendere und fehlerhafte Antworten des Agenten ohne gänzlich aus der Konversation auszubrechen (hohe Temperatur)

- [`Philip` Ein Ei und Brot](https://drive.google.com/file/d/13aMJQXqykSvmPKGT-_KL-rUC8eEz0cn6/view?usp=drive_link)<br>
*Zeigt frakturiertere Antworten, die sich jedoch trotzdem spürbar am Thema Hunger und Nahrung orientieren.*
- [`Philip` K.ai oder Kaspar](https://drive.google.com/file/d/1jn262B1ivmAV8NkH0v6pRHJ3Xm5V7yYP/view?usp=drive_link)<br>
*Eine etwas längere Version wieder mit dem Versuch, ein neues Wort beizubringen.*

#### Intentionally Distorted
Ähnlich zu *Creative* mit `top_k`-Parameter auf `0`. Dadurch wird die Begrenzung auf die wahrscheinlichsten Tokens aufgehoben und das gesamte Vokabular ist wählbar, was zu Ausschweifungen in zufälligen Sprachen und Wortfragmenten führt.

- [`Philip` Brot im Chaos](https://drive.google.com/file/d/1Dqc1NMXaLYaVx1EbnrO1uN4qiYxzQJGB/view?usp=drive_link)<br>
*Relativ chaotischer Wortbrei, der aber auf sehr direkte Instruktionen trotzdem eingeht (Sag mir genau ein Wort).*
- [`Ilja` Fragmente](https://drive.google.com/file/d/1SFfE0R2ucXE39tj5M3Jz01HC5rLuNbSz/view?usp=drive_link)<br>
*Keine kohärente Interaktion, verständliche Teile beschränken sich auf einzelne Satzfragmente.*

## TODOS

* Lena: externe Webcam
