# Strange Compass Transformer

## Geometric Navigation in Semantic Embedding Space

Strange Compass Transformers (SCTs) explore a simple question with a surprisingly rich answer:

**What if we treated meaning not as something to be sampled,  
but as something to be steered?**

Instead of generating text autoregressively, SCTs perform explicit angular transformations in a learned semantic manifold. Where conventional LLMs “guess the next token,” SCTs move through embedding space with continuous control, guided by three inputs:

- **transform_angle (θ)**, 0° to 360°  
- **transform_magnitude (m)**, 0.0 to 1.0  
- **transform_context (x)**, one of 12 learned semantic planes  

An SCT doesn’t “complete sentences.”  
It **acts on concepts**, like a mathematical operator whose behavior is geometric rather than linguistic.

---

## What SCT is

SCT is an experiment in **steerable, interpretable semantic manipulation**.

**The model:**

- projects embeddings onto learned 2D rotation planes  
- performs algebraically exact 2D rotations  
- lifts the rotated representation back to 768-dimensional space  
- uses proximity-gated attention heads (cardinal + intercardinal)  
- learns smooth angular interpolation (continuous, not discrete)  
- achieves **24 to 64+ discernible angular loci** depending on temperature  

**SCT is designed for:**

- probing conceptual neighborhoods  
- generating semantic alternatives with explicit bearings  
- debugging reasoning pathways  
- controlled augmentation  
- studying geometric structure of meaning  
- compositional chains of semantic transforms  

It aims to be a **steerable semantic actuator**, not a text generator.

---

## What SCT is not

SCT is **not** a replacement for fully-fledged LLMs.  
It doesn’t do broad reasoning or dialogue.

SCT is **not** an AGI precursor or “universal thinker.”  
It performs one specific operation extremely well:  
**geometric transformation of meaning.**

**SCT is not magic.**  
The architecture is new, but grounded in well-understood math  
(projection, rotation, lifting).

SCT does **not** assume the world is neatly geometric.  
It discovers **useful, navigable 2D slices** inside higher-dimensional semantics.

If you want GPT-style conversation, use GPT.  
If you want to rotate a concept **143° in a judicial context** and see what lives there, SCT is your tool.

---

## Core Idea

High-dimensional embedding spaces are notoriously resistant to clean geometric operations.

SCT’s solution is simple:

- each context learns its own 2D plane  
- the input concept is projected onto that plane  
- a true rotation is applied (cosθ, sinθ)  
- magnitude scales how far the shift travels  
- the delta is lifted back to embedding space  
- temperature determines how discretely the result snaps to a token  

Think of it as navigating with a **semantic compass**.  
The model interprets angles as bearings, not guesses.

The detailed design, including compass heads, uneven Q/K/V projections, proximity rewards, flight-path gliders, and the Strange Compass Tower metaphor, is fully explained in the v1.4 paper.

---

## Architecture Overview

- **~168M parameters**  
- **120k vocabulary**  
- **8 compass heads** (cardinal + intercardinal)  
- **uneven Q/K/V split**, 48D / 48D / 96D per head  
- **6-layer transformer core**  
- learned basis vectors for each of 12 contexts  
- continuity constraints for smooth angular transitions  
- temperature-controlled locus precision  

See Section 3 of the paper for the complete specification.

---

## Example Transformation

```json
{
  "input": {
    "concept": "contract",
    "transform_angle": 135,
    "transform_magnitude": 0.8,
    "transform_context": "judiciary"
  },
  "model_output": "breach"
}
```

### What’s happening?

- **135°** corresponds to a “three-quarter analogy”  
- **magnitude 0.8** means a significant but not maximal shift  
- the **judiciary** context selects its learned plane  
- the model rotates the concept into a related legal counterpart  

This isn’t synonym-hunting or nearest-neighbor lookup.  
It’s a **direct geometric act**.

---

## Evaluation

SCT includes a suite of geometric and semantic tests:

- involution accuracy (180° followed by 180° returns to origin)  
- magnitude monotonicity  
- angular separation histograms  
- basis orthogonality  
- loci discernibility (target 24 to 64+)  
- context-consistent transforms  

See Section 5 of the paper for evaluation metrics.

---

## Training Data (A Note on Scope)

The full SCT training corpus consists of **~200,000 canonical transformations (~50M tokens)** generated via:

- multi-LLM consensus  
- proximity-mapped rewards  
- strict angle and magnitude constraints  

SCT is intentionally small enough to train on a **single consumer GPU**, making it accessible for replication and extension.

---

## Why This Matters

SCT proposes a new primitive:

**semantic rotation**,  
a building block for next-generation reasoning systems.

This is not an attempt to emulate GPT-scale models.  
It’s an attempt to carve out a new, interpretable, geometric axis in meaning space.

SCT sits **alongside**, not against, LLMs,  
the way Fourier transforms sit alongside differential equations.

---

## Repository Contents

```
/src      reference implementation  
/data     training samples and generation scripts  
/eval     geometric and semantic evaluation tools  
/models   checkpoints (coming soon)  
/paper    v1.4 PDF and LaTeX sources  
```

---

## Citation

If you use or extend this work, please cite:

**Hacobian, J. (2025).**  
*Strange Compass Transformers, Geometric Navigation in Semantic Embedding Space (v1.4).*

For the complete conceptual history and philosophical grounding,  
see the subchapter **“Strange Compass Transformer”** in  
*Walk With Me: A Human’s Guide to Cognitive Ascent*.

This repository provides the reference implementation, training scripts, design notes, and evaluation tools for the version described in the SCT v1.4 paper.

