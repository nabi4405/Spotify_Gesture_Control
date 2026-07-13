# Spotify Gesture Controller

An edge-AI powered, touchless media controller that uses real-time computer vision to control Spotify playback. Built on the **Arduino Uno Q** dual-brain architecture, this project combines hardware-accelerated hand gesture classification (via a USB webcam) with instant multi-sensory feedback using **Modulino Pixels** and **Modulino Buzzer** modules, seamlessly integrated with the Spotify Web API.

---

##  System Architecture & Highlights

* **Edge Vision Processing:** Uses Arduino App Lab's `VideoObjectDetection` framework to capture and classify hand gestures in real time from a standard USB webcam, applying debounce and cooldown algorithms to prevent accidental multi-triggers.
* **Hardware-Software RPC Bridge:** The Python host communicates seamlessly with the Uno Q microcontroller via `Arduino_RouterBridge` over UART, triggering instant tactile feedback without blocking the main application loop.
* **Asynchronous Cloud Control:** Utilizes `spotipy` within non-blocking background daemon threads to dispatch Spotify Web API commands (OAuth 2.0 REST requests), ensuring that network latency never freezes or stutters the live video capture feed.
* **Headless Embedded OAuth:** Implements a custom in-memory token authentication handler (`MemoryCacheHandler`), allowing seamless background token refreshing on embedded Linux devices without relying on interactive browser prompts or file-system lock constraints.

---

##  Gesture & Hardware Mapping

When a gesture is confirmed by the vision model, the Arduino Uno Q instantly triggers sensory feedback while simultaneously sending playback commands to your active Spotify device:

| Gesture Label | Spotify Action | Modulino Pixels (LEDs) | Modulino Buzzer (Audio) |
| :--- | :--- | :--- | :--- |
| **`Play`** | Resume Playback | Solid Green | Single High Beep (880 Hz) |
| **`Pause`** | Pause Playback | Solid Red | Single Low Beep (440 Hz) |
| **`Next_Track`** | Skip Forward | Solid Blue | Up-Chirp (660 Hz → 880 Hz) |
| **`Previous_Track`** | Skip Backward | Solid Orange | Down-Chirp (880 Hz → 660 Hz) |

---

## Technology Stack
* **Hardware:** Arduino Uno Q, Modulino Pixels, Modulino Buzzer, USB Webcam
* **Firmware / MCU:** C++ / Arduino IDE (`Modulino.h`, `Arduino_RouterBridge.h`)
* **Host / SBC:** Python 3, Arduino App Lab Framework (`app_bricks`, `app_utils`)
* **Cloud & APIs:** Spotify Web API, OAuth 2.0, `spotipy`
