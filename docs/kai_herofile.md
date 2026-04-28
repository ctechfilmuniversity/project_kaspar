# k.ai
## Working Document

**Authors:** Philip Gerdes, Malte Hillebrand, Lena Gieseke  
**Last updated:** 22.04.2026  

## Change History

| Date | Change | Author |
| ---- | ------ | ------ |
|      |        |        |


---

* [k.ai](#kai)
    * [Working Document](#working-document)
    * [Change History](#change-history)
* [Overview](#overview)
    * [What is k.ai?](#what-is-kai)
    * [Vision \& Goals](#vision--goals)
    * [Usage Scenarios](#usage-scenarios)
        * [Live Rehearsal Improvisation](#live-rehearsal-improvisation)
        * [Optional Scenarios](#optional-scenarios)
    * [Behavioral Models](#behavioral-models)
* [System Architecture](#system-architecture)
    * [Input / Output](#input--output)
    * [Overview Steps](#overview-steps)
    * [Modularisierung](#modularisierung)
    * [Components](#components)
        * [Preprocessing](#preprocessing)
        * [Orchestration](#orchestration)
        * [STT Module](#stt-module)
        * [LLM Module](#llm-module)
            * [LLM Benchmarking Setup](#llm-benchmarking-setup)
        * [TTS Module](#tts-module)
        * [Visual Avatar Layer](#visual-avatar-layer)
        * [Scene Storage \& File Management](#scene-storage--file-management)
    * [Design Decisions](#design-decisions)
* [Work In Progress Tracking](#work-in-progress-tracking)
        * [Active Tasks](#active-tasks)
        * [Backlog](#backlog)
        * [Done](#done)
    * [Open Questions](#open-questions)
    * [Risks](#risks)
    * [Glossary](#glossary)
    * [References \& Links](#references--links)


# Overview

## What is k.ai?

K.ai is a real-time generative AI system built for theatrical rehearsal and improvisation. It serves as an interactive, non-human scene partner for actors. The system perceives audio and video from the rehearsal room, processes these signals through a multimodal pipeline, and responds with synthesized speech and a moving visual avatar, displayed, e.g. on a screen or projected. K.ai is not a simulated actor but a deliberately distorted entity and its errors and non-human deviations are the artistic material, not defects to be corrected. K.ai's specific behavior, and appearance can be shaped and mutated by the humans during rehearsal.

K.ai is developed in the context the artistic-scientific research project [Kaspar 2028 - AI as a Theatrical Toolbox](https://www.kulturstiftung-des-bundes.de/en/programmes_projects/image_and_space/detail/kaspar_2028.html), funded by the German Federal Cultural Foundation in the [Art & AI](https://www.kulturstiftung-des-bundes.de/en/programmes_projects/film_and_new_media/detail/kunst_und_ki.html). Hand in hand with the experimentation with K.ai, we are going to produce a repertoire-ready stage production of Peter Handke's *Kaspar* at the Residenztheater in Munich, in May 2028.

## Vision & Goals

The K.ai system should include the following features:

* Live improvisation between a human performer and an algorithmic avatar in the rehearsal room
* Experimentation with the progressiv distortion of the avatar into something alien and unstable
* A pipeline that creates a digital double of a performer, to a degeree recognizable in appearance and voice, to be used as avatar
* Production of recordings of K.ai's performance for re-use
* Operability without engineering backgrounds
* A modular and extensible setup
* Open source release for use by theatre and art practitioners


K.ai is explicitly NOT:  
* A general-purpose AI-system trained on human performance
* A simulated actor substituting a human performer
* A chatbot-style dramaturgy assistant
* A tool for script analysis, play interpretation, or general Q&A
* A system aiming for human-likeness — the goal is productive distortion, not realism
* A replacement for human creative development



## Usage Scenarios

### Live Rehearsal Improvisation

* One performer enters a defined rehearsal space. K.ai, meaning the algorithmic avatar, is active. The performer speaks and moves and the system perceives the audio and video stream, processes input through its pipeline, and responds in real time.  
* Human creatives can pre-configure the avatar's personality, behavioral rules, or visual mutation parameters or adjust them on the fly via an interface. 
* Scenes are recorded (TODO: what is exactly recorded?).

### Optional Scenarios

* Scripted performance recording: A scene is pre-scripted an can be loaded in K.ai. Its performance is recorded for later playback.
* TODO: Something along the lines of sequencing?


## Behavioral Models

Personality bundles
* Predefined sets of behaviors (personality, mood, movement style, vocal character) that can be selected and switched at runtime. 
* Examples: animalistic movement patterns, body-horror aesthetics, Stanislavski-style tempo-rhythm modulation.

Input-output relations
* Mappings from perceived sensor signals to output behavior. 
* Example: "the less the scene partner says, the more destroyed K.ai becomes". 
* These relations are pre-defined by the creative team.

Temporal design
* Response delay as an expressive parameter, inspired by Stanislavski's concept of tempo-rhythm.

Mutation trajectory
* K.ai starts with 1:1 mirroring of input and progressively diverges. Textual and visual instability is a core aesthetic goal.




# System Architecture

## Input / Output 


| Input Type              | Description                                                             |
| ----------------------- | ----------------------------------------------------------------------- |
| Audio stream            | Live microphone capture from the rehearsal room                         |
| Video stream            | Camera feed for motion/face tracking and visual context                 |
| Prompts / Configuration | Plain text or YAML files defining personality, behavioral rules, scenes |
| Controller signals      | Real-time parameter adjustments from the operator (sliders, patches)    |
| Stored scenes           | Previously recorded K.ai outputs, re-injectable as context or material  |

The system should support both **direct input** (explicit speech directed at K.ai) and **indirect / co-presence input** (movement, proximity, posture) as triggers. 

| Output            | Format            | Destination                                    |
| ----------------- | ----------------- | ---------------------------------------------- |
| Synthesized voice | Audio stream      | Speakers in rehearsal room or theatre          |
| Visual avatar     | Video             | Display / Projection / LED wall                |
| Scene recordings  | File (format TBD) | Archive for re-use on stage or as future input |

## Overview Steps

TODO:

| Step          | Description                                                                                  |
| ------------- | -------------------------------------------------------------------------------------------- |
| Preprocessing | Frame extraction from video stream; audio chunking                                           |
| STT           | Speech-to-text transcription (Whisper / faster-whisper)                                      |
| LLM           | Text processing and response generation (Gemma 4) with personality/behavior prompts injected |
| TTS           | Speech synthesis in K.ai's cloned voice (Fish Audio S2 Pro)                                  |
| Visual Avatar | MetaHuman rig driven by motion capture + distortion layer + optional real-time diffusion     |
| Output        | Rendered avatar + synthesized audio to physical representation (screen / projection)         |



<img height="420" src="./img/overview.excalidraw.svg" class="pad">


## Modularisierung

<img height="620" src="./img/modularization.excalidraw.svg" class="pad">


* Kürzeste Latenz
    * 1 spezialisierte Workstation mit mehreren dedizierten GPUs für versch. Module
    * Datenaustausch durch CUDA IPC (GPU-to-GPU) oder Shared Memory
    * Höchste Kosten & höchste Komplexität (nicht umsetzbar)
* Größte Flexibilität
    * Jedes Segment als Server mit `openai api` kompatiblem Endpunkt
    * IPC über lokales Netzwerk (zB.: Websockets)
    * gutes Mapping auf vorhandene Hardware (Verteilung auf mehrere Workstations)
    * nahtloses Fallback auf gehostete APIs (zB.: OpenAI Realtime API)


Orchestration across all segments is handled by **LiveKit Agents**.

Each segment runs as an independent server. Inter-process communication via local network (WebSockets). This allows flexible distribution across multiple workstations and seamless fallback to hosted APIs (e.g. OpenAI Realtime API).


TODO:

## Components

Overview Tech Stack

| Layer             | Technology                                       | Notes                                    |
| ----------------- | ------------------------------------------------ | ---------------------------------------- |
| Orchestration     | LiveKit Agents                                   | STT→LLM→TTS pipeline management          |
| STT               | Whisper / faster-whisper                         | May be superseded by Gemma 4 audio input |
| LLM               | Gemma 4 via vLLM / ollama                        | Local inference, Apache 2.0              |
| TTS               | Fish Audio S2 Pro                                | License TBD                              |
| Avatar engine     | Unreal Engine + MetaHuman                        | LiveLink, FaceBuilder (paid)             |
| Avatar distortion | Vertex shaders, bone manipulation, ComfyUI Spout | Real-time Stable Diffusion on UE output  |
| Video sharing     | Spout + UE Spout Plugin                          | Windows GPU buffer sharing               |
| Prompt config     | Plain text / YAML                                | Editable by non-technical creatives      |
| OS                | Linux (preferred) / Windows                      | See open questions                       |
| Hardware          | Multiple workstations with dedicated GPUs        | One per segment                          |


### Preprocessing

* Frame extraction from video stream
* Audio chunking for STT input
* Future: additional sensing signals (motion data, proximity, tracking features for co-presence input)

### Orchestration

**LiveKit Agents** manages the STT → LLM → TTS pipeline. Chosen for its native compatibility with this architecture and seamless OpenAI API compatibility.

> [LiveKit Agents](https://github.com/livekit/agents)

- spezifisch entwickelt für STT → LMM → TTS Pipelines
- nahtlose Kompatibilität mit `openai api`


### STT Module

- **Whisper** (via `faster-whisper` or `whisper-flow`)
- May become obsolete if Gemma 4's native audio input is used directly — to be evaluated

> [Whisper](https://openai.com/de-DE/index/whisper/)

- [`faster-whisper`](https://github.com/SYSTRAN/faster-whisper)
- [`whisper-flow`](https://github.com/dimastatz/whisper-flow)
- Möglicherweise obsolet wenn Gemma 4 Audio Input genutzt werden kann


### LLM Module

**Gemma 4** — selected for:
- Multimodal input (audio/vision directly into the model)
- Local inference optimizations (TurboQuant, per-layer embeddings)
- Apache 2.0 license
- Available via vLLM and ollama (llama.cpp)

Personality and behavior are controlled via prompt configuration (plain text / YAML), designed to be modifiable by non-technical creatives during rehearsal.


> [Gemma 4](https://huggingface.co/collections/google/gemma-4)

- Multimodaler Input direkt in das LLM
- Spezifische Optimierungen für lokale Inferenz
  - [TurboQuant](https://research.google/blog/turboquant-redefining-ai-efficiency-with-extreme-compression/)
  - [Per-Layer Embeddings](https://newsletter.maartengrootendorst.com/i/193064129/per-layer-embeddings)
- Apache 2.0 Lizenz
- Verfügbar in `vLLM` und `ollama (llama.cpp)`

  
Linux ?
- Deutlich besserer Support für Inferenz-Provider (`SGLang` & `vLLM`)
- Mangelnde Erfahrung mit WSL oder Docker bzw. Linux allgemein (Philip)




#### LLM Benchmarking Setup

> Alle Benchmarks wurden innerhalb der folgenden Rahmen-Parameter durchgeführt

- Single GPU → `NVIDIA GeForce RTX 4090 (24 GB)`
- OS → `WSL (Ubuntu Distro)`
- Python Environment Management → `uv`
- Referenz-Datensatz: `FreedomIntelligence/sharegpt-deutsch`

Time to First Token (TTFT)  

| N = 10 queries   | vLLM    | SGLang |
| ---------------- | ------- | ------ |
| Mean TTFT (ms)   | 125.68  | 66.53  |
| Median TTFT (ms) | 84.61   | 70.08  |
| P99 TTFT (ms)    | 427.94* | 84.02  |

| N = 6200 queries | vLLM  | SGLang |
| ---------------- | ----- | ------ |
| Mean TTFT (ms)   | 34.57 | 48.74  |
| Median TTFT (ms) | 29.84 | 42.73  |
| P99 TTFT (ms)    | 79.27 | 153.23 |

Time per Output Token   (TPOT)  

| N = 10 queries   | vLLM  | SGLang |
| ---------------- | ----- | ------ |
| Mean TPOT (ms)   | 14.30 | 12.19  |
| Median TPOT (ms) | 14.27 | 12.23  |
| P99 TPOT (ms)    | 14.45 | 12.27  |

| N = 6200 queries | vLLM  | SGLang |
| ---------------- | ----- | ------ |
| Mean TPOT (ms)   | 14.32 | 12.17  |
| Median TPOT (ms) | 14.00 | 12.11  |
| P99 TPOT (ms)    | 20.80 | 12.85  |


Ergebnisse   

* TTFT ausschlaggebend für K.ai (Latenzminimierung)
  * Fokus auf N = 6200 Anfragen
    * ggf. Fehler bei der Messung mit N = 10 [*Ausreißer auf Folie 3]
    * entspricht längerer Laufzeit dh. fakturiert ggf. Ermüdungserscheinungen durch Temperaturentwicklung mit ein
* Durchsatz (TPOT) zwar `~ 6 ms` schneller bei `SGLang`
* ABER erstes Token (Begin der Antwort) `~ 52 ms` schneller bei `vLLM`




### TTS Module 

**Fish Audio S2 Pro** — voice cloning and synthesis.
License currently unclear (research-use only), we asked about it but no response.

> [Fish Audio S2](https://github.com/fishaudio/fish-speech)

- Unklare Lizenzierung (Nur zu Forschungszwecken) ! https://github.com/fishaudio/fish-speech/blob/main/LICENSE




### Visual Avatar Layer

Pipeline: Input → MetaHuman (Unreal Engine) → Output  

Input:
* Motion capture via LiveLink in Unreal Engine
* Face mesh via FaceBuilder by KeenTools (paid)

Distortion layer (applied after animation):  
1. Bone transform manipulation of the rig
2. Scrambled LiveLink association (e.g. left eye controls right arm)
3. Vertex shader deformations

Output layer:  
* **Spout** for GPU video buffer sharing between Windows applications
* **UE Spout Plugin** captures Scene Capture 2D from Unreal Engine
* **ComfyUI Spout Nodes** enable real-time Stable Diffusion generation on the UE output image

The distortion degree is dynamically controllable.

####Controller Interface

A real-time operator-facing interface for:
* Sliders controlling body/voice posture along one or more dimensions
* Activatable behavioral patches (on/off, or on/off + additional parameter)

Example patches:
* K.ai imitates the performer's movement in a mocking rather than exact-mirroring way
* K.ai's skull becomes visible through its skin at a dynamically controllable ratio

### Scene Storage & File Management

All K.ai scenes are recorded. Stored scenes can be re-injected as input ("take scene X and modify Y").


## Design Decisions 

| Decision | Options Considered | Chosen Approach | Reasoning | Date |
| -------- | ------------------ | --------------- | --------- | ---- |
|          |                    |                 |           |      |




# Work In Progress Tracking

### Active Tasks

* Initial architecture validation: STT → LLM → TTS pipeline with LiveKit Agents
* LLM module: Gemma 4 local inference setup (vLLM / ollama)
* Avatar layer: MetaHuman rig + LiveLink + distortion prototype

### Backlog
*Features and tasks queued but not yet started.*

| Item | Priority | Owner | Notes |
| ---- | -------- | ----- | ----- |
|      |          |       |       |

### Done
*Completed items worth recording for context.*

---

## Open Questions

| Question                                                | Context                                                                                                        | Owner  | Status |
| ------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- | ------ | ------ |
| Fish Audio S2 Pro license                               | Currently "research use only" — incompatible with open-source release goal. Need alternative or clarification. |        | WIP    |
| Does Gemma 4 audio input make Whisper obsolete?         | If multimodal audio input works reliably, STT module may be droppable. Depends on latency and output quality.  |        | Open   |
| Linux vs. Windows                                       | Linux gives better inference support (SGLang, vLLM). Team has limited Linux/WSL/Docker experience.             | Philip | Open   |
| Is prompting sufficient for controlling the system?     | Possibly yes. Needs testing during rehearsal.                                                                  |        | Open   |
| How to define indirect / co-presence input technically? | Central artistic and engineering question. What signals, at what granularity, map to what behaviors?           |        | Open   |
| Direct vs. indirect input tension                       | When does K.ai respond to speech vs. presence vs. movement?                                                    |        | Open   |
| Ethical LLM (kl3m)                                      | Grant application cites kl3m as "fair data" candidate. Compatible with latency and multimodal requirements?    |        | Open   |
| Post-production data ownership                          | What happens to the performer's digital clone after the production closes? Legal and ethical.                  |        | Open   |
| FaceBuilder licensing cost                              | Paid tool                                                                                                      |        | Open   |



---

## Risks


| Risk                                                         | Likelihood | Impact | Mitigation                                                                      |
| ------------------------------------------------------------ | ---------- | ------ | ------------------------------------------------------------------------------- |
| Latency too high for live improvisation                      | High       | High   | Local inference; GPU-per-module; minimize network hops; fallback to hosted APIs |
| Fish Audio license incompatible with open-source release     | Medium     | High   | Identify TTS alternative early                                                  |
| LLM output too coherent / "normal"                           | Medium     | High   | Aggressive prompt engineering; behavioral mutation layers                       |
| Team unfamiliarity with Linux                                | Medium     | Medium | Evaluate WSL/Docker; Windows as fallback with performance trade-off             |
| Hardware insufficient for real-time multi-segment pipeline   | Medium     | High   | Profile early; design fallback to hosted APIs                                   |
| Non-technical creatives unable to modify prompts effectively | Medium     | Medium | Invest in prompt interface tooling and documentation                            |

---

## Glossary
*Domain-specific or project-specific terms defined for external readers.*


| Term               | Definition                                                                                                  |
| ------------------ | ----------------------------------------------------------------------------------------------------------- |
| K.ai / Kaspar.ai   | The AI system developed for Kaspar 2028                                                                     |
| STT                | Speech-to-text: converts spoken audio to text                                                               |
| LLM                | Large language model: generates text responses                                                              |
| TTS                | Text-to-speech: synthesizes spoken audio from text                                                          |
| RAG                | Retrieval-Augmented Generation: extends LLM context with retrieved documents                                |
| MetaHuman          | Unreal Engine tool for creating high-fidelity digital humans                                                |
| LiveLink           | Unreal Engine protocol for streaming real-time animation data                                               |
| Spout              | Windows framework for sharing GPU video buffers between applications                                        |
| Virtual Production | Real-time CGI technique for filmmaking                                                                      |
| Co-presence input  | Indirect input from the performer's physical presence (movement, proximity, posture) as opposed to explicit |
| Brain damage       | Working metaphor for the system: deliberately broken or scrambled to produce alien, non-human behavior      |


---

## References & Links

TODO:
