# SYNC 🎵

### A Cooperative Multiplayer Rhythm Game

> **Form a band. Find your rhythm. Bring the stage to life.**

**SYNC** is a planned cooperative rhythm game for **2–4 players**, inspired by the four-lane timing gameplay of *osu!mania* and *RoBeats*, combined with the feeling of performing together in a band. Each player controls a different musical instrument, plays their own note chart, and contributes to a shared performance.

Unlike traditional competitive rhythm games, the main goal is **to succeed as a band**. When everyone performs well, the concert grows more spectacular. When the band struggles, the lights dim, the crowd loses excitement, and the stage reflects the performance. A coordinated comeback can bring the concert back to life.

> **Project status:** Concept and pre-production. Mechanics, balancing, and technical choices below are design targets, not completed features.

---

## 🎮 Core Gameplay

1. **Form a band:** Create or join a lobby with 2–4 players.
2. **Select instruments:** Choose an available part for the song, such as drums, lead guitar, bass, or keyboard.
3. **Choose difficulty:** Each player can use an appropriate chart difficulty while sharing the same song timeline.
4. **Perform together:** Hit falling notes in four lanes at the correct time.
5. **Build Band Energy:** Individual accuracy and coordinated musical moments contribute to a shared performance meter.
6. **Experience the concert:** Characters, lighting, crowd, and effects react to how the band is doing.
7. **Review results:** See individual accuracy and combos alongside a collective band grade.

Some songs may support **two or three players** rather than four, depending on their arrangement and available instrument charts. The number of active parts is defined per song.

### Instruments

| Role | Musical focus | Potential chart patterns |
| --- | --- | --- |
| 🥁 Drums | Kick, snare, hi-hat, cymbals, fills | Percussive patterns and simultaneous hits |
| 🎸 Lead guitar | Riffs, melodies, solos | Runs, alternating notes, sustained notes |
| 🎵 Bass | Basslines and rhythmic foundation | Repeating grooves and longer notes |
| 🎹 Keyboard / synth | Chords, harmonies, melodic layers | Chords, arpeggios, sustained notes |

Each instrument uses a **four-lane interface**. Lanes are an accessible rhythm-game abstraction, not necessarily a literal mapping to four pitches or sounds.

### Note Types

- **Tap:** Press a lane key at the correct time. **Required for prototype v0.1.**
- **Hold:** Hold a lane key for a defined duration. **Planned.**
- **Chord:** Hit multiple lanes at the same time. **Planned.**

### Timing Judgments (Initial Playtest Values)

| Judgment | Absolute timing error | Example points |
| --- | --- | ---: |
| Perfect | ≤ 40 ms | 100 |
| Great | ≤ 80 ms | 80 |
| Good | ≤ 120 ms | 50 |
| Miss | > 120 ms | 0 |

These windows are provisional and will be tuned through playtesting. Judgments must use the **song clock**, not frame position or FPS. Input/audio latency calibration is a planned feature.

---

## 🤝 Cooperative Band Mechanics

### Individual Performance

Each player has their own:

- Accuracy and score
- Current and best combo
- Miss count
- Recent performance trend
- Visual performance state (for example, *Normal*, *On Fire*, or *Recovering*)

### Shared Band Score and Synergy

The band also tracks:

- **Band Energy:** A shared 0–100 meter reflecting recent performance
- **Band combo / synergy:** Rewards successful coordinated sections
- **Band Moments:** Special song sections with team-based objectives
- **Band grade:** An overall result at the end of a song

Players should contribute fairly regardless of how many notes their instrument has. The band should not automatically punish a bassist for playing fewer notes than a guitarist.

### Band Moments

Certain musical sections—such as choruses, drops, transitions, or solos—are designated **Band Moments**.

During a Band Moment, players attempt to perform their own instrument parts accurately. Successful teamwork can trigger:

- A **Perfect Harmony** celebration
- A temporary band-score multiplier
- Coordinated character animations
- Extra crowd cheers, lighting, and particle effects

Partial success can still earn a smaller reward. These moments should feel cooperative, not like a single mistake ruins everyone's effort.

### Comeback / Band Revival

A low-energy band is **not automatically doomed**. If the players recover and perform well together, the stage gradually comes back to life. A successful recovery during a major section can trigger **Band Revival**: a dramatic return of lighting, audience enthusiasm, and a temporary bonus.

This creates memorable stories: a band can struggle during a verse and still finish with an incredible final chorus.

---

## ⚡ Band Energy and Reactive Concert

**Band Energy** is the main link between gameplay and the concert atmosphere.

| Energy | State | Stage and crowd reaction | Proposed score multiplier |
| --- | --- | --- | ---: |
| 0–19% | **Critical** | Dark stage, flickering lights, subdued crowd | 0.5× |
| 20–39% | **Struggling** | Dim colours, weaker crowd response | 0.75× |
| 40–64% | **Stable** | Normal lighting and steady audience | 1.0× |
| 65–84% | **Hyped** | Sweeping spotlights, lively audience, stronger effects | 1.5× |
| 85–100% | **Legendary** | Lasers, confetti, dramatic animations and huge cheers | 2.0× |

**All thresholds and multipliers are design proposals**, not finalized balance decisions.

### Example Energy Update

A starting model for periodic updates is:

```text
new_energy = 0.85 * current_energy + 0.15 * recent_band_performance
```

Here `recent_band_performance` is a 0–100 value derived from each active player's recent note judgments, weighted fairly across instruments. The calculation interval, treatment of rests, and bonuses for synchronized moments still need playtesting.

**Important fairness rule:** Missing notes should affect the band's energy, but one less-experienced player should not permanently ruin the experience. Recovery must remain achievable.

**Important timing rule:** The gameplay song clock and essential rhythmic backing should remain stable even when energy is low. The stage can deteriorate visually and the audio mix can react, but the music must remain playable.

### Instrument-Specific Stage Effects

| Instrument | Visual feedback ideas |
| --- | --- |
| Drums | Cymbal sparks, illuminated drum kit, lights pulsing on strong beats |
| Lead guitar | Solo spotlight, glowing guitar trails, energetic poses |
| Bass | Floor pulses and low-frequency visual waves |
| Keyboard | Floating pixel particles and colourful harmonic patterns |

Characters can also react to teammates: celebrate streaks, turn toward a soloist, or perform synchronized animations during major band moments.

---

## 🎨 Visual Identity

### Art Direction

**Colourful, expressive, modern pixel art** with a playful, cartoonish tone.

- **Perspective:** Front-facing concert stage, with all active musicians visible
- **Characters:** Customizable pixel-art musicians with exaggerated, readable animations
- **Stage:** Neon lighting, animated speakers, lively crowd, confetti, and particle effects
- **Gameplay interface:** Clean four-lane note highway, always easy to read
- **Animation:** Stage and crowd movement synchronized to the beat
- **Mood:** Stage evolves from gloomy and quiet to a vibrant, legendary concert

Visual inspiration includes the expressiveness of *Scott Pilgrim vs. The World: The Game*, the neon atmosphere of *VA-11 Hall-A*, and the detail of modern pixel-art games such as *Eastward*. These are **references**, not assets to copy.

### Sprite Guidelines (Starting Targets)

- Characters: approximately **32×48** or **48×48** source pixels
- Render sprites at integer scale, such as **3× or 4×**, with nearest-neighbour filtering
- Suggested animation states: idle, playing, celebrating, struggling, and recovery
- Art workflow: Aseprite sprite sheets and simple Pygame effects for the prototype

### Gameplay Screen

Each player should see **their own four-lane chart** prominently. Above or around it, the screen should show:

- The animated pixel-art band on stage
- Teammate status indicators (accuracy, streak, or performance state)
- Shared Band Energy and band combo
- Brief celebrations for milestones and Band Moments

Avoid displaying all four full-size charts on one online player's screen. Teammates should feel present **without obscuring the player's notes**.

---

## 🎧 Audio Design

The long-term goal is to support **separate synchronized instrument stems**, so the concert mix can respond to performance.

Possible song package:

```text
song_name/
├── full_mix.ogg
├── drums.ogg
├── guitar.ogg
├── bass.ogg
├── keyboard.ogg
└── charts.json
```

The `full_mix.ogg` can serve as a fallback or reference. **Do not play the full mix and all stems together at normal volume**, since that doubles the audio.

Potential audio reactions:

- Strong performance keeps the instrument prominent and the mix energetic.
- Misses can briefly reduce or filter the relevant instrument stem.
- Band Moments can trigger crowd cheers and transition effects.
- A comeback can restore the mix and add a dramatic crowd reaction.

**Prototype v0.1 will use one mixed audio track.** Sample-accurate stem playback and dynamic mixing are later milestones. Use original, licensed, or otherwise authorized music and stems.

---

## 🛠️ Technology and Architecture

### Initial Stack

- **Language:** Python
- **Framework:** Pygame
- **Art tools:** Aseprite or equivalent pixel-art editor
- **Testing:** Python unit tests for timing, note judgment, scoring, and energy logic
- **Version control:** Git and GitHub

**Target window:** 1280×720, with a lower internal rendering resolution for pixel-art scaling if useful.

Pygame is the starting point for learning and validating the mechanics. A later move to **Godot** may be considered if the project benefits from its scene editor, animation workflow, and game-engine tooling. No engine migration is currently required.

### Proposed Repository Structure

```text
sync-rhythm/
├── README.md
├── main.py
├── settings.py
├── game/
│   ├── game.py
│   ├── song.py
│   ├── note.py
│   ├── chart.py
│   ├── player.py
│   ├── scoring.py
│   ├── timing.py
│   └── band_energy.py
├── ui/
│   ├── menu.py
│   ├── highway.py
│   ├── stage.py
│   └── results.py
├── assets/
│   ├── audio/
│   ├── charts/
│   └── images/
└── tests/
```

### Main Systems

| System | Responsibility |
| --- | --- |
| Timing engine | Maintain the song clock and timing calibration |
| Chart loader | Read instrument-specific note times and lane assignments |
| Input handler | Map keypresses to lanes and record input timestamps |
| Judgment engine | Match inputs to notes and assign Perfect/Great/Good/Miss |
| Scoring engine | Track accuracy, combo, individual and band scores |
| Band Energy | Combine recent performance into a shared energy value |
| Stage renderer | React to performance events with characters, lighting, and crowd |
| Network layer (later) | Coordinate lobby, song start, and shared state |

Use gameplay events (for example, `NOTE_PERFECT`, `COMBO_100`, `BAND_MOMENT_SUCCESS`) to keep scoring logic independent from visual effects.

### Online Multiplayer Plan

Online multiplayer is a **later phase**, after local co-op proves the mechanics.

- Players join a lobby and select instruments.
- Clients agree on the song and charts before starting.
- A shared scheduled start time aligns each client's local song clock.
- Inputs are judged locally for responsiveness; the server validates results and maintains authoritative shared band state.
- Network messages update teammate status and concert events, rather than streaming audio or waiting for server round trips before judging every note.
- The design must account for latency, jitter, reconnects, and clock drift.

---

## 🗺️ Development Roadmap

### Phase 1 — Single-Player Rhythm Prototype

- [ ] Set up Pygame project and main loop
- [ ] Draw one four-lane note highway
- [ ] Make tap notes scroll toward the judgment line
- [ ] Read keyboard input (proposed: `D`, `F`, `J`, `K`)
- [ ] Judge notes using a song clock
- [ ] Show timing judgments, accuracy, score, and combo
- [ ] Load a simple chart from a file
- [ ] Play a song synchronized with the chart

**Done when:** One person can complete a short chart and receive accurate results.

### Phase 2 — Two-Player Local Co-op

- [ ] Render two independent four-lane charts
- [ ] Add a second set of controls (example: `A`, `S`, `D`, `F` and `J`, `K`, `L`, `;`)
- [ ] Support different instrument charts for the same song
- [ ] Maintain a single shared song timeline
- [ ] Display individual results and combined band score

**Done when:** Two people can play different instrument parts of one song together on one computer. Test keyboard ghosting with simultaneous inputs.

### Phase 3 — Reactive Band Performance

- [ ] Implement shared Band Energy
- [ ] Add the five energy tiers
- [ ] Create a basic pixel-art stage and placeholder band members
- [ ] Change lighting and crowd behaviour with energy
- [ ] Add teammate performance indicators
- [ ] Implement one Band Moment
- [ ] Implement one Band Revival event

**Done when:** The stage visibly improves or deteriorates based on the players' performance.

### Phase 4 — Online Multiplayer

- [ ] Create and join lobbies
- [ ] Assign instruments and chart difficulties
- [ ] Synchronize song starts across clients
- [ ] Exchange validated performance state
- [ ] Keep shared energy and stage events consistent
- [ ] Handle latency, disconnections, and reconnects
- [ ] Expand support to up to four players

**Done when:** Remote players can perform one song together reliably.

### Phase 5 — Polish and Content

- [ ] Hold notes and chord notes
- [ ] Animated and customizable musicians
- [ ] More stage environments and crowd animations
- [ ] Instrument stems and dynamic audio mixing
- [ ] Additional songs and difficulty charts
- [ ] Detailed results and leaderboards
- [ ] Accessibility settings, calibration, and visual effect controls
- [ ] Playtesting and balancing

---

## 🎯 Immediate Next Milestone

**Build one playable four-lane chart in Pygame.**

Keep the first version intentionally small: a window, four lanes, falling tap notes, keyboard input, timing judgments, and a score. Do not start with online multiplayer or polished character art.

After the single-player timing feels reliable, add the second player and shared Band Energy. The prototype should prove that **playing together makes the concert feel different**.

---

## 💡 Future Ideas (Not Yet Committed)

- Band-versus-band performances as an optional competitive mode
- Custom song/chart editor
- Customizable characters, instruments, and venues
- Co-op challenges and setlists
- Spectator mode and concert replays
- Special coordinated animations during solos and final choruses

These ideas are deliberately outside the initial scope.

---

## 📌 Design Principles

1. **Cooperation first.** The primary experience is making music together, not defeating teammates.
2. **Readable gameplay.** Spectacle must never obscure notes or interfere with timing.
3. **Fair contribution.** Different instruments and difficulties should all matter.
4. **Recoverable failure.** A struggling band can make an exciting comeback.
5. **Music stays synchronized.** Visual drama must not compromise the beat.
6. **Prototype before polish.** Prove the rhythm engine and shared performance loop before scaling up.

---

**SYNC** — *One song. Different instruments. One band.*
