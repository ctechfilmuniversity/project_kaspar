---
layout: default
title: Layer STT
parent: Live Rehearsal Improvisation
nav_order: 3
---


# STT

**Authors:** Philip Gerdes

File Change History:

| Date       | Change          | Author |
| ---------- | --------------- | ------ |
| 2026-05-04 | Aktueller Stand | Philip |

### Multimodalität von Gemma 4

*To Do*: Kompatibilität & Funktionalität in vLLM testen

Als multimodales Modell können bei Gemma 4 Audio Snippets direkt eingespeist werden. Inwieweit sich das gerade im Hinblick auf Genauigkeit und Latenz auswirkt muss noch getestet werden.


### Fallback Alternative: Voxtral

[`mistralai/Voxtral-Mini-4B-Realtime-2602`](https://huggingface.co/mistralai/Voxtral-Mini-4B-Realtime-2602)

- Einstellbare Verzögerung/Qualität (`480 ms best, 240 ms - 2.4 s`)
- Unterstützt Streaming
- Apache-2 license