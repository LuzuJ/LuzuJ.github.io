---
title: "Jonathan Luzuriaga | Systems & Applied AI"
date: 2026-09-25
draft: false
---

# Jonathan Luzuriaga
### Software Engineer (Systems & Applied AI)

> **Engineering Principle:** Distributed architectures, fault tolerance, and algorithmic optimization under Zero-Trust principles.

<div class="action-links">
  <a class="action-btn btn-code" href="https://github.com/LuzuJ" target="_blank" rel="noopener">GitHub: @LuzuJ</a>
  <a class="action-btn btn-code" href="https://www.linkedin.com/in/jonathan-luzuriaga/" target="_blank" rel="noopener">LinkedIn: jonathan-luzuriaga</a>
  <a class="action-btn btn-code" href="mailto:jonaluzu1@gmail.com">Email: jonaluzu1@gmail.com</a>
  <a class="action-btn btn-doc" href="/docs/Jonathan_J_Luzuriaga_CV_EN.pdf" target="_blank">Download Resume (English) [PDF]</a>
</div>

---

## 1. [Distributed Systems Architecture](/en/sistemas-distribuidos/) (Core Backend)

Critical backend infrastructure designed under hostile network assumptions, immutable persistence, and asynchronous decoupling to eliminate lock contention and single points of failure.

### El Juez Seguro — Zero-Trust Judicial Ecosystem & Immutable Ledger

Platform for issuing cryptographically verifiable, anonymous judicial rulings. The system implements threshold and group signatures to shield the physical identity of magistrates while enforcing tamper-proof integrity and non-repudiation.

<div class="action-links">
  <a class="action-btn btn-code" href="https://github.com/LynxCores/Juez-Seguro.git" target="_blank" rel="noopener">View Code (GitHub)</a>
  <a class="action-btn btn-arch" href="/en/sistemas-distribuidos/juez-seguro/">Read Architecture</a>
</div>

* **Multi-Language Topology:** Strict separation between the client desktop app in **Rust** (Tauri v2)—executing local cryptographic signing in isolated memory (FROST-ed25519)—and the distributed backend control plane in **Go**.
* **Asynchronous Bottleneck Mitigation:** Event ingestion and distribution via **NATS JetStream**, decoupling inbound HTTP ingestion from cryptographic validation (`ms-validador`) and ledger persistence (`ms-persistencia`).
* **Immutable Persistence:** Append-only ledger in **PostgreSQL** linked by SHA-256 cryptographic hash chains with B-Tree indexes optimized for forensic auditing queries.
* **Zero-Trust Boundary:** Identity token validation, network metadata scrubbing (stripping IP and User-Agent headers at the API Gateway), and mutual TLS (mTLS) for inter-service communication.

---

## 2. [Empirical Modeling & Cloud Deployments](/en/investigacion-ia/) (Research & AI)

Formal research and machine learning engineering bounded by hardware constraints, memory bandwidth limits, and acoustic signal processing.

### Deep Learning Optimizer Research (AHFE 2026)

Peer-reviewed paper presented at the *17th International Conference on Applied Human Factors and Ergonomics (AHFE 2026)*, Fenerbahçe University (Istanbul, Turkey):
**"A Unified Taxonomy of Deep Learning Optimizers for Scalable and Efficient AI Systems"** *(DOI: [10.54941/ahfe1008074](https://doi.org/10.54941/ahfe1008074))*.

<div class="action-links">
  <a class="action-btn btn-code" href="https://github.com/LuzuJ/Experimentos-LLMs" target="_blank" rel="noopener">View Code (GitHub)</a>
  <a class="action-btn btn-code" href="https://openaccess.cms-conferences.org/publications/book/978-1-964867-79-3/article/978-1-964867-79-3_15" target="_blank" rel="noopener">Read Publication (Open Access)</a>
  <a class="action-btn btn-arch" href="/en/investigacion-ia/taxonomia-optimizadores-ahfe/">Read Architecture</a>
</div>

* **Computational Cost Reduction:** A systematic framework classifying optimizers (Lion, Sophia, Adan) against the standard AdamW baseline.
  * **Lion:** Replaces second-order moments with the `sign(momentum)` operator, slashing optimizer VRAM footprint by 50% and boosting training throughput on Transformer models.
  * **Sophia:** Evaluates stochastic diagonal Hessian estimates with aggressive coordinate clipping, preventing catastrophic divergences on non-convex loss ravines and accelerating convergence.
  * **Adan:** Decouples gradient estimation and finite difference terms to accelerate convergence with fewer total FLOPS.
* **Symbolic Evolutionary Discovery:** Experimental Python pipeline for automated symbolic search and validation of novel optimizer candidates.

### Continuous Multi-Label Bioacoustics Pipeline (BirdCLEF 2026)

End-to-end audio processing pipeline for continuous wildlife acoustic event detection under extreme environmental noise (Kaggle / Cornell Lab of Ornithology).

<div class="action-links">
  <a class="action-btn btn-code" href="https://github.com/LuzuJ" target="_blank" rel="noopener">View Repositories (GitHub)</a>
  <a class="action-btn btn-arch" href="/en/investigacion-ia/birdclef-acustica/">Read Architecture</a>
</div>

* **Signal Ingestion:** Deterministic 5.0s window slicing with GPU-accelerated Mel-spectrogram extraction.
* **Acoustic Noise Isolation:** Suppression of rain, wind, and background noise via bandpass filtering (0.5 – 14 kHz) and spectral data augmentation (*SpecAugment* & *Mixup*).
* **Model Distillation:** Convolutional ensemble distilled into compact neural networks for continuous edge/CPU inference.

### Lynx Cores (Vixora Tech) — Edge-to-Cloud Phytosanitary Architecture

Offline-first distributed system for early detection of cacao crop diseases (*Monilia*, *Phytophthora*, and healthy tissue). Awarded 2nd place at Hult Prize EPN with national qualification.

<div class="action-links">
  <a class="action-btn btn-code" href="https://github.com/LynxCores" target="_blank" rel="noopener">View Organization (GitHub)</a>
  <a class="action-btn btn-arch" href="/en/investigacion-ia/lynx-cores-cloud-edge/">Read Architecture</a>
</div>

* **Edge Inference (Offline-First):** Quantized INT8 YOLO model executed locally on mobile hardware, ensuring inference latencies under 200 ms without cellular connectivity.
* **Asynchronous Cloud Sync:** Idempotent batch dispatcher synchronizing field telemetry and GPS-tagged diagnoses with backend cloud clusters upon network reconnection.

### Out-of-Core Modeling for Intelligent Irrigation

Predictive soil water requirement system processing national-scale satellite telemetry.

<div class="action-links">
  <a class="action-btn btn-code" href="https://github.com/LuzuJ/Riego_Inteligente" target="_blank" rel="noopener">View Code (GitHub)</a>
  <a class="action-btn btn-arch" href="/en/investigacion-ia/riego-inteligente/">Read Architecture</a>
</div>

* **Out-of-Core Processing:** Lazy evaluation DAGs using **Dask** to process massive spatiotemporal datasets beyond physical RAM limits.
* **Hybrid Modeling:** Weighted ensemble of gradient boosting (**XGBoost**) and recurrent neural networks (**LSTM**) capturing hydrological lag.

---

## 3. [Operational Orchestration & CI/CD](/en/operaciones-cicd/)

Automating critical data workflows and internal infrastructure to eliminate operational latency and ensure state consistency across teams.

### Manticore Labs — Backend Automation & Idempotent Sync

Cross-departmental workflow orchestration integrating asynchronous event streams and central knowledge bases.

<div class="action-links">
  <a class="action-btn btn-code" href="https://github.com/LuzuJ" target="_blank" rel="noopener">View GitHub Profile</a>
  <a class="action-btn btn-arch" href="/en/operaciones-cicd/manticore-orquestacion-n8n/">Read Architecture</a>
</div>

* **n8n Orchestration:** Reactive event-driven pipelines triggered via webhooks for ingesting operational tasks.
* **Notion API Integration:** Token-bucket rate limiting (under 3 req/s), exponential backoff with jitter on HTTP 429, and SHA-256 payload deduplication preventing update loops.

---

## 4. [Audited Credentials](/en/credenciales/)

Third-party certifications and peer-reviewed academic credentials.

| Credential / Certification | Issuing Body | ID / DOI | Validity / Date | Verification |
| :--- | :--- | :--- | :--- | :--- |
| **DevSecOps Essentials (DSE)** | EC-Council | `ECC6947035182` | Jul 2026 – Jul 2029 | [View Certificate PDF](/docs/ECC_DSE_Certificate.pdf) |
| **Speaker & Author AHFE 2026** | AHFE International | Paper ID: `1410` | July 2026 | [View Certificate PDF](/docs/AHFE2026_Certificate_Jonathan_Luzuriaga.pdf) |
| **Peer-Reviewed Paper AHFE** | AHFE Open Access | `10.54941/ahfe1008074` | July 2026 | [DOI Access](https://doi.org/10.54941/ahfe1008074) |

<div class="action-links">
  <a class="action-btn btn-arch" href="/en/credenciales/dse-eccouncil/">Audit Credentials</a>
</div>
