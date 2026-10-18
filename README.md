# Lifelong Reinforcement Learning for FANET Routing

**Research in progress** · B.Tech research project, IIITDM Kancheepuram · Author: Shirish Giroti

> This repository is a public **project overview**. The research code, datasets, experiment
> logs and results are kept private until the work is submitted for publication, and will be
> released alongside the paper.

## Problem

Flying Ad-hoc Networks (FANETs) are swarms of UAVs that relay packets over a wireless mesh
with no fixed infrastructure. Topology changes every few seconds, links degrade as traffic
builds, and nodes run on small batteries. Deep-RL routing agents are a natural fit, but a
policy that is deployed and left running over a long mission runs into three distinct problems:

| | Failure mode | What goes wrong in deployment |
|---|---|---|
| **F1** | **Cold start** | A randomly initialised agent routes badly at first. Under load, bad routing creates congestion, congestion degrades links, and the agent may never recover. |
| **F2** | **Catastrophic forgetting** | When mobility or traffic conditions shift, fine-tuning overwrites what the agent already knew. |
| **F3** | **No episodic recall** | The agent re-derives from scratch situations it has already solved, such as a recurring interference pattern in one area. |

The project studies a routing agent designed to address all three: **supervised warm-starting
from expert teachers**, **continual learning** across mobility regimes, and **case-based
reasoning** over previously encountered situations.

## What has been built

**Physically grounded 3D FANET simulator**
- Two-level time model: slow topology/mobility frames containing fine-grained channel slots
- 802.11 DCF contention modelled with the Bianchi collision model; SINR-based link quality
  with log-normal shadowing and hidden-terminal interference
- Automatic repeat request (ARQ), bounded per-node queues, energy-aware forwarding, and a
  full packet-drop taxonomy
- A load → interference → link quality → routing feedback loop, so that congestion collapse
  is actually reproducible rather than assumed away

**Classical routing teacher panel**
- Eight classical routing algorithms spanning shortest-path, geographic, and
  backpressure families, benchmarked across network regimes
- Used to derive regime-specific expert teachers for imitation-based warm-starting

**Learning pipeline**
- Large-scale expert-demonstration dataset generation with decision-level features
- Graph-attention policy network, evaluated against a parameter-matched MLP control

## Engineering and methodology

Most of the effort in this project has gone into making the experiments trustworthy:

- **Pre-registered predictions and thresholds.** Thresholds are never revised after seeing
  results; contradictions are reported as-is.
- **30+ seeds with Holm–Bonferroni correction** for every headline comparison, with effect
  size and confidence interval reported before the p-value.
- **Negative controls.** Every equivalence check is run against a deliberately broken variant
  to prove it can fail before its passing result is trusted.
- **Assertion-guarded patching.** Every code change is a dry-run-first, idempotent patch that
  writes nothing unless every anchor matches exactly once.
- **Audit gates and a defect ledger.** Dedicated verification stages found and logged
  50+ defects, including several in the checking code itself, before they could affect results.
- **Negative results are published** alongside positive ones.

## Tech stack

Python · PyTorch · Graph neural networks (attention) · DQN · NetworkX · NumPy · CUDA ·
Gymnasium · YAML-driven experiment configuration

## Status

| Stage | Status |
|---|---|
| Interference-aware link model | Complete |
| Multi-packet simulator | Complete |
| Teacher panel and oracle benchmarking | Complete |
| Dataset generation and independent audit | Complete |
| Graph-attention policy, supervised pre-training | Complete |
| RL environment and training | Upcoming |
| Cold-start, continual-learning and CBR studies | Planned |

**Target venues:** IEEE TNSM, IEEE TMC, IoT Journal, Ad Hoc Networks (Elsevier)

## Contact

Shirish Giroti · shirishgiroti@gmail.com ·
[LinkedIn](https://linkedin.com/in/shirish-giroti-271137359) ·
[GitHub](https://github.com/dovahkkin1234)
