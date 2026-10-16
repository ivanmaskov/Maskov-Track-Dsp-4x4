![alt text](https://github.com/ivanmaskov/Maskov-Track-Dsp-4x4/blob/main/03-analyzer.png?raw=true)
# DSP 4x4 Editor

An unofficial editor for the **t.racks DSP 4x4 Mini** loudspeaker processor, by **Maskov**.

It replaces the original Windows-only t.racks editor with one that runs on **Linux**
(Debian and Ubuntu package) and **Windows**, and that phones and other computers on your
network can use as a remote. It is written in Rust with no external libraries, and the
editor opens in your web browser.

> Not affiliated with or endorsed by Thomann or t.racks. The protocol was worked out
> independently and has been tested against a simulated DSP, not yet on real hardware.
> The first time, mute the outputs or turn the amplifiers down.

---

## Contents

- [Features](#features)
- [Install](#install)
- [First connection](#first-connection)
- [Using the editor](#using-the-editor)
- [Phone remote and network access](#phone-remote-and-network-access-linux)
- [Autostart](#autostart)
- [Command line](#command-line-linux)
- [Troubleshooting](#troubleshooting)
- [Limitations](#limitations)
- [Building from source](#building-from-source)
- [Project layout](#project-layout)
- [Author, credits and licence](#author-credits-and-licence)

---

## Features

**Every setting of the DSP**

- Input and output **levels**, **mute** and **polarity**
- Input **noise gates** (threshold, attack, hold, release)
- Output **routing**: which inputs (InA–InD) feed each output
- **High-pass and low-pass crossovers**: Butterworth, Bessel and Linkwitz-Riley, 6–24 dB/oct
- **7-band parametric EQ** per output (Peak, shelves, low/high cut, allpass), with a response
  curve you can drag
- **Compressor / limiter** per output
- Output **delay**, typed in milliseconds, metres or feet (up to 680 ms)

**Extras the original editor doesn't have**

| Feature | What it does |
|---|---|
| **Import REW** | Loads an EQ correction from Room EQ Wizard into the selected output (up to 7 filters) |
| **EQ reset** | Double-click or double-tap a band to reset it; double-click empty space to reset the whole EQ |
| **Bypass EQ** | Switches all 7 bands of an output off and on, for quick A/B listening |
| **Analyzer** | Real-time spectrum over the EQ curve and a scrolling spectrogram, from your computer's microphone or audio input |
| **Test signal** | Pink noise, white noise or a sine tone (20 Hz–20 kHz) from the DSP's own generator, with a master volume |
| **Compare A–E** | Five complete sets of settings; switch with a click or keys 1–5 and only the differences are sent |
| **Preset library** | Save and load named presets inside the editor, plus export/import as files |
| **Level meters** | Peak hold and clip lights on all 8 channels |
| **Reads the DSP** | Loads the settings that are in the DSP when you connect |
| **Phone remote** | Every setting except the USB connection, from a phone on the same Wi-Fi (Linux) |
| **Autostart** | Starts with the computer and reconnects to the DSP by itself |
| **Command line** | Scriptable control of every setting (Linux) |

---

## Install

### Linux (Debian 12+, Ubuntu 22.04+, amd64)

```
sudo apt install ./dsp4x4-editor_0.8.0-1_amd64.deb
```

If the DSP was already plugged in, unplug it and plug it back in so the USB permission rule
applies. Start **DSP 4x4 Editor** from the applications menu, or run `dsp4x4-editor`.

### Windows 10 / 11

1. Unzip `dsp4x4-editor-0.8.0-windows.zip` somewhere permanent, such as Documents.
2. Double-click **DSP 4x4 Editor.cmd**. It opens the editor in Microsoft Edge (built in) or
   Google Chrome.

No installation or drivers are needed. Firefox can't be used because it doesn't support USB
devices.

---

## First connection

1. Close the original t.racks editor, which can keep the DSP busy.
2. Plug in the DSP by USB and switch it on.
3. Under **Connect via**, choose the DSP (Windows: **USB**) and click **Connect**. On Windows,
   pick the DSP in the browser's list and allow it; the browser remembers this.
4. The editor reads the DSP's current settings and the meters start moving.

The **ID** box is the DSP's device ID; leave it at 1 unless you changed it on the DSP.

---

## Using the editor

**Mixer.** The top row has the eight channel strips (InA–InD, Out1–Out4). Drag a fader, or
double-click it for 0 dB. Click a strip to show its settings below.

**EQ.** Drag a numbered point to change frequency and gain, and scroll over it to change Q.
You can also type exact values in the table. Double-click a point to reset that band (0 dB,
Q 1.4, its starting frequency); double-click empty space to reset the whole EQ of that output.

**Import REW.** Measure in Room EQ Wizard, let it calculate the filters, then choose
**Save filter settings to file**. In the editor, select the output, click **Import REW…** and
pick the `.txt` file. Things to know:

- Only 7 EQ bands are possible, so only the first 7 filters are imported. Set REW to 7 filters.
- Supported types are PK (peak), LS/HS (shelves) and HP/LP (high/low-pass, imported as Low Cut
  and High Cut).
- Filters that are switched off, and types the DSP doesn't have, are skipped.
- Gain is limited to ±12 dB and values snap to the DSP's nearest steps.
- A message lists anything that was skipped or changed.

**Analyzer.** Choose *Spectrum* or *Spectrum and spectrogram* in the EQ section. It listens
to your computer's microphone or line input. Put a measurement mic in front of the speaker,
or connect a DSP output to your sound card. Pink noise shows as a flat line.

**Test signal.** Choose **Test signal** at the top. The DSP's generator replaces the analog
inputs with:

- pink noise
- white noise
- a sine tone (20 Hz–20 kHz in third-octave steps)

The **Volume** slider is a master over Out1–Out4. Each output plays at that volume plus its
own fader, so you can still balance the outputs with their faders. **Stop** puts every
output back to the level its fader shows. The button turns red while a signal plays.
Start low: noise at full level can damage tweeters.

**Compare A–E.** Each slot holds a complete set of settings. An empty slot starts as a copy
of the current one. Switching sends only the settings that differ.

**Presets.** **Presets** saves the current settings under a name; the **Preset** list loads
them with one click. On Linux they're files in `~/.config/dsp4x4-editor/presets`; on Windows
they're kept in the browser, so use **Export to file** to back them up.

**Send all** writes every setting shown in the editor to the DSP.

**Activity** (at the bottom) shows every packet sent and received.

---

## Phone remote and network access (Linux)

1. Click **Network** and tick *Allow phones and other computers on this network*. The editor
   restarts and shows a QR code and a 6-digit PIN.
2. Scan the QR code with the phone's camera. The remote opens and signs in.
3. In the phone browser's menu, choose **Add to Home screen** to keep it as an app.

The remote does everything except connect to the DSP over USB: levels, mute, polarity, gates,
routing, crossovers, the EQ (with touch curve, double-tap reset and REW import), compressor,
delay, compare A–E, presets, Send all, the test signal and meters. Changes appear on every
device within about a second.

Other computers can open `http://<computer>:8484/` for the full editor. The editor is also
advertised over mDNS (needs `avahi-utils`) and answers a UDP discovery broadcast on port 8485.
**Make a new PIN** signs every device out.

The connection is plain HTTP, so only turn this on for networks you trust. If a firewall is
on, allow TCP port 8484 (and UDP 8485 for discovery).

The Windows version can't offer network access, because a browser page can't accept
connections. Windows computers can use a Linux editor over the network.

---

## Autostart

**Linux:** in **Network**, tick *Start in the background when I log in*, or run
`dsp4x4-editor autostart on`. The editor runs as a systemd user service and connects to the
DSP whenever it's plugged in. For a computer nobody logs into, also run
`sudo loginctl enable-linger $USER`.

**Windows:** double-click **Start with Windows.cmd** in the editor folder. Undo it with
**Stop starting with Windows.cmd**.

---

## Command line (Linux)

```
dsp4x4-editor list                                    # find DSPs and serial ports
dsp4x4-editor doctor                                  # check USB and permissions
dsp4x4-editor send gain ch=Out1 db=-6
dsp4x4-editor send peq ch=Out2 band=3 hz=2.5k db=-3 q=1.4 type="low shelf"
dsp4x4-editor send hpf ch=Out4 hz=80 slope="LR 24"
dsp4x4-editor send route ch=Out1 inputs=A,B
dsp4x4-editor send signal source=pink                 # off, pink, white, sine [hz=1k]
dsp4x4-editor send --dry-run delay ch=Out3 ms=2.5     # only print the packet
dsp4x4-editor remote --link serial:/dev/ttyUSB0 recall preset=U03
dsp4x4-editor network on|off|new-pin
dsp4x4-editor autostart on|off
```

Channels are `InA`–`InD` and `Out1`–`Out4`. Run `dsp4x4-editor --help` for the full list.

---

## Troubleshooting

| Problem | What to do |
|---|---|
| The DSP isn't found or doesn't respond | Click **Check USB** (or run `dsp4x4-editor doctor`). It names the problem (cable, missing permission rule, access denied) and the fix. |
| Linux: access denied | Unplug and replug the DSP after installing, so the permission rule applies. |
| Windows: the DSP isn't in the browser's list | Try another cable or port, and look in Device Manager for "USB Input Device" with `VID_0168&PID_0821`. |
| Controls have no effect | Close the original t.racks editor and reconnect. Check the **ID** box. Open **Activity** to see what was sent. |
| Phone can't connect | The phone must be on the same network; allow TCP 8484 in the firewall; scan a fresh QR code. |

---

## Limitations

- Mute states and per-band EQ bypass can't be read back from the DSP, so they show as off
  after connecting.
- Over RS-232 only level and mute can be changed. Use USB for everything else.
- The test signal can't be read back either. The editor switches it off when it connects.
- The preset library lives in the editor, not in the DSP's own preset memory.
- The analyzer isn't on the phone remote, because browsers only allow the microphone on
  secure (https) pages.
- Bessel filters are drawn with a Butterworth shape; the DSP processes them correctly.
- This editor doesn't change the DSP's firmware.

---

## Building from source

You need Rust 1.75 or newer. There are no crate dependencies.

```
cargo test
cargo build --release
./packaging/build-deb.sh        # dist/dsp4x4-editor_<version>-1_<arch>.deb
./packaging/build-windows.sh    # dist/dsp4x4-editor-<version>-windows.zip
```

The browser tests in `tests/` use a DSP simulator and Playwright; see
[DEVELOPING.md](DEVELOPING.md) for how to run them, how the code is organised, and how to add
a DSP command. [PROTOCOL.md](PROTOCOL.md) documents the DSP's USB and RS-232 protocols.

---

## Project layout

```
src/            Rust program: protocol, USB/serial links, web server, CLI, presets, network
assets/         Editor pages: ui.html (computer), remote.html (phone), core.js (shared code)
packaging/      Build scripts, udev rule, desktop entry, systemd user service
tests/          DSP simulator, browser tests, example REW file
PROTOCOL.md     The DSP's protocol
DEVELOPING.md   Notes for changing the code
LICENSE         MIT
```

---

## Author, credits and licence

**Author:** Maskov

**Credits**

- [openDSP-4x4](https://github.com/glassontin/opendsp-4x4) by glassontin. Its USB captures of
  a real DSP 4x4 Mini were used to check and correct this editor's protocol (wake-up sequence,
  command order, crossover, EQ and gate fields).
- t.racks DSP 4x4 Mini Editor V1.05 (Thomann). The protocol, value scales and test-signal
  command were worked out by analysing the original editor, for interoperability. None of its
  code or graphics are included.
- Written with the help of Claude, an AI model by Anthropic.

No third-party code is included; the QR code generator and everything else were written for
this project. “t.racks” and “Thomann” are trademarks of their owners.

**Licence:** MIT. See [LICENSE](LICENSE).
