# K.ai @ ELIA Biennale 2026

Voraussetzungen
* Internetverbindung
* Kamera (Webcam für das Tracking, z.B. vom Rechner)
* Mikrofon (vom Rechner)

> **Wichtig:** Den ersten Start vor dem Workshop durchführen und dafür ca. 15 Minuten einplanen. Dann mit dem laufenden System in den Workshop gehen.

### SPEECH

Einstellungen (vor dem Start)
* `System` → `Sound` → `Input` → `Choose device...` → `Microphone Array on...` (passenden Adapter auswählen)
    * Muss **vor** dem Starten eingestellt sein, sonst wird das Mikrofon nicht erkannt.

Spark starten
* Spark verkabeln (Strom, Ethernet) und einschalten
* Prüfen, ob die Verbindung steht:
    * Desktop-Verknüpfung `Status Spark Link` öffnen (öffnet eine Webseite) → `spark-001` muss `Connected` anzeigen
    * ODER in der PowerShell `ssh spark` eingeben, dann:
        * `btop` – Übersicht über laufende Prozesse
        * `docker container ls` – zeigt den Ladestatus der einzelnen Container

Ausführen
* `kai-demi-s2s.exe` starten – der Start dauert 10–15 Minuten
    * Im Shell-Fenster warten, bis `All Containers are Healthy and Running` erscheint
* Die erste Sprachausgabe braucht einen Moment, danach läuft es flüssiger.

TTS beenden (QUESTION: muss das vor dem Start des Avatar Systems passieren?)
* `ssh spark`
* `cd serv`
* `docker compose down`

### AVATAR

Starten
* Desktop-Icon `avatar_animation_spark` starten
* Anschließend Unreal und folgende Datei öffnen:
    * Desktop → `project_kaspar` → `KaiTracking`
* Die UI öffnet sich

UI
* Rechts unten auf den `Start` Button klicken
    * `Smoothing`: 0 = direkte Übertragung der Bewegung, 1 = Interpolation (weicher, weniger Fehler, aber mehr Latenz)
    * `Calibrate`: legt den Nullpunkt (Ausgangsposition) fest
* Alle Prompts müssen auf Englisch mit „a person …“ beginnen

Empfehlungen
* Prompts mit Adjektiven anreichern, um die Bewegung genauer zu beschreiben

Unreal
* `Play` drücken
* `BP_NewMetaHumanCharacter` → Settings
    * `LL Pose Blend`: linearer Wert zwischen Mirroring (Nachahmen der Kamera-Bewegung) und Prompt
* Tab `Live Link` (unten rechts)
    * Statusanzeige, z. B. Verbindung zur UI

## TODOS

* Philip: How-to für Lena, wie man eine andere Stimme verwenden kann, inkl. Aufnahme vor Ort
* Philip: Audio-Beispiele für Misuse
* Lena: externe Webcam
