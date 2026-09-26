---
title: "Certificación DevSecOps Essentials (DSE) — EC-Council"
description: "Acreditación oficial en principios de seguridad Shift-Left, auditoría de código estático y dinámico y protección del ciclo de vida del software."
date: 2026-09-25
weight: 10
tags: ["DevSecOps", "Security", "EC-Council", "Certification", "Zero-Trust"]
---

<div class="action-links">
  <a class="action-btn btn-doc" href="/docs/ECC_DSE_Certificate.pdf" target="_blank">Descargar Certificado Oficial (PDF)</a>
</div>

## 1. Datos de la Certificación

* **Nombre Oficial:** DevSecOps Essentials (DSE)
* **Entidad Emisora:** EC-Council (International Council of E-Commerce Consultants)
* **Número de Identificación de Credencial:** `ECC6947035182`
* **Periodo de Vigencia:** Julio 2026 – Julio 2029
* **Foco del Estándar:** Integración sistemática de controles de seguridad en pipelines continuos de integración y despliegue (CI/CD), transitando de la seguridad como etapa perimetral tardía a la seguridad como código en etapas tempranas (*Shift-Left Security*).

---

## 2. Competencias Auditadas y Aplicación en Arquitectura

La certificación valida habilidades prácticas y teóricas en los siguientes dominios, reflejados en proyectos como **El Juez Seguro**:

| Dominio DevSecOps | Control Implementado | Herramienta / Estándar |
| :--- | :--- | :--- |
| **Análisis Estático (SAST)** | Inspección automática de código fuente para detectar desbordamientos, concurrencia insegura y mal uso de criptografía en Go y Rust. | `gosec`, `cargo clippy`, Azure DevOps CI |
| **Detección de Secretos** | Bloqueo preventivo de commits con entropía sospechosa o patrones de llaves privadas. | `gitleaks` pre-commit hooks |
| **Análisis de Dependencias (SCA)** | Detección de vulnerabilidades conocidas (CVEs) en submódulos y dependencias de terceros. | `cargo audit`, `govulncheck` |
| **Aislamiento de Infraestructura** | Contenerización sin privilegios (*rootless*) e imágenes mínimas. | Docker Distroless, Caddy TLS |
| **Modelado de Amenazas** | Análisis de superficies de ataque bajo supuestos de red hostil. | Principios Zero-Trust, Modelado C4 |

---

## 3. Publicación Científica Revisada por Pares (AHFE 2026)

* **Organismo:** 17th International Conference on Applied Human Factors and Ergonomics (AHFE 2026), Fenerbahçe University, Estambul, Turquía.
* **Paper Title:** *A Unified Taxonomy of Deep Learning Optimizers for Scalable and Efficient AI Systems*
* **Paper ID:** `1410`
* **DOI:** [10.54941/ahfe1008074](https://doi.org/10.54941/ahfe1008074)
* **Documento:** [Certificado de Ponencia y Publicación (PDF)](/docs/AHFE2026_Certificate_Jonathan_Luzuriaga.pdf)
