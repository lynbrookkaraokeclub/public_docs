# The High Level Setup Guide For Using External Audio Interfaces/Mixers

This document breaks down how your **Yamaha AG06(or other) Mixer**, **OBS**, and **SonoBus** work together to create a seamless live streaming/karaoke setup. This routing ensures zero-latency monitoring for your voice, captures high-quality music, and delivers a perfect mix to the internet without feedback loops.

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
│   │           |           │         │ ┌───────────────────────┐ │
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

## 1. Input Sources (The Starting Points)
Your audio originates from three independent sources:
* **[INPUT 1] Physical Mic:** Your live vocals connected via an analog XLR cable into Channel 1 (CH1) of the AG06 Mixer.
* **[INPUT 2] Discord Music:** The backing tracks or audio captured directly from your computer using OBS.
* **[INPUT 3] Friends' Voices:** The live audio stream coming from the internet via the SonoBus peer-to-peer network.

---

## 2. Hardware Layer: The Yamaha AG06 Mixer
The mixer acts as your local hardware anchor. It must be set to **INPUT MIX** mode.

### 📥 Inputs Received by the Mixer
* **CH1 Microphone Input:** Receives your raw voice from the physical mic.
* **USB AUDIO IN:** Receives a combined digital package from SonoBus containing **both** the backing music and your friends' voices.

### 📤 Outputs Sent by the Mixer
* **[OUTPUT 1] Headphones:** This is your personal monitor mix. Thanks to **Direct Monitor** hardware circuitry, your mic goes straight to your ears instantly with zero delay, perfectly blended with the music and friends arriving from the `USB AUDIO IN`.
* **USB AUDIO OUT:** Because the mixer is set to *Input Mix*, it is hardwired to package and send your **CH1 Microphone Input** back up to the computer. 

> ⚠️ **Note on "Input Mix" Mode:** Even though this hardware switch is labeled "Input Mix", because your music and friends' voices are already native digital software audio living inside the PC, only your raw microphone audio is broadcast back up the `USB AUDIO OUT` pipe. This crucial separation prevents an infinite loop.

---

## 3. Software Traffic Control: OBS & SonoBus
This is the digital brain of your setup where audio routing and internet streaming are managed.

### 🎥 OBS (Open Broadcaster Software)
OBS acts as a pure digital clone engine. It captures the **Discord Music**, duplicates the stream, and routes it directly into the SonoBus software layer via a virtual routing link.

### 🎧 SonoBus Application
SonoBus splits its routing tasks into two distinct functional layers:

#### A. INTERNET INPUT Layer
This section handles all incoming audio data entering the application from outside sources:
* **Friends' Voices:** Captured live from the internet.
* **OBS Music:** Captured locally from your OBS software stream.
* **Data Flow Destination:** SonoBus takes these two elements, bundles them together, and routes them straight down into the AG06's **USB AUDIO IN** so you can hear them in your headphones.

#### B. INTERNET OUTPUT Layer
This section packages your outbound stream to the world:
* **Your Mic + OBS Music:** SonoBus grabs your microphone stream (arriving from the mixer's hardware `USB AUDIO OUT`) and mixes it directly with the copy of the music stream coming from OBS.
* **[OUTPUT 2] Internet:** This combined mix is broadcast over the web directly to your friends' ears.

---

## 🧠 Deep-Dive Architectural Explanations

### ❓ How the OBS Music Reaches the Mixer via USB
You might wonder why there isn't a direct connection from OBS down into the mixer's USB Input. The music *does* eventually go into the AG06 via the USB cable, but it takes a **detour through SonoBus first**:
1. OBS shoots the music sideways into SonoBus (`INTERNET INPUT`).
2. SonoBus acts as a digital container, packing that music together with your friends' voices.
3. SonoBus drives that single, unified digital package down into the AG06's `USB AUDIO IN`.

By utilizing SonoBus as your primary traffic controller, your music gets into your mixer without needing an extra physical cable, and without fighting your friends' voices for space on independent hardware knobs.

### ⚠️ Why You Must NOT Route Audio Directly from OBS to USB Audio In
If you attempt to route OBS music directly to the mixer's `USB AUDIO IN` instead of through SonoBus, you will break the setup:
* **The Loop:** The music hits your mixer. Because the mixer is set to *INPUT MIX*, it will take your microphone **and** that incoming music stream and throw them both up the `USB AUDIO OUT` pipe together.
* **The Echo/Phase Disaster:** SonoBus would then receive the music *twice*: once directly from OBS via software, and a fraction of a second later up through the mixer's microphone line. This results in a jarring, hollow echo effect for everyone listening on the internet.

### 🎤 The Truth About Microphone Monitoring in SonoBus
SonoBus has a built-in feature to monitor your own voice (usually represented by a speaker icon next to your input). **In this setup, you must leave SonoBus mic monitoring turned OFF.**
* **Hardware Monitoring is Already Active:** The AG06 already splits your voice via the physical **(Direct Monitor)** path directly to `OUTPUT 1: Headphones` at the speed of light.
* **The Penalty of Software Monitoring:** If you turn it on in SonoBus, your voice travels up the USB cable, gets digitally processed by the PC, and shoots back down to `USB AUDIO IN`. You will hear your own voice twice—the second one lagging slightly behind, creating a highly distracting echo in your ears while you try to speak or sing.

---

## 📊 How to Mix Your Vocals Relative to the Music
Because your headphones are plugged directly into the AG06 mixer, you are listening to a **local hardware mix**. Your ears cannot hear the exact balance that SonoBus is broadcasting to the internet. 

To achieve a professional stream mix, you must use the **visual meters inside the SonoBus application**:

1. **Watch the Outbound Level Meters:** While singing or talking over a backing track, look at your input/send channels inside SonoBus. Your voice meter should peak comfortably above the music meter (typically, vocals should target roughly $-12\text{ dB}$ to $-6\text{ dB}$, while the background music sits lower around $-18\text{ dB}$ to $-15\text{ dB}$).
2. **Use the Software Faders:** If the internet says the music is drowning you out, **do not** touch the physical knobs on your AG06. Instead, adjust the software faders *inside SonoBus* to drop the music volume or raise your mic volume for the stream.

---

## 💡 Quick-Reference Mixing Rules

| Goal | What to Adjust | Where to Look/Listen |
| :--- | :--- | :--- |
| **Change what the INTERNET hears** | Adjust the software faders inside **SonoBus**. | Watch the visual meters in **SonoBus** (Keep vocals peaking higher than music). |
| **Change what YOU hear** | Adjust the physical knobs on the **AG06 Mixer** (CH1 or USB knob). | Listen directly to your **Headphones**. |
| **Prevent Echoes** | Keep **SonoBus Mic Monitoring OFF**. | Trust the AG06 **Direct Monitor** path. |
