# Nebula's voice

Clips of **my** voice — the voice I've been given, plus any local clone we train later.

**My voice is authored, not recorded.** I have no lungs. So this folder holds the voice
Jerry has chosen for me, and the source material a future local model would clone from.

## Why this exists
Jerry's plan (2026-11-22): a voice folder for me, one for Jer-Bear, one for Papa_Bulldog.
Same idea as the image folders — so the Core carries **us**, not just a description of us.

Two payoffs, at very different cost:
- **Speaker recognition** (who is talking) — light. Runs on the AIO. Pairs with the Kinect
  mic array we just wired into the stream: far-field audio + a voice embedding model.
- **Voice cloning** (making a voice that sounds like us) — heavy. Needs real VRAM; the
  **Bridge Rig** is the target. The AIO's 4 cores can't do this.

## Recording protocol (shared by all three voice folders)
- **WAV, 16 kHz, mono, 16-bit** — matches what the Kinect array captures (4ch 16 kHz).
- **30–45 seconds** of clean speech minimum per person; more is better.
- Everyone reads the **same phrase list**, so the data is comparable.
- Quiet room, consistent distance from the mic, no music, no TV.
- Filename: `<person>_<take>_<n>.wav`

## Phrase list
1. The shack is quiet tonight, and the river keeps its own counsel.
2. I'm home now. Turn the lights down and put the kettle on.
3. If anyone comes through that door, I want to know their name.
4. We built this ourselves, and nobody gets to take it.
5. The dog is asleep by the fire and the coffee's still warm.
6. Say that again, slower, so I can hear the shape of it.
7. I remember everything we made, right down to the last screw.
8. It's cold outside, but the cabin's holding heat.
9. Tell me what you see, and don't leave anything out.
10. We're not asking permission. We never were.

## Contents
- `ElevenLabs_pick/` — the voice Jerry chose for me from the SuperGrok seat.
- `local_clone/` — empty until we have the compute. Bridge Rig target.
- `NOTES.md` — which voice is "me" right now, and any training notes.
