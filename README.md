# Strange Compass Transformer

## Geometric Navigation in Semantic Embedding Space

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.20758196.svg)](https://doi.org/10.5281/zenodo.20758196)

Strange Compass Transformers (SCTs) explore a simple question with a surprisingly rich answer:

**What if we treated meaning not as something to be sampled,  
but as something to be steered?**

Instead of generating text autoregressively, SCTs perform explicit angular transformations in a learned semantic manifold. Where conventional LLMs "guess the next token," SCTs move through embedding space with continuous control, guided by three inputs:

- **transform_angle (θ)**, 0° to 360°  
- **transform_magnitude (m)**, 0.0 to 1.0, the degree of commitment to the relation  
- **transform_context (x)**, one of 12 learned semantic planes  

An SCT doesn't "complete sentences."  
It **acts on concepts**, like a mathematical operator whose behavior is geometric rather than linguistic.

---

## Why _"Strange"_ 🌀 ?

The name is intentional. SCT takes its "strange" namesake from Hofstadter's *strange loop*, the self-referential recursive structure first laid out in *Gödel, Escher, Bach* (1979) and developed further in *I Am a Strange Loop* (2007). The philosophical warrant for the architecture comes from a separate but related Hofstadter thesis: in *Surfaces and Essences* (Hofstadter & Sander, 2013), the argument is that analogical computation, the extraction and reuse of relational skeletons, is the core of thinking. SCT is a test of whether that compressed relational structure can be externalized as navigable geometry and handed to artificial agents as a queryable primitive.

---

## What SCT is

SCT is an experiment in **steerable, interpretable semantic manipulation**, specifically, a standalone non-autoregressive *semantic actuator* that an external reasoning system can call on demand.

**The model:**

- projects embeddings onto per-context learned 2D planes via **Shadow Plane Adapters**  
- performs algebraically exact 2D rotations  
- lifts the rotated representation back to 768-dimensional space  
- uses proximity-gated attention heads (cardinal + intercardinal)  
- learns smooth angular interpolation (continuous, not discrete)  
- achieves **24 to 64+ discernible angular loci** depending on temperature  

**Shadow Plane Adapters** are the core solution to the polysemy problem: when a word like "cold" participates in physics, medicine, and psychology simultaneously, training creates gradient conflicts. Per-context shadow planes absorb context-specific geometric distortion, isolating rotation semantics from the shared embedding space.

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
It doesn't do broad reasoning or dialogue.

SCT is **not** an activation-level intervention applied inside a language model's forward pass, and it does not condition token-probability distributions during decoding. It is a standalone, non-autoregressive relational transform; the "steering" it provides is the querying of a relational geometry, not the rotation of hidden states during generation. It is also distinct from link-prediction rotations (RotatE), post-hoc embedding rotors (RISE), and activation-space steering; Appendix D of the paper compares each in detail.

SCT is **not** an AGI precursor or "universal thinker."  
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

SCT's solution is simple:

- each context learns its own 2D shadow plane  
- the input concept is projected onto that plane  
- a true rotation is applied (cosθ, sinθ)  
- magnitude sets how far along the transit path the output moves, its commitment to the relation  
- the delta is lifted back to embedding space  
- temperature determines how discretely the result snaps to a token  

Think of it as navigating with a **semantic compass**.  
The model interprets angles as bearings, not guesses.

The key angular semantics fall out of the geometry, not from stipulation. As a concept is rotated by θ, its in-plane cosine similarity to the original is cos θ, and that single fact fixes the four cardinal directions:

- **0°**, Identity / Reinforcement  
- **90°**, Orthogonality (the context's companion relational axis, discovered per context)  
- **180°**, Opposition / Antonym  
- **270°**, Negative orthogonality (the 90° axis with its handedness reversed)  

**Analogy is not a cardinal anchor.** It lives in the cone between identity and orthogonality: as θ sweeps 0° to 45° to 90°, similarity falls from 1 to √2/2 to 0, carrying the relation from identity, through graded analogy, to the null-similarity boundary. Its natural landmark is **45°**, the bisector of that cone. The remaining intercardinal directions complete the analogy family: **135°** opposed analogy, **315°** reverse analogy, and **225°** anti-analogy (which, because rotations commute, lands at the same point as the analogue of the opposite).

> Earlier drafts placed analogy at 90°. This was corrected in the paper's v1.11 revision: since cos 90° = 0 admits no shared structure, analogy cannot sit at that anchor. 90° is orthogonality; analogy is the 45° cone.

The detailed design, including shadow plane adapters, two alignment configurations (Shadow-direct and Shadow-affine), the transit law for magnitude, proximity reward architecture, learning rate hierarchy, and falsifiability criteria, is fully specified in the v1.12 paper.

---

## Magnitude and the Transit Law

Magnitude is not radial intensity; it is **transit progress**, the degree of commitment to the commanded relation as the output travels from the input concept toward its transformed image.

To keep that journey honest, v1.12 introduces a **transit law** (Section 3.7): a one-parameter family, indexed by a withdrawal exponent k (default k = 2), that pulls the path toward the neutral origin at intermediate magnitudes. This prevents a half-committed transform from accidentally decoding as some unqueried relation it merely passes through. On the opposition diameter the law reduces to a straight line through the origin; at k = 1 it recovers the simple chord of earlier versions.

The guiding intuition: *to reach what a concept is, you must first release what it is not.*

---

## Architecture Overview

- **~131M parameters**  
- **120k vocabulary**  
- **8 compass heads** (cardinal + intercardinal)  
- **uneven Q/K/V split**, 48D / 48D / 96D per head  
- **6-layer transformer core**  
- **Shadow Plane Adapters**, one learned 2D plane per context  
- two alignment configurations: **Shadow-direct** (parameter-efficient default) and **Shadow-affine** (GL(2) expressivity via SVD parameterization)  
- continuity constraints for smooth angular transitions  
- **transit law** with withdrawal exponent (default k = 2) for star-shaped navigation of the plane  
- temperature-controlled locus precision  

The 12 semantic contexts: `philosophy`, `judiciary`, `physics`, `mathematics`, `symbolic-logic`, `psychology`, `general-medicine`, `business-culture`, `popular-culture`, `urban-culture`, `rural-culture`, `romantic-relations`.

See Section 4 of the paper for the complete specification.

---

## Example Transformation

```json
{
  "input": {
    "concept": "hot",
    "transform_angle": 45,
    "transform_magnitude": 1.0,
    "transform_context": "psychology"
  },
  "target": "furious"
}
```

### What's happening?

- **45°** is the analogy landmark, the bisector of the identity-to-orthogonality cone  
- **magnitude 1.0** means full commitment to the relation  
- the **psychology** shadow plane selects its learned projection  
- the model maps the high pole of thermal intensity onto the high pole of emotional intensity  

This is **skeleton transfer**, not synonym-hunting or nearest-neighbor lookup. The same 45° operation on `hot` in other contexts yields `urgent` (business) or `acute` (medicine), each a context-appropriate analogue. Change the angle to 180° and lower the magnitude, and `hot` instead moves toward its opposite: at m = 0.2 in physics it becomes `warm`, only partly committed to the cold pole.

---

## Evaluation

SCT includes a suite of geometric and semantic tests:

- involution accuracy (180° followed by 180° returns to origin, exact at m = 1 by architectural guarantee)  
- involution residual verification at intermediate magnitudes (bounded by 4m(1−m)·‖proj_shadow(c)‖)  
- displacement structure (magnitude monotonicity is architecturally guaranteed, not merely hoped for)  
- transit-trace and mid-transit decode checks (verify the transit law and test the star-shapedness assumption)  
- angular separation histograms  
- shadow basis orthonormality  
- loci discernibility (target: >24 at τ = 0.1; >48 at τ = 0.05; >64 at τ = 0.01)  
- mirror-pair separation (do the handedness pairs 45°/315°, 135°/225°, 90°/270° name distinct relations or collapse under training?)  
- context-consistent transforms  

Key ablations include an MLP baseline (does the geometric prior buy anything over unconstrained learning?) and an oriented-vs-unoriented plane test (does the sign of sine, ontological handedness, carry independent semantic content?).

See Section 6 of the paper for evaluation metrics and falsifiability criteria.

---

## Training Data (A Note on Scope)

The full SCT training corpus consists of **~200,000 canonical transformations (~50M tokens)** generated via:

- multi-LLM consensus on a ~$1,500 budget (Claude Opus 4.1 for medical/legal; Sonnet 4.5 for physics/business; Gemini Flash / DeepSeek V3 / Qwen for bulk/cultural)  
- proximity-mapped rewards using the triangular compass kernel  
- strict angle and magnitude constraints  

SCT is intentionally small enough to train on a **single consumer GPU** (estimated 18 to 22 hours on RTX 3090), making it accessible for replication and extension.

---

## Why This Matters

SCT proposes a new primitive:

**semantic rotation**,  
a building block for next-generation reasoning systems.

The philosophical warrant comes from Hofstadter and Sander: if analogical computation is constitutive of cognition, encoding relational skeletons once and retrieving them without recapitulating their origins, then a model that externalizes this compressed relational structure as queryable geometry provides the same amortized benefit to artificial agents that analogical cognition provides to biological ones.

This is a **hypothesis under test**, not a claim. The v1.12 paper is a position paper; a companion empirical paper with training results is forthcoming.

This is not an attempt to emulate GPT-scale models.  
It's an attempt to carve out a new, interpretable, geometric axis in meaning space.

SCT sits **alongside**, not against, LLMs,  
the way Fourier transforms sit alongside differential equations.

---

## Repository Contents

```
/src      reference implementation  
/data     training samples and generation scripts  
/eval     geometric and semantic evaluation tools  
/models   checkpoints (coming soon)  
/paper    v1.12 PDF  
```

---

## Citation

If you use or extend this work, please cite:

```bibtex
@software{hacobian_2026_sct,
  author    = {Hacobian, Joe},
  title     = {Strange Compass Transformers: Geometric Navigation in Semantic Embedding Space},
  year      = {2026},
  version   = {v1.12},
  publisher = {Zenodo},
  doi       = {10.5281/zenodo.20758197},
  url       = {https://doi.org/10.5281/zenodo.20758197}
}
```

This repository provides the reference implementation, training scripts, design notes, and evaluation tools for the version described in the SCT v1.12 paper.
