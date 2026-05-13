---
layout: default
title: Layer Avatar Engine
parent: Live Rehearsal Improvisation
nav_order: 7
---


# Avatar Engine

Unreal Engine's [MetaHumans](https://www.metahuman.com/) are used to quickly build, display, animate and distort virtual clones of the Actors.

The integreation deep into the UE systems makes it feasible to render high-quality avatars in real-time using UE's various systems optimized for real-time rendering in virtual production contexts.

- [Avatar Engine](#avatar-engine)
  - [File Management](#file-management)
  - [Building an Avatar](#building-an-avatar)
    - [Key Concepts \& Definitions](#key-concepts--definitions)
    - ["Standard" Method: MetaHuman Creator (MHC)](#standard-method-metahuman-creator-mhc)
    - [Advanced Methods of Cloning a Human](#advanced-methods-of-cloning-a-human)
      - [MetaHuman Animator (Live Link Face for iOS)](#metahuman-animator-live-link-face-for-ios)
      - [Photogrammetry (RealityScan / Meshroom)](#photogrammetry-realityscan--meshroom)
      - [FaceBuilder by KeenTools](#facebuilder-by-keentools)
      - [OpenCV Pose Estimation (Skeletal Scaling)](#opencv-pose-estimation-skeletal-scaling)
    - [Comparison \& Combination](#comparison--combination)
  - [Animating an Avatar](#animating-an-avatar)
    - [LiveLink](#livelink)
    - [Pre-Built Animations](#pre-built-animations)
    - [Proceudrally Generating Animations](#proceudrally-generating-animations)
      - [MetaHuman Animator: Audio to Animation](#metahuman-animator-audio-to-animation)
      - [NVIDIA Audio2Face](#nvidia-audio2face)
      - [NVIDIA Kimodo - Full-Rig Diffusion Model](#nvidia-kimodo---full-rig-diffusion-model)
      - [NVIDIA ACE (Avatar Cloud Engine)](#nvidia-ace-avatar-cloud-engine)
      - [State of the Art: DiDiffGes (2025)](#state-of-the-art-didiffges-2025)
      - [State of the Art: AsynFusion (2025)](#state-of-the-art-asynfusion-2025)
  - [Sending Data](#sending-data)
    - [Transfering Framebuffers](#transfering-framebuffers)
      - [**Spout (Windows)**](#spout-windows)
      - [**Syphon (MacOS)**](#syphon-macos)
      - [**NDI: Network Device Interface (Cross-Platform)**](#ndi-network-device-interface-cross-platform)
      - [**Native Unreal Alternative: Pixel Streaming**](#native-unreal-alternative-pixel-streaming)
    - [Animation Data](#animation-data)
  - [Distorting an Avatar](#distorting-an-avatar)
    - [Mesh Distortion](#mesh-distortion)
    - [(Vertex) Shader Distortion](#vertex-shader-distortion)
    - [Neural Style Transfer](#neural-style-transfer)
    - [Scrambled LiveLink association](#scrambled-livelink-association)
    - [Framebuffer to Image2Image Diffusion](#framebuffer-to-image2image-diffusion)

## File Management

**Authors:** Malte Hillebrand

File Change History:

| Date       | Change        | Author |
| ---------- | ------------- | ------ |
| 2026-05-04 | First Version | Malte  |

## Building an Avatar

![MetaHuman Avatar Example](../../img/avatar_metahuman_example_01.gif)

A variety of tools with varying degrees of complexity can be used to produce a MetaHuman Identity which Unreal Engine can animate. Likeness to an actor can be achieved via manual labor and comparison or a more advanced technical pipeline.

### Key Concepts & Definitions

* **MetaHuman Identity:** A specialized asset in UE that acts as a "bridge." it maps a unique 3D mesh (from a scan or photo) to the standardized MetaHuman topology
* **MetaHuman DNA:** The underlying data file that contains the specific vertex positions, rig constraints, and animation logic for a character. It ensures the avatar behaves realistically regardless of its shape
* **Mesh to MetaHuman:** The automated process within Unreal Engine that "wraps" the standard MetaHuman topology onto a custom 3D scan

### "Standard" Method: MetaHuman Creator (MHC)

* **Preset Start:** Choose from a library of diverse humans
* **Sculpting:** Manually adjust features using a sculpt tool or by blending between different preset faces

### Advanced Methods of Cloning a Human

#### MetaHuman Animator ([Live Link Face for iOS](https://apps.apple.com/us/app/live-link-face/id1495370836))
Uses the TrueDepthfront camera of  an iPhone to capture facial geometry

The actor performs a neutral pose and a profile turn in the MetaHuman Live Link app. The data is sent to UE to solve the MetaHuman Identity

👍 Extremely fast; captures the general mesh

👎 Can not transfer a texture

#### Photogrammetry ([RealityScan](https://www.realityscan.com/) / [Meshroom](https://meshroom.org/))
The most high-fidelity method, involving 50–200 high-resolution photos of the actor.

Photos are processed into a dense point cloud and then a high-poly mesh. This mesh is imported into UE as the source for a MetaHuman Identity

👍 Captures the detailled mesh; captures detailled texture

👎 Hard to automate; slow and tedious process; need to transfer the texture on the complex MetaHuman UV map (unclear, need to try)

#### [FaceBuilder](https://keentools.io/products/facebuilder-for-blender) by KeenTools

A plugin for Blender that allows for "manual photogrammetry" from a limited set of photos

Align a base head model to 2D photos by placing "pins" on features (nose, eyes, lips)

👍 Great for when you cannot perform a live scan and only have reference photography; produces a very clean mesh that is easy for the MetaHuman solver to process

👎 Manual labor, time intense

#### OpenCV Pose Estimation (Skeletal Scaling)

OpenCV can be used to match the body proportions

Webcam feed of the actor is processed using OpenCV to extract specific metrics: total height, arm length, and shoulder width

Ratios are used to drive the "Body" selection in MHC or procedurally scale the skeletal bones in the UE Blueprint Construction Script

### Comparison & Combination

| Method | Accuracy | Setup Time | Complexity | Best For |
| :--- | :--- | :--- | :--- | :--- |
| **MHC Only** | Low | Low | Low | Generic background characters |
| **iOS Link** | Medium | Medium | Low | Rapid prototyping/Live rehearsal |
| **FaceBuilder** | Medium-High | High | Medium | When actor is not physically present |
| **Photogrammetry** | Ultra-High | Very High | High | Hero "Clones" for close-ups |

**Combining Tools:** In practice, **FaceBuilder / iOS link** used to get the facial structure right, **Photogrammetry** for the skin textures, and **OpenCV** to ensure the avatar's height matches the physical actor on stage

## Animating an Avatar

### LiveLink

Common interface for streaming and consuming animation data from external sources into Unreal Engine.

A variety of sources can be used to drive a variety of subjects. Some input can move a camera within the engine, a webcam feed can drive a MetaHuman's facial mesh, a MoCap suit can move a MetaHuman's skeleton

### Pre-Built Animations

By pre-building animations and interpolating between them using UE's Blendspace, complex and unique movement can be achieved.

_Problem: Pre-building lots of animations_

### Proceudrally Generating Animations

Generating rig-animations needs to balance generation time and quality.

Complex full-rig animations don't seem to be feasible in real-time yet, while face mesh animation from an audiostream seems to be doable.

#### MetaHuman Animator: Audio to Animation

Native MetaHuman plugin to process audio and build facial animations to be used to move a MetaHuan face mesh.

Works in real-time!

_Lacks a bit of "depth", mostly just mouth movement, not really emotional changes to a face (frowning etc.)_


#### [NVIDIA Audio2Face](https://www.nvidia.com/en-us/omniverse/apps/audio2face.md/)

Is able to generate expressive facial animation from an audio source in real-time

#### [NVIDIA Kimodo](https://research.nvidia.com/labs/sil/projects/kimodo/) - Full-Rig Diffusion Model

Generates a full skeleton animation from a text prompt

- Reasonably fast, but not real-time (~2-5 seconds)
- Does not natively work with UE MetaHuman#s rig (uses SOMA rig, has to be retargeted)

##### First Test with Kimodo -> Blender for conversion into FBX animation -> Retargetting in Unreal Engine to MetaHuman

Generation time for 10 animations in batch:  25.99s
Average: 2.6 secs
Time to Load Model:  23.76s

Result:
![Kimodo 1st Test](../../img/avatar_kimodo_test01.gif)

#### [NVIDIA ACE](https://developer.nvidia.com/ace-for-games) (Avatar Cloud Engine)

Bit hard for me to fully grasp, but seems to be a complete workflow that does:

1. NLP from voice input
2. Generate LLM output
3. Output TTS and drive facial animations using Audio2Face in real-time
4. Chose and blend between pre-defined animations appropriate to generated answer

#### State of the Art: [DiDiffGes](https://arxiv.org/abs/2503.17059) (2025)

Can generate gestures from speech with just 10 sampling steps.
Decouples gesture data into body and hand distributions

Claims to be real-time!

Demo: https://cyk990422.github.io/DIDiffGes/

! WAS MERGED WITH ANOTHER PAPER ("HoleGest") INTO ["Efficient-Audio-Gesture"](https://github.com/whuhxb/Efficient-Audio-Gesture)



#### State of the Art: [AsynFusion](https://arxiv.org/abs/2505.15058) (2025)

Enables paralell generation of facial and body animation, running a syncrhonization between them to relate their dependencies.

Claims to generate more cohesive / "natural" full body animations from a single audio stream.

Does not seem to work in real-time.


## Sending Data

Getting data in and out of Unreal Engine

### Transfering Framebuffers

A variety of open-source solutions leverage GPU architecture to send and recieve framebuffers with low-latency.

While Spout and Syphon are the fastest through their OS-specific integration, sharing raw textures directly on the local GPU, NDI offers a cross-plattform solution by encoding video to send over an IP network.

❗ **Raw pixel data** is important for img2img generation using Diffusion models, compression artifacts can alter the diffusion output siginificantly and hinder a visually consistent output.

#### **[Spout](https://spout.zeal.co/) (Windows)**

✅ Transfers raw pixel data

#### **[Syphon](https://syphon.info/) (MacOS)**

✅ Transfers raw pixel data

#### **[NDI](https://ndi.video/): Network Device Interface (Cross-Platform)**

Encodes the video and sends it over an IP network and can also be easily routed locally (using 127.0.0.1 or localhost) to share frames between apps on the same machine.

Integrated into Unreal Engine: NewTek provides an official Unreal Engine plugin.

❌ Sends compressed video frames

Applies a lightweight compression codec. Latency is low (usually around 1 frame), technically slightly heavier on the CPU/GPU than Spout's or Syphon's raw memory sharing.

#### **Native Unreal Alternative: Pixel Streaming**

❌ Sends compressed video frames

It uses a one-way-out architecture to compress and send video frames out of Unreal Engine.

It does not have the capabilities to recieve an image / framebuffer as an input for a shader etc.

### Animation Data

The LiveLink protocol can be used to send data structures from and to Unreal Engine, but it is not designed to parse any arbitrary JSON data. While third-party plugins enable the features, it is computationally heavy! Other, more efficitent data types can be used to improve performance, but still limit actions to animating a skeletal mesh. 

OSC (Open Sound Control) as a protocol can be natively used within Unreal Engine to send and recieve any data, does not have to be sound (!). 
It organizes data using a URL-like address system. Instead of sending a confusing string of numbers, you send a specific value to a specific address.

| Protocol | JSON via LiveLink | OSC (Open Sound Control) |
| --- | --- | --- |
| Performance | Moderate (CPU parsing strings) | Better (Parsing binary data) |
| Ease of Use (Animation) | High. LiveLink maps data directly to skeletal rigs automatically | Low. Have to manually route actions in a(n Animation) Blueprint |
| Flexibility | Limited mostly to transforms and blendshapes | Virtually endless. Can trigger events, change material colors, spawn particles, etc |
| Support | Requires custom third-party plugins | Native UE plugin, supported by almost all creative software |

## Distorting an Avatar

When distorting the avatar, it is important to keep the animation of the TTS and its body language intact and not interfere with this animation layer.

Hence, all distortions appear afterwards and transform later parts of the pipeline.

### Mesh Distortion

![Bone Scaling Proportional](../../img/avatar_distortion_mesh_proportional.gif)

![Bone Scaling Unproportional](../../img/avatar_distortion_mesh_unproportional.gif)

The bones' of a MetaHuman can be transformed.

### (Vertex) Shader Distortion

![Vertex Shader Displacement](../../img/avatar_distortion_shader.gif)

The vertex shader of a MetaHuman can be dsiplaced to transform the mesh.

### Neural Style Transfer

![Neural Style Transfer](../../img/avatar_distortion_styletransfer.gif)

Using Unreal Engine's Neural Post Processing, [ONXX](https://onnx.ai/) Style Transfer of the framebuffer is possible within the Engine.

This is significantly faster than applying a style transfer on the framebuffer: The render pipeline does not need to render a high-quality MetaHuman first, just for it to be processsed again!

### Scrambled LiveLink association

e.g.: left eye controls right arm

### Framebuffer to Image2Image Diffusion

By [transfering a framebuffer](#Transfering-Framebuffers) to [ComfyUI](https://www.comfyai.org/index.html), the output of the render can be used as basis to generate an image.