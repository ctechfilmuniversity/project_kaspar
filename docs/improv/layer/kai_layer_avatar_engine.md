---
layout: default
title: Layer Avatar Engine
parent: Live Rehearsal Improvisation
nav_order: 7
---


# Avatar Engine

Unreal Engine's [MetaHumans](https://www.metahuman.com/) are used to quickly build, display, animate and distort virtual clones of the Actors.

The integreation deep into the UE systems makes it feasible to render high-quality avatars in real-time using UE's various systems optimized for real-time rendering in virtual production contexts.

## Building an Avatar

## Animating an Avatar

### LiveLink

Common interface for streaming and consuming animation data from external sources into Unreal Engine.

A variety of sources can be used to drive a variety of subjects. Some input can move a camera within the engine, a webcam feed can drive a MetaHuman's facial mesh, a MoCap suit can move a MetaHuman's skeleton

### Audio to Animation (MetaHuman Animator)

Native MetaHuman plugin to process audio and build facial animations to be used to move a MetaHuan face mesh.

Also works in real-time!

_Lacks a bit of "depth", mostly just mouth movement, not really emotional changes to a face (frowning etc.)_

### Pre-Built Animations

By pre-building animations and interpolating between them using UE's Blendspace, complex and unique movement can be achieved.

_Problem: Pre-building lots of animations_

## Sending Data

Getting data in and out of Unreal Engine

### Transfering Framebuffers

A variety of open-source solutions leverage GPU architecture to send and recieve framebuffers with low-latency.

While Spout and Syphon are the fastest through their OS-specific integration, sharing raw textures directly on the local GPU, NDI offers a cross-plattform solution by encoding video to send over an IP network.

❗ **Raw pixel data** is important for img2img generation using Diffusion models, compression artifacts can alter the diffusion output siginificantly and hinder a visually consistent output.

#### [Spout](https://spout.zeal.co/) (Windows)

✅ Transfers raw pixel data

#### [Syphon](https://syphon.info/) (MacOS)

✅ Transfers raw pixel data

#### [NDI](https://ndi.video/): Network Device Interface (Cross-Platform)

Encodes the video and sends it over an IP network and can also be easily routed locally (using 127.0.0.1 or localhost) to share frames between apps on the same machine.

Integrated into Unreal Engine: NewTek provides an official Unreal Engine plugin.

❌ Sends compressed video frames

Applies a lightweight compression codec. Latency is low (usually around 1 frame), technically slightly heavier on the CPU/GPU than Spout's or Syphon's raw memory sharing.

#### Native Unreal Alternative: Pixel Streaming

❌ Sends compressed video frames

It uses a one-way-out architecture to compress and send video frames out of Unreal Engine.

It does not have the capabilities to recieve an image / framebuffer as an input for a shader etc.

## Distorting an Avatar