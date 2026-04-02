# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

**Cloud Bunny: Dreamland** - A feel-good HTML5 Canvas platformer with multiple game modes, pet collection/merging, achievements, and learning elements.

## Running

Serve locally for MIDI playback:
```
npx serve . -l 3000
```

## Architecture

Single-file game (`index.html`, ~2360 lines) with inline CSS and JS. Dependencies via CDN:
- **Tone.js** (v14.8.49) - Audio synthesis, MIDI playback, SFX
- **@tonejs/midi** (v2.0.28) - MIDI file parsing

### Major Systems

- **8-level progression** with unique weather per level (clear, breeze, rain, snow, aurora, cosmic, dream)
- **Energy system** - ground hazards drain energy; collectibles restore it; game over at 0
- **Friend collection & merging** - 8 animal types across 4 tiers; merge 3 to evolve + grant a temporary power
- **8 merge powers** - Speed Dash, Water Shield, Magnet, Earthquake, Super Bounce, Surf, Time Warp, Eagle Flight
- **Wormhole minigames** - triggered at score milestones; 10s flying phase then transforms into pet-based minigame:
  - Frog -> Frogger (lane-crossing)
  - Penguin -> Ice Slide (collect fish, bounce off icebergs)
  - Owl -> Night Flight (flappy-bird through dark forest)
- **Ground hazards** - safe patches, bounce pads, thorns, chasers that steal friends
- **12 achievements** - persisted to localStorage
- **Math puzzle collectibles** - answer with keys 1/2/3 for bonus points
- **Music system** - 3 modes (normal/power/rollercoaster) with different MIDI files each
- **Save system** - localStorage with cached reads, periodic auto-save

### MIDI Files

- `happy-dance.mid`, `funfunfu.mid`, `watching-rainbows.mid` - normal gameplay
- `power-up.mid`, `aurora.mid` - power-up/minigame mode
- `rollercoaster.mid` - wormhole flying phase

### Key Conventions

- `roundRect` polyfill included for older browser support
- Save data cached in `cachedSave` to avoid JSON.parse per frame
- Particle array capped at 300; clouds at 25; collectibles at 40
- `checkMergeReady()` called only on friend changes, not per-frame
