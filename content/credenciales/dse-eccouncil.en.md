---
title: "DevSecOps Essentials (DSE) — EC-Council"
description: "Official accreditation in Shift-Left security principles, static and dynamic code auditing, and secure software lifecycle engineering."
date: 2026-09-25
weight: 10
tags: ["DevSecOps", "Security", "EC-Council", "Certification", "Zero-Trust"]
---

<div class="action-links">
  <a class="action-btn btn-doc" href="/docs/ECC_DSE_Certificate.pdf" target="_blank">Download Official Certificate (PDF)</a>
</div>

## 1. Certification Records

* **Credential Name:** DevSecOps Essentials (DSE)
* **Issuing Body:** EC-Council (International Council of E-Commerce Consultants)
* **Credential ID:** `ECC6947035182`
* **Validity Period:** July 2026 – July 2029
* **Competency Focus:** Seamless integration of security testing within continuous deployment lifecycles (*Shift-Left Security*), replacing late-stage perimeter audits with automated security-as-code gates.

---

## 2. Audited Competencies & Architectural Application

The certification validates operational competencies implemented across projects like **El Juez Seguro**:

| DevSecOps Domain | Implemented Control | Standard / Tooling |
| :--- | :--- | :--- |
| **Static Analysis (SAST)** | Automated AST code scanning detecting integer overflows, insecure concurrency, and cryptographic flaws in Go and Rust. | `gosec`, `cargo clippy`, Azure DevOps CI |
| **Secret Leak Prevention** | Pre-commit entropy validation preventing private key material leakage. | `gitleaks` |
| **Software Composition (SCA)** | Automated scanning for known Common Vulnerabilities and Exposures (CVEs) in third-party libraries. | `cargo audit`, `govulncheck` |
| **Hardened Infrastructure** | Rootless container deployments with minimal distroless base images. | Docker Distroless, Caddy TLS |
| **Threat Modeling** | Adversarial surface analysis under hostile network assumptions. | Zero-Trust principles, C4 Model |

---

## 3. Peer-Reviewed Academic Publication (AHFE 2026)

* **Organization:** 17th International Conference on Applied Human Factors and Ergonomics (AHFE 2026), Fenerbahçe University, Istanbul, Turkey.
* **Paper Title:** *A Unified Taxonomy of Deep Learning Optimizers for Scalable and Efficient AI Systems*
* **Paper ID:** `1410`
* **DOI:** [10.54941/ahfe1008074](https://doi.org/10.54941/ahfe1008074)
* **Document:** [Presentation and Publication Certificate (PDF)](/docs/AHFE2026_Certificate_Jonathan_Luzuriaga.pdf)
