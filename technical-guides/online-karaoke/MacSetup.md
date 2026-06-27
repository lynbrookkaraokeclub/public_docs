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
