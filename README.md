# Bipedal Robot Papers: Model-Based & Optimal Control

A curated list of papers and code on **model-based control of bipedal (and legged) robots**, focused on **Model Predictive Control (MPC)** and **trajectory optimization**.

Papers are grouped by **how the robot is modeled**, which is the main design choice in these controllers. Within each group they go roughly from foundational to advanced.

> **Legend:** 📄 Paper · 🎥 Video · 💻 Code · 🦿 Biped/Humanoid · 🐕 Quadruped (the methods carry over to bipeds)

---

## 📑 Contents

1. [Suggested Reading Path](#-suggested-reading-path)
2. [Key Concepts (Glossary)](#-key-concepts-glossary)
3. [Robot Platforms & Hardware Design](#1-robot-platforms--hardware-design)
4. [Reduced-Order Model MPC (LIP / ALIP)](#2-reduced-order-model-mpc-lip--alip)
5. [Single-Rigid-Body & Centroidal MPC](#3-single-rigid-body--centroidal-mpc)
6. [Rough Terrain & Contact Estimation](#4-rough-terrain--contact-estimation)
7. [Jumping & Acrobatic Motions](#5-jumping--acrobatic-motions)
8. [Open-Source Code Summary](#-open-source-code-summary)

---

## 🧭 Suggested Reading Path

If you are new to the field, read in this order:

```
 ① Linear models (LIP)          →  ② Angular-momentum models (ALIP)  →  ③ Force-based / centroidal MPC
   "Robust linear MPC..."            "Terrain-Adaptive ALIP..."           "Force-and-moment-based MPC..."
   "Variable Horizon MPC..."                                              "Online Non-linear Centroidal MPC..."
                                                                                       │
                                                                                       ▼
                          ⑤ Full robot systems  ←  ④ Jumping & highly dynamic motion
                            "MIT Humanoid" papers      "Optimized Jumping on MIT Cheetah 3"
```

---

## 📘 Key Concepts (Glossary)

| Term | Meaning |
|------|---------|
| **MPC** (Model Predictive Control) | Repeatedly solve an optimization over a short future horizon, apply the first input, then re-plan. |
| **LIP / LIPM** (Linear Inverted Pendulum) | Treats the robot as a point mass at constant height on a massless leg. The dynamics become linear, so the MPC is fast and convex. |
| **ALIP** (Angular-momentum LIP) | A LIP variant that tracks angular momentum about the contact point instead of CoM velocity. It works better for underactuated robots like Cassie. |
| **SRB** (Single Rigid Body) | Treats the robot as one rigid body pushed by contact forces. This captures orientation and 3D motion. |
| **Centroidal dynamics** | The dynamics of the robot's total linear and angular momentum about its center of mass. |
| **Trajectory Optimization (TO)** | Offline or online optimization of a whole motion, e.g. a jump, often paired with MPC for tracking. |
| **Footstep adaptation** | Changing where and when the robot steps in real time to keep balance. |

---

## 1. Robot Platforms & Hardware Design

Complete robot systems, covering hardware, planning and control together.

| Paper | Year | Robot | Links |
|-------|:----:|-------|-------|
| The MIT Humanoid Robot: Design, Motion Planning, and Control For Acrobatic Behaviors | 2021 | 🦿 MIT Humanoid | [📄](https://arxiv.org/pdf/2104.09025.pdf) [🎥](https://www.youtube.com/watch?v=Z9oFlzL0T2s) |
| Design and Development of the MIT Humanoid: A Dynamic and Robust Research Platform | 2023 | 🦿 MIT Humanoid | [📄](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=10375199) |

---

## 2. Reduced-Order Model MPC (LIP / ALIP)

Simple, mostly linear pendulum models that keep the optimization fast. **Start here.**

| Paper | Year | Robot | Links |
|-------|:----:|-------|-------|
| A Robust Linear MPC Approach to Online Generation of 3D Biped Walking Motion | 2015 | 🦿 | [📄](https://ieeexplore.ieee.org/document/7363423) |
| Variable Horizon MPC with Swing Foot Dynamics for Bipedal Walking Control | 2020 | 🦿 Bolt | [📄](https://arxiv.org/pdf/2010.08198.pdf) [🎥](https://www.youtube.com/watch?v=xzCn1nQiVPI) [💻](https://github.com/machines-in-motion/reactive_planners) |
| Terrain-Adaptive, ALIP-Based Bipedal Locomotion Controller via Model Predictive Control and Virtual Constraints | 2021 | 🦿 Cassie | [📄](https://arxiv.org/pdf/2109.14862.pdf) [💻](https://github.com/UMich-BipedLab/cassie_alip_mpc) |
| Baseline Walking Controller (code only) | — | 🦿 Humanoids | [💻](https://github.com/isri-aist/BaselineWalkingController) |

---

## 3. Single-Rigid-Body & Centroidal MPC

Richer models that optimize **contact forces and moments**, which lets the robot move dynamically in 3D.

| Paper | Year | Robot | Links |
|-------|:----:|-------|-------|
| Force-and-moment-based Model Predictive Control for Achieving Highly Dynamic Locomotion on Bipedal Robots | 2021 | 🦿 | [📄](https://arxiv.org/pdf/2104.00065.pdf) |
| Online Non-linear Centroidal MPC for Humanoid Robot Locomotion with Step Adjustment | 2022 | 🦿 iCub | [📄](https://arxiv.org/pdf/2203.04489.pdf) [💻](https://github.com/ami-iit/paper_romualdi_2022_icra_centroidal-mpc-walking) |
| Orientation-Aware Model Predictive Control with Footstep Adaptation for Dynamic Humanoid Walking | 2022 | 🦿 | [📄](https://arxiv.org/pdf/2205.15443.pdf) |
| Model Predictive Control of Running Biped Robot | 2022 | 🦿 | [📄](https://www.mdpi.com/2076-3417/12/21/11183) |

---

## 4. Rough Terrain & Contact Estimation

Walking on uneven ground, stepping stones, and detecting when the feet touch down.

| Paper | Year | Robot | Links |
|-------|:----:|-------|-------|
| Contact Model Fusion for Event-Based Locomotion in Unstructured Terrains | 2018 | 🐕 MIT Cheetah 3 | [📄](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=8460904) |
| Dynamic Walking of Bipedal Robots on Uneven Stepping Stones via Adaptive-frequency MPC | 2022 | 🦿 | [📄](https://arxiv.org/pdf/2209.08664.pdf) |

> See also: *Terrain-Adaptive, ALIP-Based...* in [Section 2](#2-reduced-order-model-mpc-lip--alip).

---

## 5. Jumping & Acrobatic Motions

Highly dynamic motions, usually planned with **trajectory optimization** and tracked with MPC. Most of this work was done on quadrupeds first, but the same methods apply to bipeds.

| Paper | Year | Robot | Links |
|-------|:----:|-------|-------|
| Online Planning for Autonomous Running Jumps Over Obstacles in High-Speed Quadrupeds | 2015 | 🐕 MIT Cheetah | [📄](https://www.roboticsproceedings.org/rss11/p47.pdf) |
| Optimized Jumping on the MIT Cheetah 3 Robot | 2019 | 🐕 MIT Cheetah 3 | [📄](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=8794449) |
| Contact-timing and Trajectory Optimization for 3D Jumping on Quadruped Robots | 2022 | 🐕 Unitree A1 | [📄](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=9981284) |
| Continuous Jumping for Legged Robots on Stepping Stones via Trajectory Optimization and Model Predictive Control | 2022 | 🐕 Unitree A1 | [📄](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=9993259) |

> See also: *The MIT Humanoid Robot ... Acrobatic Behaviors* in [Section 1](#1-robot-platforms--hardware-design).

---

## 💻 Open-Source Code Summary

Papers you can run and modify directly:

| Project | Method | Code |
|---------|--------|------|
| Variable Horizon MPC (reactive_planners) | LIP-based MPC + swing foot | [GitHub](https://github.com/machines-in-motion/reactive_planners) |
| Cassie ALIP MPC | ALIP + MPC | [GitHub](https://github.com/UMich-BipedLab/cassie_alip_mpc) |
| Centroidal MPC Walking | Non-linear centroidal MPC | [GitHub](https://github.com/ami-iit/paper_romualdi_2022_icra_centroidal-mpc-walking) |
| Baseline Walking Controller | Humanoid walking controller | [GitHub](https://github.com/isri-aist/BaselineWalkingController) |

---

## 🤝 Contributing

To add a paper, put it in the section that matches its **model type**, keep each table in year order, and add links using the 📄 / 🎥 / 💻 icons.
