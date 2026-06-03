---
layout: default
title: Live Rehearsal Improvisation
nav_order: 2
has_children: true
---

#### k.ai Developments

# System Architecture - 1. Live Rehearsal Improvisation 

> Aktueller Stand

| module               | state                | issue                                        | tested                           | to-do             |
| -------------------- | -------------------- | -------------------------------------------- | -------------------------------- | ----------------- |
| kai-core             | incomplete           | missing TTS (hardware constraints)           | omni conversation (high latency) | streaming, tts    |
| kai-llm              | completed (prebuilt) | none                                         | api interface                    | awaiting hardware |
| kai-tts              | incomplete           | missing server component                     | text input demo                  | server interface  |
| kai-avatar-animation | incomplete           | missing lang pipeline (hardware constraints) | linux-windows comm / api access  | awaiting hardware |