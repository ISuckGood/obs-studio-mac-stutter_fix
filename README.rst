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
