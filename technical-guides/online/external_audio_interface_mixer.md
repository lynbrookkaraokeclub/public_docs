# The Unified Audio Highway: Setup Guide

This document breaks down how your **Yamaha AG06 Mixer**, **OBS**, and **SonoBus** work together to create a seamless live streaming/karaoke setup. This routing ensures zero-latency monitoring for your voice, captures high-quality music, and delivers a perfect mix to the internet without feedback loops.

---

## 🗺️ The Audio Map Diagram

``` text
===================================================================================
                       THE UNIFIED AUDIO HIGHWAY (FINAL)
===================================================================================

 [INPUT 1: Physical Mic]            [INPUT 2: Discord Music]   
           │                                    │              
           │ (Analog XLR)                       │ (Captured Software Stream)
           ▼                                    ▼              
┌───────────────────────────┐         ┌───────────────────────────┐
│      [COMP 1: AG06]       │         │       [COMP 2: OBS]       │
│    (Set to INPUT MIX)     │         │                           │
├───────────────────────────┤         │  • Clones & routes music  │
│ • CH1 Microphone Input    │         │    directly to SonoBus    │
│   │                       │         └─────────────┬─────────────┘
│   ├──►(Direct Monitor)    │                       │             
│   │         │             │                       │             
│   │         ▼             │                       │ (Pure Digital
│   │   ┌───────────────┐   │                       │  Software Route)
│   │   │ [OUTPUT 1:    │   │                       ▼             
│   │   │  Headphones]  │   │         ┌───────────────────────────┐
│   │   └───────▲───────┘   │         │     [COMP 3: SONOBUS]     │
│   │           │           │         ├───────────────────────────┤
│   │           ▼           │         │ ┌───────────────────────┐ │
│   │   ┌───────────────┐   │         │ │    INTERNET INPUT     │ │
│   │   │  USB AUDIO IN │◄──┼─────────┼─┤ • Friends' Voices     │◄┼─── [INPUT 3: Friends'
│   │   │(Music+Friends)│   │         │ │   (Goes to: USB In)   │ │      Voice from Internet]
│   │   └───────────────┘   │         │ └───────────────────────┘ │
│   ▼                       │         │ ┌───────────────────────┐ │
│ ┌───────────────────────┐ │         │ │    INTERNET OUTPUT    │ │
│ │     USB AUDIO OUT     ├─┼────────►│ │ • Your Mic + OBS Music│ ├────► [OUTPUT 2: Internet]
│ └───────────────────────┘ │         │ │   (From AG06 & OBS)   │ │      (To Friends' Ears)
│    (ONLY sends CH1 Mic)   │         │ └───────────────────────┘ │
└───────────────────────────┘         └───────────────────────────┘
```
