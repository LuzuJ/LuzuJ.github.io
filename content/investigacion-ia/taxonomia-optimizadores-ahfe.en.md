---
title: "Deep Learning Optimizer Taxonomy (AHFE 2026)"
description: "Analytical framework of computational and memory efficiency in emerging optimizers (Lion, Sophia, Adan) for large-scale AI training."
date: 2026-09-25
weight: 20
tags: ["Deep Learning", "Optimizers", "Research", "AHFE 2026", "Python", "VRAM Efficiency"]
---

<div class="action-links">
  <a class="action-btn btn-code" href="https://github.com/LuzuJ/Experimentos-LLMs" target="_blank" rel="noopener">View Code (GitHub)</a>
  <a class="action-btn btn-code" href="https://openaccess.cms-conferences.org/publications/book/978-1-964867-79-3/article/978-1-964867-79-3_15" target="_blank" rel="noopener">Open Access Publication</a>
  <a class="action-btn btn-doc" href="/docs/AHFE2026_Certificate_Jonathan_Luzuriaga.pdf" target="_blank">Presentation Certificate (PDF)</a>
</div>

## 1. Research Context & Publication

* **Conference:** 17th International Conference on Applied Human Factors and Ergonomics (AHFE 2026), Fenerbahçe University, Istanbul, Turkey.
* **Paper Title:** *A Unified Taxonomy of Deep Learning Optimizers for Scalable and Efficient AI Systems*.
* **Publisher:** AHFE Open Access, Vol. 203.
* **DOI:** [10.54941/ahfe1008074](https://doi.org/10.54941/ahfe1008074)
* **Paper ID:** 1410

## 2. The Core Bottleneck: Memory Wall in Large Model Training

Distributed training of Large Language Models (LLMs) is fundamentally bounded by **GPU High-Bandwidth Memory (HBM/VRAM)** rather than raw tensor arithmetic capabilities.

The industry default optimizer, **AdamW**, tracks two 32-bit floating-point state tensors per parameter:
* First moment (momentum vector, $m_t$): 4 bytes/parameter.
* Second moment (uncentered variance, $v_t$): 4 bytes/parameter.

For a 7-billion parameter (7B) architecture, optimizer states alone consume **56 GB of pure VRAM**, necessitating complex sharding schemes (ZeRO Stage 3 / FSDP) that increase inter-node network communications.

## 3. Mechanistic Justification of Emerging Optimizers

Our research unified optimizers by mathematical order, memory footprint, and compatibility with *IO-aware* kernels (such as FlashAttention):

| Optimizer | Order | State Tensors | Memory Footprint vs AdamW | Stability Mechanism |
| :--- | :--- | :--- | :--- | :--- |
| **AdamW** | 1st Order | $m_t, v_t$ (2 tensors) | 1.0x (Baseline) | Coordinate-wise adaptive $L_2$ rescaling |
| **Lion** | 1st Order | $c_t$ (1 tensor) | **0.5x (-50% VRAM)** | Non-linear sign operation $\text{sign}(\beta_1 m_t + (1-\beta_1) g_t)$ |
| **Sophia** | 2nd Order | $m_t, \hat{h}_t$ (2 tensors) | 1.0x (Accelerated steps) | Diagonal stochastic Hessian with aggressive coordinate clipping |
| **Adan** | 1st Order | $m_t, v_t, n_t$ (3 tensors) | 1.5x | Decoupled Nesterov momentum and finite gradient differences |

### 3.1. Lion (EvoLved Sign Momentum)
Lion discards tracking of variance $v_t$ entirely:
$$c_t = \beta_1 m_{t-1} + (1 - \beta_1) g_t$$
$$\theta_t \leftarrow \theta_{t-1} - \eta_t (\text{sign}(c_t) + \lambda \theta_{t-1})$$
$$m_t = \beta_2 m_{t-1} + (1 - \beta_2) g_t$$

* **Operational Advantage:** The coordinate-wise `sign` operation imposes uniform step magnitudes across all dimensions, acting as an implicit regularizer. Slashing optimizer state memory by 50% allows larger batch sizes and higher GPU compute utilization.

### 3.2. Sophia (Second-order Clipped Stochastic Hessian)
Unlike standard second-order methods with intractable $\mathcal{O}(d^2)$ Hessian storage, Sophia estimates a stochastic diagonal $\hat{h}_t$ using Hutchinson's estimator every $k$ iterations:
$$\theta_t \leftarrow \theta_{t-1} - \eta_t \cdot \text{clip}\left( \frac{m_t}{\max(\hat{h}_t, \epsilon)}, \rho \right)$$

* **Operational Advantage:** Element-wise clipping ensures gradients navigating sharp non-convex ravines do not explode, achieving convergence with up to 50% fewer steps on language modeling tasks.

## 4. Symbolic Optimizer Search (Experimentos-LLMs)

To empirically validate mathematical variants, we built an evolutionary search pipeline in Python:
* **AST Computation Graph Formulation:** Generates symbolic expressions combining arithmetic operators, temporal momentum filters, and norm normalizations.
* **Isolated Benchmark Runner:** Evaluates candidates across convex and non-convex test surfaces to measure convergence rate, wall-clock time, and numerical stability.
