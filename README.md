# LEVEL — Audio Meter for Windows

LEVEL is a Windows audio meter for guitar players, musicians, and audio users who want to compare signal level and tonal balance over time. It is available as a standalone application and a VST3 plug-in. The standalone version can listen to an audio interface directly; the plug-in receives audio routed to it by a compatible DAW.

> **Release status:** Windows x64. ASIO device access is available when the build includes ASIO support and a compatible ASIO driver is installed. The ASIO-enabled build must not be distributed until the Steinberg ASIO SDK agreement has been countersigned and all applicable licensing terms are met.

## Download

Download the latest version from the repository’s **Releases** page. Each release should include separate packages for:

- **LEVEL-Windows-Standalone.zip** — the desktop application.
- **LEVEL-Windows-VST3.zip** — the VST3 plug-in bundle.

Download the ZIP you need, extract it, and follow the relevant installation steps below. GitHub Actions build artifacts are temporary workflow outputs; use the release packages for regular installation and sharing.

Before running any downloaded software, make sure it came from this project’s official Releases page. Windows may show a security prompt for software downloaded from the internet; only continue if you trust the source and the specific release.

## System and compatibility

- Windows 10 or Windows 11, 64-bit.
- A 64-bit VST3-compatible DAW for the plug-in.
- An audio interface or other audio input for live measurement.
- For ASIO device access: an ASIO-enabled LEVEL build and the device manufacturer’s ASIO driver. ASIO is not a driver by itself.

The plug-in has been reported working in REAPER and Ableton Live on Windows. Host compatibility can vary by version and configuration. The standalone application and plug-in use the same measurement display, but audio-device setup is different: the standalone app selects a device itself, while a DAW manages device selection and routes audio to the plug-in.

## Install the standalone app

1. Download `LEVEL-Windows-Standalone.zip` from **Releases**.
2. Extract the ZIP to a folder you can find again, such as `Documents\LEVEL`.
3. Open the extracted folder and run `LEVEL.exe` (the executable may be inside a `LEVEL` subfolder, depending on the release packaging).
4. Choose **Options** to open audio-device settings.
5. Select the desired audio device, enable the input channel or stereo pair carrying your signal, and confirm the displayed sample rate.
6. Start audio and check that the status reads **INPUT OK** and the meter responds.

For ASIO, select the interface’s ASIO driver in the device settings. Install or update the driver from the interface manufacturer if it is missing. If ASIO is unavailable, choose another listed Windows audio device type where supported.

## Install the VST3 plug-in

1. Download `LEVEL-Windows-VST3.zip` from **Releases** and extract it.
2. Copy the complete `LEVEL.vst3` bundle to your VST3 plug-in folder. A common per-user location is:

   `%LOCALAPPDATA%\Programs\Common\VST3`

   The system-wide VST3 folder is commonly:

   `C:\Program Files\Common Files\VST3`

3. Restart or rescan plug-ins in your DAW.
4. Insert **LEVEL** on an audio track or bus and route the signal you want to inspect through that track.

Copy the whole `.vst3` bundle, including its contents. Do not copy only the files inside it. Some DAWs let you add a custom VST3 scan folder; if so, use the folder containing `LEVEL.vst3` and rescan.

## Quick start

### Continuous mode

LEVEL opens in **CONTINUOUS** mode so you can see live readings as soon as audio reaches it. In the standalone app, first select the input device. In a DAW, route audio through the plug-in.

### Capture & Hold

1. Choose **CAPTURE & HOLD**.
2. Set **WINDOW** to the duration you want to analyze. The default is 3 seconds.
3. Set **START THRESHOLD**. The default is −30 dBFS.
4. Press **CAPTURE**. LEVEL arms and waits for its short-term RMS reading to reach the threshold, then measures for the selected window and freezes the result.
5. Press **SET REFERENCE** to save the captured result as a comparison baseline.

A zero-second window uses one 10 ms analysis frame. It is not a single-sample measurement. If a reference is already saved, starting another capture gives the option to keep the existing reference or replace it when the new capture completes. **UPDATE REF** replaces the reference with the current reading. **RESET** clears measurements and the saved reference.

## Controls and readouts

### Main meters

- **RMS — dBFS:** average signal level over the selected analysis window.
- **WINDOW LUFS:** an approximate, ungated loudness estimate over the window. It is not a standards-compliant integrated LUFS measurement.
- **PEAK — dBFS:** the highest sample peak observed during the window. It is a sample-peak reading, not a true-peak or inter-sample-peak measurement.
- **SAMPLE RATE:** the current audio-device or host sample rate.
- **INPUT OK / clipping indicator:** indicates whether signal is present and warns when input samples reach the clipping region. A clip indication may remain lit until reset or a new measurement, depending on the display state.

### Reference comparison

A reference stores the captured RMS, window LUFS estimate, peak, and spectrum. The main readouts show their difference from that baseline. The spectrum shows the current reading, a reference marker, and current-minus-reference delta. Positive deltas are above the reference; negative deltas are below it.

Spectrum delta colors:

- **Green:** within ±0.5 dB.
- **Yellow:** +0.5 dB to +2 dB.
- **Orange:** above +2 dB.
- **Light blue:** −0.5 dB to −2 dB.
- **Dark blue:** below −2 dB.

Reference comparisons are useful for repeatable A/B checks, but they do not account for microphone position, room acoustics, pickup position, or other changes in the signal chain. Keep those conditions consistent when comparing captures.

### Spectrum views and resolution

- **BARS:** choose 10, 20, or 30 logarithmically spaced bands.
- **RTA:** choose 30, 60, or 120 plotted points for a smoother-looking trace.

Both views cover approximately 31.5 Hz to 16 kHz. The RTA option changes the number of display-analysis points; it does not create additional EQ bands or a high-resolution FFT. Bars and RTA values are approximate frequency-band energy readings, not calibrated equalizer measurements. The RTA trace is a connected display of the selected filter-bank points.

### Hearing profile

The profile selector offers **RAW**, **LOW (50 phon)**, **MODERATE (65 phon)**, **LOUD (80 phon)**, and **ARENA (90 phon)**. RAW shows the unadjusted spectrum. Other profiles add translucent estimated shadows for the live and reference spectra to illustrate how equal-loudness contours may change perceived tonal balance at different listening levels.

These shadows are an educational approximation. They do not change the audio, the underlying meter values, or the reference. They are not an SPL measurement, do not predict a particular listener’s hearing, and should not be treated as an EQ prescription. The ARENA curve is approximate above the frequency range covered by the selected equal-loudness data.

## Using a Helix or another multi-channel interface

The standalone app can show multiple input channels exposed by the selected audio driver. Helix USB channel assignments depend on the device model, firmware, global settings, and routing. Select the input pair that carries the signal you intend to compare—for example, a processed tone or a dry signal. If the display remains idle, try another input pair and check the Helix USB routing and audio settings.

For a DAW plug-in, select the interface and inputs in the DAW, then route the desired track or input to LEVEL. The plug-in does not independently choose the audio device in this setup.

## Troubleshooting

### The meter shows no input

- In the standalone app, open **Options** and confirm the correct device and input channels are selected and enabled.
- In a DAW, confirm the track has the intended input, is receiving signal, and routes through LEVEL.
- Check that the interface is not muted and that its control panel shows input activity.
- Try a different channel pair. Multi-channel USB devices often expose several pairs with different routing.
- In Capture & Hold, check that the signal exceeds the start threshold.

### The plug-in does not appear in the DAW

- Confirm you copied the complete `LEVEL.vst3` bundle into a VST3 scan folder.
- Restart the DAW or run its plug-in rescan.
- Confirm the DAW and plug-in are both 64-bit and VST3-compatible.
- Check the DAW’s plug-in manager for a scan error or a disabled plug-in.

### Audio crackles or drops out in standalone mode

- Open **Options** and increase the audio buffer/block size.
- Use the interface manufacturer’s ASIO driver when available and included in the distributed build; otherwise try another supported device type.
- Match the device sample rate with the project or system setting where possible.
- Close other applications that may be using the device in exclusive mode, or select a different audio device.

Increasing buffer size usually reduces the load on the computer at the cost of higher monitoring latency. Since LEVEL is a meter, prioritize stable measurement over low-latency monitoring.

### The standalone app does not list ASIO

ASIO appears only when the build includes ASIO support and an ASIO driver is installed and recognized by Windows. Install the official driver for your interface, reconnect the device, then reopen LEVEL. ASIO support and distribution are also subject to the licensing note below.

### The reading differs from another meter

Different meters may use different averaging times, weighting, gating, channel summing, true-peak handling, or spectrum filters. LEVEL’s LUFS and spectrum are approximate; see **Measurement notes** before using readings for calibration or safety decisions.

## Measurement notes and limitations

LEVEL is intended for practical monitoring and tonal comparison. It is not a certified SPL meter, hearing-safety monitor, room-correction system, or mastering loudness analyzer.

- LUFS is an approximate, ungated, windowed estimate with approximate K-weighting; it is not EBU R128 integrated loudness.
- Peak is sample peak, not true peak.
- Spectrum levels are approximate filter-bank energy readings relative to digital full scale; they are not calibrated acoustic frequency response or a replacement for a measurement microphone and analyzer.
- Listening-profile shadows are estimates and are not calibrated to the actual sound pressure level at the listener’s ears.
- Results depend on the source, routing, device gain, sample rate, and selected analysis window.
- For critical calibration or hearing-safety decisions, use appropriate calibrated equipment and established procedures.

## Licensing and third-party components

### Project and binary-use terms

This repository distributes compiled builds; it does not grant rights to the LEVEL source code. The repository’s GitHub **License** setting should remain **None** unless the project owner chooses and publishes a separate license. Before public distribution, each release should include a separate `EULA.txt` or other clear terms covering permitted use, copying, redistribution, warranty, and liability. Do not assume that GitHub’s default license or the word “shareware” defines those terms.

### Steinberg ASIO

ASIO access in the Windows standalone application uses the ASIO SDK/interface through JUCE. Steinberg’s proprietary ASIO SDK agreement is royalty-free, but it must be signed by both parties to become effective. The project owner has submitted the signed agreement and is awaiting Steinberg’s countersignature.

**Release gate:** Do not publish or share an ASIO-enabled build externally until the fully executed agreement has been returned and the build’s distribution complies with the agreement and applicable ASIO Usage Guidelines. Internal development and testing may proceed while the agreement is pending. This applies to free public betas as well as paid releases. Retain the countersigned agreement with project records; do not upload personal contract details to the public repository.

See Steinberg’s [ASIO SDK Proprietary Licensing FAQ](https://forums.steinberg.net/t/asio-sdk-proprietary-licensing-faq/1044024) and the [official ASIO SDK page](https://www.steinberg.net/asiosdk/). The agreement and usage guidelines control if they differ from this summary.

### JUCE

LEVEL is built with JUCE 8.0.15. JUCE’s terms depend on the applicable license tier and the project owner’s circumstances, including revenue or funding. Review the current [JUCE 8 licence](https://juce.com/legal/juce-8-licence/) and include any notices required by the selected JUCE license and the release package. The README is not a substitute for checking those terms.

## Privacy and support

LEVEL is an offline audio utility. It processes the audio routed into it and does not require an account. This project does not provide formal technical support or guarantee compatibility with every computer, interface, driver, or DAW.

For bug reports, include the LEVEL version, Windows version, standalone or DAW/host details, audio interface and driver type, sample rate, and steps to reproduce. Do not include private audio recordings unless they are necessary and you are comfortable sharing them.

Support development: [Buy me a coffee on Ko-fi](https://ko-fi.com/carlosbraga).

---

*LEVEL is an independent project. Product, company, and driver names are used only to describe compatibility; their respective owners retain their trademarks.*
