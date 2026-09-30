# Kai Development Milestones

## January Seminar and Event

### Overall


* Shifting the architecture towards more flexibility by utilizing docker images hosting APIs orchestrated by a client application (Philip)


### 2026-10-01

LLM Backend API endpoint running (Philip)
- [ ] vLLM hosting LLM backbone
- [ ] custom optimized docker image for reproducibility (open source)

Mirrored and procedurally generated animations in Unreal Engine (Malte)
- [x] Drive Manny skeleton via LiveLink
- [x] Retarget Manny skeleton to MetaHuman
- [x] Construct rig from webcam subject via Google MediaPipe
- [x] additionally drive MediaPipe rig via NVIDIA Kimodo

### 2026-10-15

ASR (STT) + TSS endpoints running (Philip)
- [ ] ASR architecture set (in-LLM vs separate module)
- [ ] ASR endpoint running
- [ ] TTS endpoint running

Unified GUI for sending and recieving data to external machine used to generate motion capture data via Kimodo (Malte)
- [ ] Establish remote connection and stream data via tailscale
- [ ] Send data to Unreal Engine simultanously, establish two LiveLink subjects

Simple distortion controller in UE (Malte)
- [ ] Blend between mirrored and generated motion
- [ ] Distort rig via ControlRig, data distortion and / or vertex shader

Improve motion (Malte)
- [ ] Mirroring: X Y Position translation, optionally locked in place
- [ ] Mirroring: Test infrared footage
- [ ] Kimodo: Loop generation, idle positions -> inifinite data stream / ALTERNATIVELY: check Ardy state for usability and potentially switch

### 2026-11-01

Client Application running (Philip)
- [ ] Endpoint Orchestration
- [ ] Demo Setup (speech to speech)

Cloning (Malte)
- [ ] Automate as much of MetaHuman creation process as possible
- [ ] Look into alternatives to MetaHumans: Generate 3D scan and rig via AI ?
- [ ] Utilize webcam to automate cloning process -> Idea: Take pictures as "masks" / facescan, use MediaPipe to get arm lengths etc
- [ ] Sketch setup process -> one wrapper programm that starts UE in background, fires up the motion GUI etc.

### 2026-11-15

Full pipeline tests (Philip)
- [ ] Realtime Goal Eval (compute resources sufficient?)

Marry both pipelines (Malte)
- [ ] Drive face from TTS
- [ ] Generate motion from LLM
- [ ] Orchestrate distortions from LLM / GUI -> "Temperature" slider to change AI involvement

### 2026-12-01

Finetuning (Malte)
- [ ] Improve mirroring
- [ ] Improve prompting for motion generation
- [ ] Improve distortion layer
- [ ] Test and iterate on the cloning process

Switch to real-world application (Malte)
- [ ] Plan actual setup in workshops
- [ ] Test connectivity and data transfer
- [ ] Plan data collection methods and legal stuff

### 2026-12-15

Bugfixing (Malte)
- [ ] Mirroring
- [ ] Motion Generation
- [ ] Unreal Engine
- [ ] GUIs

Build the actual setup (Malte)
- [ ] Monitor
- [ ] Camera
- [ ] HCI
- [ ] Data collection

### 2027-01-01

Test the workshop setup (Malte)
- [ ] Data transfer
- [ ] Data collection
- [ ] Hardware
- [ ] Acessibility