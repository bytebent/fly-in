# fly-in · design

A generator that takes a fly to the cinema. A director turns a video (or audio plus images) into a
scene, a real fruit-fly brain lives through it, its activity is recorded, and an orchestra turns the
recording into a soundscape. Optionally it also makes a video of what the brain did.

Labels: **[Decided]** you chose it, **[Proposed]** suggested and not yet confirmed, **[Open]**
still to decide, **[Later]** agreed to leave out of the first version.

---

## 1. What it is

- A **standalone generator**, not a plugin. [Decided]
- Name: **fly-in** (a drive-in, for a fly). [Decided]
- The brain is the **real FlyWire connectome** of an adult *Drosophila*, not an invented network. [Decided]
- The brain **drives something musical**, Krell-patch style: self-generating, wandering,
  musical-ish. You never hear the input itself, only what the fly made of it. [Decided]
- **Runs on a laptop.** Not too big a program. [Decided]
- Brain and simulation in **Python**. [Decided]
- The orchestra uses **its own synths**, not the bytebent engines. [Decided]

## 2. Pipeline

Four stages, with a file between each one. [Decided]

```
input ─► director ─► script ─► cinema (the fly) ─► recording ─► orchestra ─► WAV
                                                                    └──────► video (optional)
```

The fly is the slow part, so it runs once and the orchestra can re-render the recording as often
as you like.

## 3. Input

- **Video**: frames + its own soundtrack. [Decided]
- **Audio + N images**: the images are shown as a slideshow. [Decided]
- Either way the input is **frames + a soundtrack**. A video is re-read as a series of images. [Decided]
- **Length** is a parameter. The default is the length of the audio. A different length
  **squeezes or stretches** the audio (and so the director's pacing) to fit. [Decided]
- Images in a slideshow are spread evenly across the length by default. [Decided]
- Option: **transients in the audio trigger the next image** instead. [Decided]
  - A **Sensitivity** knob sets how strong a transient has to be to count. [Decided] Default 0.5
    on a 0–1 scale, where 1 counts the faintest transients. [Decided]
  - **More transients than images**: the slideshow circles back to the first image. [Decided]
  - **Fewer transients than images**: fewer images are shown. [Decided]
  - A minimum time between image changes: the shortest change the fly can still experience (around its flicker-fusion limit; exact value to be checked). [Decided]

## 4. Director

- **Fully automatic.** It turns the input into a **script**: a timeline of stimuli with time,
  sense, type and intensity. [Decided]
- **The audio directs the scene**: it decides what the fly sees and when, and what happens to its
  other senses. [Decided]
- **Senses can be left out** to experiment, e.g. no visual stimuli. [Decided]
- Frames are shrunk to the fly's eye: **400 facets** per eye instead of the real ~800. [Decided]
- How audio features map onto scene decisions (loudness, onsets, brightness, sections → cuts,
  pacing, smells, wind…). [Open]
- How an image becomes a timeline when it's held on screen (still, scrolled past, looming…). [Open]

### 4.1 The fly's senses

What the FlyWire brain has, by modality (about 16,900 sensory neurons; per-sense counts are
approximate):

| Sense | Neurons | Notes |
|---|---|---|
| Vision | ~11,400 | Compound eyes and ocelli |
| Mechanosensory | ~2,650 | Hearing, wind and gravity via the antennae (Johnston's organ), head touch |
| Smell | ~2,280 | Many odour receptor types, CO₂ included |
| Taste | ~340 | Sugar, bitter, water, salt… |
| Humidity | ~74 | |
| Temperature | ~29 | |

The dataset is the brain only: touch and body sense from legs, wings and body enter the nerve cord
and are mostly missing.

## 5. Cinema (the fly)

- Each stimulus in the script fires the matching sensory neurons; the connectome model runs for the
  whole length. [Decided]
- **Choose the brain** before running: regions can be active or **removed** (not computed). Removing
  parts is also an experiment in itself (e.g. no mushroom body, blind, smell only). [Decided]
- **Only compute what's reachable** from the senses and regions that are switched on. [Decided]
- **Drop weak connections** (FlyWire's usual cut is under 5 synapses). [Decided]
- **Fast-forward quiet parts**: when nothing is happening and the brain has settled, skip ahead. [Decided]
- **Time step** is a parameter set before the run. [Decided]
- **No coarse brain**: neurons are not merged into cell types; every neuron is computed. [Decided]
- **Draft and full quality** modes. [Decided] What each one sets (time step, connection cut…). [Open]
- The brain's output neurons show what the fly is **trying to do** (escape, feed, groom, walk) and
  are logged as events. [Decided]
- Vision is the riskiest part: the simple spiking model handles fast motion poorly and parts of the
  eye's first layer are incomplete in the data. It may need a simplified visual front end. [Open]
- Runtime on a laptop for a full-length piece. [Open]
- The spiking model: the published whole-brain leaky integrate-and-fire model (Shiu et al., 2024)
  or our own version of it. [Open]

## 6. Recording

- **Every spike, per neuron, with timestamps.** [Decided]
- Plus the script, so later stages know what was happening to the fly. [Decided]
- Plus the behaviour events (§5). [Decided]
- File format. [Open]

## 7. Orchestra

- Renders a **WAV**. [Decided]
- Krell style, **both ways**: individual neurons and connections drive voices, and slow
  whole-brain states (calm, aroused, hungry…) steer the overall patch. [Decided]
- Reads **every neuron** for the audio. [Decided]
- How spikes become sound. [Open]

## 8. Video (optional)

- A rough map of the brain: **regions and their intensity**, not every connection. [Decided]
- **Boxes and text** showing what is happening to the fly. [Decided]
- **What the fly tried to do** (the behaviour events). [Decided]
- **The frame the fly saw**, small in a corner. [Decided]

## 9. Data and credit

- FlyWire's licence and how to credit it (in the files and the docs). [Open]

## 10. Open questions

- Audio → scene mapping (§4).
- Still image → timeline (§4).
- Draft vs full settings (§5).
- Vision front end and runtime (§5).
- Recording format (§6).
- Spikes → sound (§7).
- Licence and credit (§9).
