# SonoBus: Role and Configuration Guide

Think of **SonoBus** like a specialized Discord Voice Channel or a Zoom Call on steroids. While apps like Zoom and Discord are built for standard speech and heavily compress your audio (which causes noticeable lag and cuts out music), SonoBus is engineered specifically for musicians and live performers. It transmits high-fidelity, uncompressed, peer-to-peer audio across the internet with the absolute lowest latency possible, making it the ultimate tool for real-time online karaoke and collaborative sessions.

---

## 1. The Role of SonoBus in this Setup

SonoBus acts as your digital **Traffic Controller** for the internet. Its primary responsibilities are split into **Two Main Inputs** and **Two Main Outputs**:

### 📥 The Two Inputs
1. **Local Computer Input (Apps & Hardware):** SonoBus accepts audio coming from inside or attached to your computer, such as your physical microphone or a digital music copy sent over from OBS.
2. **Internet Input (The Group):** SonoBus receives the live, incoming digital audio streams transmitted over the internet from all of your online friends.

### 📤 The Two Outputs
1. **Local Computer Output (Your Ears):** SonoBus mixes everything together and drives it out to the playback device inside your computer (such as your headphones or speakers) so you can hear the full session.
2. **Internet Output (The World):** SonoBus packages your combined local sounds (your voice + your music) and broadcasts them out across the internet directly into your friends' ears.

---

## 2. Device Configuration (The Audio Setup)

To configure how SonoBus talks to your local computer, click the **Gear Icon** located in the top-left corner of the window. 

> 💡 **Configuration Rule:** This panel manages the audio moving to and from other apps and physical hardware attached directly to your machine. Because everyone uses different setups, **this step will look very different depending on your specific hardware.** Fundamentally, your goal in this menu is to answer two questions:
* **Input Device:** Where is the live sound coming from *within* or *attached to* your computer? (e.g., your microphone interface or a virtual software cable link).
* **Output Device:** Where do you want to send the "master mixed sound" *inside* your computer so you can hear it? (e.g., your headphones or system speakers).

---

## 3. Managing the Input Mixer

The **Input Mixer** button is located in the **middle top** of the main SonoBus window. Clicking this opens the multi-channel configuration overlay.

* **Channel Matching:** You must ensure that the channels activated inside the Input Mixer window explicitly match the physical or digital channels of your configured input device. 
* **Independent Volume Control:** If your input hardware separates your background music and your microphone into independent channels, the Input Mixer will display them as separate strips. This allows you to adjust the volume of your voice independently from the volume of the backing tracks before it ever leaves your machine.

---

## 4. Main Control Sliders & Settings

Outside of the Input Mixer panel, on the main interface, you will find your primary audio behavior and transmission controls. 

It is a common misconception that these global sliders change how loud you sound to other people. **Crucially, neither the Monitor slider nor the Out Level slider controls the volume of what gets sent out to the internet.** Instead, they govern your own personal listening environment:

### 🎛️ Send Mono vs. Send Stereo
* **Send Mono:** Converts your input audio into a single, centered stream. This is ideal for microphones, as vocal tracks should always be sent in mono so your voice sounds centered in your friends' headphones.
* **Send Stereo:** Keeps distinct Left and Right audio channels. This is ideal for background backing tracks or music captured from OBS, preserving the wide, immersive spatial sound of the original file.

### 🎚️ The "Monitor" Slider
The **Monitor** slider strictly adjusts the volume of your *own local input* (your voice and your instruments) inside your own headphones. 
* **The Purpose:** It exists purely so you can hear yourself better over the group. Turning this up or down has zero effect on what the rest of the room hears. 
* **Rule of Thumb:** If your physical hardware already feeds your voice directly to your ears with zero latency, keep this turned down to avoid a delayed digital echo.

### 🎛️ The "Out Level" Slider
The **Out Level** slider acts as the master volume control for the entire application interface. 
* **The Purpose:** Rather than altering your internet transmission, this slider scales the combined audio of *both* your own instruments and all other group participants simultaneously in your headphones. Think of it as the master volume knob for your local listening layout.

### 🔇 The Master Mute Button
In the **lower-left corner** of the SonoBus interface, there is a dedicated microphone icon button. Clicking this acts as a master privacy kill-switch: **it mutes your internet transmission only.** It instantly prevents any audio from being broadcast out to the internet room, while still safely allowing you to hear the incoming audio from the rest of the group.

---

## 5. Connecting to a Live Session

The **Connect** button is located at the top of the screen. This is the gateway used to link multiple remote users into a unified low-latency session.

To join or host an online session:
1. Click the **Connect** button to open the connection dialogue window.
2. **Server Group Name:** Enter the exact group room name designated for your session. If you are starting a new room, typing a unique name will instantly create it on the public SonoBus keyserver.
3. **Password:** If your group requires privacy (strongly recommended for structured sessions), enter a room password.
4. **Your Name:** Choose a recognizable display name so your friends can easily identify your specific channel strip in their mixers.
5. Click **Connect**. Once successfully handshake-linked, your online group members will dynamically appear as individual volume strips in the center section of the dashboard, allowing you to selectively balance their volumes relative to one another.

---

## 6. Summary of Best Practices for Stability

* **Match Sample Rates:** Ensure your operating system, your music apps, and SonoBus are all explicitly locked to the same sample rate (`48000 Hz`) to prevent digital distortion, clicks, or pops.
* **Prioritize Ethernet:** Because SonoBus passes high-fidelity, low-latency audio directly between users (peer-to-peer), wireless jitter will cause severe audio dropouts. Always use a wired Ethernet connection instead of Wi-Fi when running live interactive sessions.
