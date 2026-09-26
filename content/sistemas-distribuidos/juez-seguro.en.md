---
title: "El Juez Seguro — C4 Architecture, Zero-Trust & Immutable Ledger"
description: "Design and implementation of a Zero-Trust judicial ecosystem with Rust threshold signatures, Go microservices, NATS asynchronous bus, and PostgreSQL cryptographic ledger."
date: 2026-09-25
weight: 10
tags: ["Go", "Rust", "Zero-Trust", "NATS", "PostgreSQL", "C4 Model", "Cryptography"]
---

<div class="action-links">
  <a class="action-btn btn-code" href="https://github.com/LynxCores/Juez-Seguro.git" target="_blank" rel="noopener">View Code (GitHub)</a>
  <a class="action-btn btn-doc" href="/docs/ECC_DSE_Certificate.pdf" target="_blank">DevSecOps Certification (PDF)</a>
</div>

## 1. Problem Space & Threat Model

In high-risk judicial jurisdictions, issuing rulings exposes magistrates to physical violence, extortion, and bribery. Legacy centralized judicial systems suffer from three critical vulnerabilities:
1. **Direct Issuer Traceability:** Standard nominal digital signatures (e.g., X.509) irrevocably bind the signer's identity to the ruling, visible to database administrators and network adversaries.
2. **Infrastructure Trust Dependency:** A rogue database admin or attacker with privileged server access can modify or suppress judicial records retroactively without leaving a cryptographic audit trail.
3. **Metadata Leakage:** Even with payload encryption, network traffic analysis (source IP, exact timestamps, packet bursts) permits statistical correlation of the magistrate's physical identity.

## 2. Fundamental Architectural Decisions

To solve these constraints without single points of compromise, **El Juez Seguro** is architected under strict **Zero-Trust** principles:

### Split Plane: Rust (Client) vs. Go (Backend Services)
* **Magistrate Client (Tauri v2 + Rust):** The magistrate's physical hardware is the sole trusted boundary. Cryptographic threshold signing (FROST-ed25519) runs natively in Rust with immediate memory zeroization of private key material. It integrates a local SQLite Outbox encrypted with SQLCipher and deterministic timing jitter to break temporal traffic correlation.
* **Go Distributed Microservices:** Go was selected for backend services due to lightweight concurrency (goroutines), minimal memory footprint per open TCP socket, and static single-binary compilation enabling minimal distroless container images.

### Asynchronous Messaging via NATS JetStream
Communication between the API gateway and backend worker microservices is decoupled via **NATS JetStream**:
* **Mitigating Head-of-Line Blocking:** Incoming HTTP requests receive an immediate `202 Accepted` after token parsing, offloading heavy cryptographic verification (`ms-validador`) and ledger persistence (`ms-persistencia`) to the event broker.
* **Burst & Failure Tolerance:** Even under database spikes or temporary latency, events persist in NATS streams with at-least-once semantics, preventing cascading failures.

---

## 3. C4 Architecture Model

### Level 1 — System Context Diagram (C4-L1)

```mermaid
flowchart TD
    M["Magistrate / Judge (Secure Hardware)"] -->|Signs and issues rulings via mTLS| S["El Juez Seguro (Zero-Trust Core)"]
    A["Auditor / Citizen (Public Portal)"] -->|Queries and verifies ledger validity| S
    S -->|Immutable external timestamping| OTS["OpenTimestamps / Trillian"]
```

### Level 2 — Container Diagram (C4-L2)

```mermaid
flowchart TB
    subgraph Dispositivo_Magistrado ["Magistrate Device (Trusted Boundary)"]
        Cliente["Magistrate Client (Tauri v2 + Rust)<br/>FROST-ed25519, SQLCipher, Metadata Scrubbing, Jitter"]
    end

    subgraph Backend_Cloud ["Cloud Backend (Untrusted Zero-Trust Boundary)"]
        Gateway["API Gateway (Go)<br/>Anti-replay, HMAC sessions, RBAC, Header Scrubbing"]
        NATS["NATS JetStream Broker<br/>Asynchronous event streaming"]
        Validador["MS Validator (Go)<br/>Threshold signature and quorum verification"]
        Persistencia["MS Persistence (Go)<br/>Immutable hash-chained ledger"]
        Auditoria["MS Audit (Go)<br/>Forensic trace consumption"]
        PG[("PostgreSQL Ledger (Append-Only)")]
    end

    Cliente -->|HTTPS / mTLS| Gateway
    Gateway -->|Async publish| NATS
    NATS -->|Subscribe| Validador
    NATS -->|Subscribe| Persistencia
    NATS -->|Subscribe| Auditoria
    Persistencia -->|Append-Only write| PG
    Auditoria -->|Audit trail| PG
```

### Level 3 — API Gateway Component Diagram (C4-L3)

```mermaid
flowchart LR
    Req["Inbound Request"] --> MW_Scrub["Metadata Scrubbing Middleware (Strips IP / User-Agent)"]
    MW_Scrub --> MW_Replay["Anti-Replay Control (Nonce & Timestamp Check)"]
    MW_Replay --> MW_Auth["Session Authenticator (HMAC & Role Validation)"]
    MW_Auth --> Router["Domain Router"]
    Router --> NATS_Pub["JetStream Event Publisher"]
    NATS_Pub --> NATS_Bus["NATS Broker"]
```

---

## 4. Security Auditing & DevSecOps Implementation

In alignment with **DevSecOps Essentials (DSE)** industry standards:
1. **Static Analysis (SAST):** Automated `gosec` and `cargo clippy` scanning in CI pipelines to prevent memory leaks and insecure randomness.
2. **Secret Leak Prevention:** Strict `.gitleaks.toml` rules blocking commits with high-entropy keys or test certificates.
3. **Network Isolation:** Hardened reverse proxy configuration with Caddy for TLS termination and DoS mitigation.
