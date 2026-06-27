# macOS Low-Latency Karaoke Setup Guide

This comprehensive configuration guide outlines how to build an ultra-low-latency, high-fidelity online karaoke station on macOS. 

By leveraging **Discord** (for communications and WatchTogether multimedia), **OBS Studio** (for application-specific audio isolation), **BlackHole** (for internal audio plumbing), and **SonoBus** (as your master peer-to-peer network mixer), you can sing with remote performers in real time without audio drift, heavy compression, or distracting echo.

---

```
===================================================================================
             MAC DISCORD/WATCHTOGETHER & SONOBUS ARCHITECTURE DIAGRAM
===================================================================================

                    +------------------------------------+
                    |            DISCORD APP             |
                    |  - Runs WatchTogether Activities   |
                    |  - Generates the Karaoke Audio     |
                    |  - Displays Lyrics on Screen       |
                    +-----------------+------------------+
                                      |
                                      | (Audio Stream Only)
                                      v
                    +------------------------------------+
                    |             OBS STUDIO             |
                    |  - [macOS Audio Capture]: Targets   |
                    |    the Discord Application Only.    |
                    +-----------------+------------------+
                                      |
                                      | OBS Audio Monitor clones the Discord 
                                      | music and pipes it to a Virtual Cable
                                      v
                          +-----------------------+
                          |  VIRTUAL AUDIO PIPE   |
                          |   (e.g., BlackHole)   |
                          +-----------+-----------+
                                      |
                                      | (Provides Channels 2-3)
                                      v
  +-------------------------------------------------------------------------+
  |                             AUDIO MIDI SETUP                            |
  |                  [ MAC AGGREGATE DEVICE CONFIGURATION ]                 |
  |                                                                         |
  |     +-------------------------+     +-----------------------------+     |
  |     |   PHYSICAL MICROPHONE   |     |     VIRTUAL AUDIO PIPE      |     |
  |     |      (Your Voice)       |     |  (Discord Music Loopback)   |     |
  |     +------------+------------+     +--------------+--------------+     |
  |                  |                                 |                    |
  |                  v (Channel 1)                     v (Channels 2-3)     |
  +------------------+---------------------------------+--------------------+
                     |                                 |
                     +----------------+----------------+
                                      |
                                      | (Feeds a unified 3-Channel stream)
                                      v
+-----------------------------------------------------------------------------------+
|                                      SONOBUS                                      |
|                                                                                   |
|  [ LOCAL DEVICE SETUP (GEAR ICON) ]                                               |
|   - Input Device: Custom "Aggregate Device" (Receives Ch 1, 2, and 3)             |
|   - Output Device: Set to your physical headphones port, USB DAC, or interface.   |
|                                                                                   |
|  [ INPUT MIXER (MIDDLE TOP BUTTON) ]                                              |
|   - Channel 1: Set to Mono (Controls your Microphone Volume)                      |
|   - Channels 2-3: Set to Stereo Pair (Controls the Discord Music Volume)          |
|                                                                                   |
|                               +-------------------------------------+             |
|                               | LOCAL HEADPHONE OUTPUT              |             |
|                               | (Your complete combined live mix)   |             |
|                               +------------------+------------------+             |
|                                                  ^                                |
|                                                  | (Combines Local + Remote Audio)|
|                                                  |                                |
|       +------------------------------------------+--------------------------+     |
|       |                                                                     |     |
|       v                                                                     |     |
| +-------------------------+                               +---------------------+ |
| | INTERNET OUTPUT         |                               | INTERNET INPUT      | |
| | (What You Transmit)     |                               | (Where Group Tracks | |
| +------------+------------+                               |  Land)              | |
|              |                                            +----------+----------+ |
+--------------|-------------------------------------------------------|------------+
               |                                                       ^
               | [ BROADCAST OUT ]                                     | [ INBOUND STREAM ]
               |   Music + Voice                                       | Receives everyone 
               |                                                       | else's uncompressed
               v                                                       | singing voices + music
               =========================================================
                                        INTERNET
               =========================================================
```

## 1. Software Manifest & Installation Requirements

Before beginning configuration, download and install the following free software components:

### 1. Audio Loopback Engine: BlackHole (2ch & 16ch)
* **Purpose:** Acts as an invisible, virtual patch cable passing audio from OBS to your hardware aggregate layer. 
* **Installation:** Download and install **both** the 2ch and 16ch versions of BlackHole from the official Existential Audio source. While we will only need the **2ch version** for this specific karaoke setup, having both installed ensures system compatibility for advanced routing configurations later.
* *Security Note:* Go to Mac **System Settings > Privacy & Security** and manually click "Allow" if the system software extension is blocked by macOS.

### 2. Live Isolation Console: OBS Studio
* **Purpose:** Captures, clones, and isolates the internal audio processing layers directly out of Discord.
* **Installation:** Download the native Mac package from the official OBS Studio website, open the `.dmg` file, and drag OBS into your system `/Applications` folder.

### 3. Master Digital Network Mixer: SonoBus
* **Purpose:** Peer-to-peer transmission pipeline ensuring uncompressed, low-latency audio sync with remote performers.
* **Installation:** Download the latest macOS package from the official SonoBus website, launch the installer wizard, and follow the basic installation steps.

---

## 2. Configuration Step 1: macOS Audio MIDI Setup

Because SonoBus can only connect to a single input hardware option at one time, you must glue your separate physical microphone and your virtual music pipeline together into a unified custom combined driver.

1. Launch the **Audio MIDI Setup** utility (Press `Cmd + Space`, type `Audio MIDI Setup`, and press `Enter`).
2. Navigate to the bottom-left corner of the sidebar, click the native **`+` icon**, and select **Create Aggregate Device**.
3. Double-click the default title of this new entry and rename it to exactly: `Karaoke Aggregate Input`.
4. In the grid panel on the right side, check the device option boxes in this specific order to construct your sub-channel map:
    * **Check Your Physical Microphone** (or USB Audio Interface) first. This hard-maps your vocal stream to **Channel 1**.
    * **Check BlackHole 2ch** second. This links the software capture pipeline to the next available slots: **Channels 2 and 3**.
5. **Enable Drift Correction:** Check the **Drift Correction** box specifically for **BlackHole 2ch**. This is a mandatory step that prevents the virtual driver's timing from desynced timing errors relative to your physical hardware mic over long singing sessions.
6. **Take Note of Your Channel Mapping:** Write down or carefully note which channels belong to which source (e.g., Channel 1 = Microphone, Channels 2-3 = BlackHole 2ch Music). You will need to know these exact designations when managing the Input Mixer inside SonoBus.

---

## 3. Configuration Step 2: OBS Studio Routing

OBS serves as your isolation container to cleanly sample your backing track playback loops while ignoring other Mac sound layers like alert bells or system chimes. 

> 🔗 For a deeper dive into complex layouts and troubleshooting, consult the [Detailed OBS Setup Documentation on GitHub](https://github.com/lynbrookkaraokeclub/public_docs/blob/main/technical-guides/online-karaoke/OBS.md).

For a standalone live Mac karaoke setup, configure your OBS layers using these explicit parameters:

1. Open **Discord**, join a voice room, and launch a **WatchTogether** activity from the Activities tray button.
2. Launch **OBS Studio**. Under the **Sources** dock at the bottom, click the **`+` icon** and add **macOS Audio Capture**. Set its properties to target the **Discord** application explicitly to isolate the track music.
3. Click the **`+` icon** again and add **macOS Screen Capture**. Choose the specific window layer targeting Discord so your screen displays the video lyrics cleanly.
4. Navigate into the application preferences: **Settings > Audio > Advanced**.
5. Locate the **Monitoring Device** dropdown configuration menu and set it to **BlackHole 2ch**. Click **Apply** and hit **OK**.
6. Find the Discord fader track in your main OBS Audio Mixer section, click its configuration properties icon (the three vertical dots), and select **Advanced Audio Properties**.
7. **Set to Monitor Only:** Since our goal is purely live streaming performance over the network and **not recording** local files to your hard drive, locate your Discord capture source track line, scroll over to its **Audio Monitoring** dropdown menu, and switch it to **Monitor Only (mute output)**. This routes the audio strictly out through your virtual monitor cable without creating unnecessary local rendering overhead.

---

## 4. Configuration Step 3: SonoBus Core Parameters

> 🔗 For the complete master copy of the application rules and features, review the [Official SonoBus Technical Documentation on GitHub](https://github.com/lynbrookkaraokeclub/public_docs/blob/main/technical-guides/online-karaoke/SonoBus.md).

1. Open the **SonoBus** application and click the primary **Gear Icon** found in the top-left corner of the master workspace to establish your base preferences.
2. Inside the audio device matrix panel, map your system routes as follows:
    * **Input Device:** Select your custom created **Karaoke Aggregate Input** option.
    * **Channel Selection:** You must explicitly make sure that **all 3 inputs** (Channel 1, Channel 2, and Channel 3) are active and selected underneath the Input Device assignment area. Leaving any unchecked will permanently drop that respective voice or music leg before it ever reaches the application.
    * **Output Device:** Choose your physical system monitoring hardware (e.g., your Mac Built-In Headphones Jack, your external USB DAC, or your hardware audio interface line monitor ports).
3. **Hardware Latency Thresholds:** Set your operational **Sample Rate** to `48000 Hz` (48 kHz) and drop your processing **Buffer Size** to `128 samples` (or `64 samples` if your Mac processor handles it cleanly). This eliminates local processing lag without introducing audio dropouts or click artifacts.

---

## 5. Configuration Step 4: Input Mixer Allocation

The **Input Mixer** control button is located at the **middle top** of the primary SonoBus window layout. Clicking it expands your multi-channel lane configuration overlay. This is where your written MIDI sub-channel notes are applied:

* **Channel 1 (Your Microphone Voice):** Set this single vertical track to **Send Mono**. Microphones should always transmit in mono so your singing remains perfectly centered within everyone else's headphones.
* **Channels 2-3 (Discord Music Loopback via BlackHole 2ch):** Click the link icon on these adjacent channels to handle them together as a stereophonic pair, and switch the toggle selection to **Send Stereo**. This preserves the spatial fidelity, depth, and original panning layouts of the instrument backing files.
* **Pre-Flight Balancing:** Use the individual volume faders inside this top Input Mixer tray to scale your local voice levels independently from the music tracks *before* your combined performance enters the outbound network internet broadcast.

---

## 6. Operational Dashboard Adjustments

Outside of the Input Mixer, the primary window houses your global listening modifiers on the main interface. **Crucially: Neither the Monitor slider nor the Out Level slider controls your outbound transmission volume on the web.** They exist solely to balance your own local ears:

### 🎚️ Send Mono vs. Send Stereo
* **Send Mono:** Converts your input audio into a single, centered stream. This is ideal for microphones, as vocal tracks should always be sent in mono so your voice sounds centered in your friends' headphones.
* **Send Stereo:** Keeps distinct Left and Right audio channels. This is ideal for background backing tracks or music captured from OBS, preserving the wide, immersive spatial sound of the original file.

### 🎚️ The "Monitor" Slider
* **Function:** Adjusts the feedback loop volume of your *own* combined inputs (your microphone voice + the Discord backing music clone) reflecting straight back into your headphones.
* **Missing Voice Troubleshooting:** If you can hear the backing tracks clearly but your live voice is completely missing from your headphones, open the top **Input Mixer** panel and ensure the tiny **speaker icon** (the dedicated monitoring toggle) directly beneath the Channel 1 fader is clicked **ON**. 
* *Note:* If your physical interface uses hardware zero-latency monitoring, you may want this turned down to prevent double-monitoring artifacts.

### 🎛️ The "Out Level" Slider
* **Function:** Serves as the master volume fader for the whole application interface. Sliding this up or down dynamically scales the combined audio of *both* your local monitored feeds and all incoming internet participant streams at the exact same time inside your headphones.

### 🔇 The Master Mute Button
* **Function:** Located in the **lower-left corner** of the console (the microphone icon button). Clicking this acts as a network firewall kill-switch: **it mutes your internet transmission only.** Your outbound voice and backing music will immediately stop broadcasting to the internet room, but your incoming lines remain open so you can still hear everyone else in the session.

---

## 7. Connecting to your Karaoke Group Session

The **Connect** button is located at the top of the screen and serves as the gateway to link multiple remote users into a unified low-latency session.

1. Hit the **Connect** button at the top center of the SonoBus interface to open the connection dialogue window.
2. **Server Group Name:** Provide your group's designated private room name. If you are starting a new room, typing a unique name will instantly create it on the public SonoBus keyserver.
3. **Password:** If your group requires privacy (strongly recommended for structured sessions), enter the room password.
4. **Your Name:** Type a clear, recognizable display name into the text box so your group members can distinguish your specific audio track strip easily.
5. Click **Connect**. Once successfully handshake-linked, your remote group users will instantly form dynamic individual mixer faders across the center of your primary dashboard window, giving you full structural control to selectively balance your live multi-user environment.
