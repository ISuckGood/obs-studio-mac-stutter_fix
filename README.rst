# OBS Studio - macOS Fullscreen Projector Stutter Fix Experiment (for AverMedia Capture)

This is a modified version of OBS Studio, based on the official source code

## Purpose of Modification

The changes in this repository were made specifically to try and resolve a **stuttering issue** observed under the following conditions on a **MacBook Pro**:

1.  Using OBS Studio's **Fullscreen Projector** output on one display.
2.  Simultaneously capturing video from an external **USB AverMedia capture card**.

Under these specific circumstances, the fullscreen projector output would exhibit significant stutter or lag, which was not resolved by the standard "Disable macOS V-Sync" option in OBS settings.

## Changes Implemented

The core modification involves adding a background thread within `main.cpp` (specifically targeting macOS builds). This thread performs the following actions approximately every minute:

* Calls the internal `EnableOSXVSync(false)` function (which uses private macOS APIs `CGSSetDebugOptions` and `CGSDeferredUpdates`).
* Immediately calls `EnableOSXVSync(true)`.

The goal was to see if periodically cycling the V-Sync state via these low-level functions could mitigate the stuttering experienced in the described scenario.

## Disclaimer

* **Experimental:** This is a personal experiment, not an official fix.
* **Private APIs:** Relies on undocumented macOS functions that could change or break in future OS updates.
* **macOS Only:** These changes are specific to macOS.
* **Effectiveness:** [**Important:** You should state here whether this modification *actually solved* the stuttering problem for you, partially helped, or had no effect.]
* **Use At Your Own Risk:** This modification might introduce other instabilities.

## Building

Build this modified version following the standard OBS Studio build procedures for macOS.

## Ouput 
Check in the OBS log file that this message is repeated every 1 minute 

....

20:36:37.425: Video Capture Device: Selected device 'Live Gamer Ultra 2.1-Video'

20:36:37.455: Launching background VSync toggler thread...

20:36:37.455: VSync toggler thread launched.

20:36:37.455: [VSync Cycle] Thread started.

20:36:37.456: Video Capture Device: Capturing 'Live Gamer Ultra 2.1-Video' (0x120000007ca2553):

20:36:37.456:  Using Format          : 1920x1080 (16:9) - 29.97, 30, 50, 59.94, 60, 120, 240 FPS - CS 709 - NV12 (420v) 

20:36:37.456:  FPS                   : 60.0002 (60000240/1000000)

20:36:37.456:  Frame Interval        : 0.0166666 s

20:36:37.456: 

20:36:37.522: [mac-virtualcam] mac-camera-extension: OSSystemExtensionErrorCode 2 ("Missing entitlement com.apple.developer.system-extension.install")

*** 20:37:37.444: [VSync Cycle] Setting VSync state to: Disabled ***

*** 20:37:37.444: [VSync Cycle] Setting VSync state to: Enabled ***

*** 20:38:37.447: [VSync Cycle] Setting VSync state to: Disabled ***

*** 20:38:37.447: [VSync Cycle] Setting VSync state to: Enabled ***



