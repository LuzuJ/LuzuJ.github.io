---
title: "Jonathan Luzuriaga | Systems & Applied AI"
date: 2026-09-25
draft: false
---

# Jonathan Luzuriaga
### Software Engineer (Systems & Applied AI)

> **Postulado de Ingeniería:** Arquitecturas distribuidas, tolerancia a fallos y optimización algorítmica bajo principios Zero-Trust.

<div class="action-links">
  <a class="action-btn btn-code" href="https://github.com/LuzuJ" target="_blank" rel="noopener">GitHub: @LuzuJ</a>
  <a class="action-btn btn-code" href="https://www.linkedin.com/in/jonathan-luzuriaga/" target="_blank" rel="noopener">LinkedIn: jonathan-luzuriaga</a>
  <a class="action-btn btn-code" href="mailto:jonaluzu1@gmail.com">Email: jonaluzu1@gmail.com</a>
  <a class="action-btn btn-doc" href="/docs/CV_Jonathan_J_Luzuriaga_ES.pdf" target="_blank">Descargar CV (Español) [PDF]</a>
</div>

---

## 1. [Arquitectura de Sistemas Distribuidos](/sistemas-distribuidos/) (Core Backend)

Sistemas diseñados bajo supuestos de red hostil, persistencia inmutable y desacoplamiento asíncrono para eliminar contención y puntos únicos de fallo.

### El Juez Seguro — Ecosistema Judicial Zero-Trust y Ledger Inmutable

Plataforma de emisión de fallos judiciales anónimos pero criptográficamente auditables. El sistema implementa firmas de umbral y de grupo para proteger la identidad física del magistrado mientras garantiza integridad y no repudio absoluto.

<div class="action-links">
  <a class="action-btn btn-code" href="https://github.com/LynxCores/Juez-Seguro.git" target="_blank" rel="noopener">Ver Código (GitHub)</a>
  <a class="action-btn btn-arch" href="/sistemas-distribuidos/juez-seguro/">Leer Arquitectura</a>
</div>

* **Topología Multi-Lenguaje:** Separación estricta entre el cliente de escritorio en **Rust** (Tauri v2) que ejecuta firma criptográfica local en memoria aislada (FROST-ed25519) y el plano de control backend distribuido en **Go**.
* **Mitigación de Cuellos de Botella Asíncronos:** Ingesta y distribución de eventos mediante **NATS JetStream**, desacoplando la recepción HTTP de los microservicios de validación criptográfica (`ms-validador`) y registro (`ms-persistencia`).
* **Persistencia Inmutable:** Almacenamiento en **PostgreSQL** mediante esquema append-only con encadenamiento criptográfico por hash SHA-256 (ledger inmutable) e índices B-Tree optimizados para consultas de auditoría forense.
* **Modelo Zero-Trust & Aislamiento:** Verificación de identidad con tokens firmados, saneamiento de metadatos de red (scrubbing de IP/User-Agent en el API Gateway) y mTLS en enlaces inter-servicios.

---

## 2. [Modelado Empírico y Despliegues Cloud](/investigacion-ia/) (Investigación & IA)

Investigación formal y desarrollo de sistemas de aprendizaje automático guiados por restricciones físicas de cómputo, eficiencia de memoria y análisis de señales.

### Investigación en Optimizadores de Deep Learning (AHFE 2026)

Publicación científica en *Applied Human Factors and Ergonomics (AHFE) 2026*, Fenerbahçe University (Estambul, Turquía). Coautor del paper:
**"A Unified Taxonomy of Deep Learning Optimizers for Scalable and Efficient AI Systems"** *(DOI: [10.54941/ahfe1008074](https://doi.org/10.54941/ahfe1008074))*.

<div class="action-links">
  <a class="action-btn btn-code" href="https://github.com/LuzuJ/Experimentos-LLMs" target="_blank" rel="noopener">Ver Código (GitHub)</a>
  <a class="action-btn btn-code" href="https://openaccess.cms-conferences.org/publications/book/978-1-964867-79-3/article/978-1-964867-79-3_15" target="_blank" rel="noopener">Ver Publicación (Open Access)</a>
  <a class="action-btn btn-arch" href="/investigacion-ia/taxonomia-optimizadores-ahfe/">Leer Arquitectura</a>
</div>

* **Reducción de Costes Computacionales:** Marco analítico que clasifica optimizadores (Lion, Sophia, Adan) frente al estándar AdamW.
  * **Lion:** Reemplazo de momentos de segundo orden por la operación `sign(momentum)`, reduciendo la huella de memoria del optimizador al 50% y acelerando el throughput en arquitecturas tipo Transformer.
  * **Sophia:** Estimación diagonal del Hessiano mediante perturbaciones estocásticas con clipping agresivo, previniendo oscilaciones catastróficas en superficies de pérdida no convexas y reduciendo pasos de convergencia.
  * **Adan:** Estimación simultánea de gradientes y diferencias de gradiente para acelerar convergencia con menor conteo de FLOPS totales.
* **Búsqueda Simbólica Evolutiva:** Pipeline experimental implementado en Python para el descubrimiento y evaluación automatizada de variantes de optimizadores.

### Pipeline Multietiqueta de Bioacústica (BirdCLEF 2026)

Arquitectura de procesamiento de audio continuo para la detección de eventos sonoros de fauna en ecosistemas de alta interferencia ambiental (Kaggle / Cornell Lab of Ornithology).

<div class="action-links">
  <a class="action-btn btn-code" href="https://github.com/LuzuJ" target="_blank" rel="noopener">Ver Pipeline (GitHub)</a>
  <a class="action-btn btn-arch" href="/investigacion-ia/birdclef-acustica/">Leer Arquitectura</a>
</div>

* **Ingesta de Señal:** Segmentación temporal determinista en ventanas de 5 segundos con extracción de espectrogramas Mel calculados en GPU.
* **Aislamiento Acústico:** Supresión de ruido de fondo (viento, lluvia) mediante filtros pasa-banda y técnicas de aumento espectral (*SpecAugment*).
* **Inferencia y Destilación:** Ensamble de backbones convolucionales ligeros y visión por computadora con destilación de conocimiento para viabilizar inferencia concurrente en recursos limitados.

### Lynx Cores (Vixora Tech) — Detección Fitosanitaria Edge-to-Cloud

Arquitectura distribuida para el diagnóstico temprano de fitopatologías en plantaciones de cacao (*Monilia*, *Fitoftora*, tejido sano). Galardonado con el 2do lugar en Hult Prize EPN y clasificación nacional.

<div class="action-links">
  <a class="action-btn btn-code" href="https://github.com/LynxCores" target="_blank" rel="noopener">Ver Organización (GitHub)</a>
  <a class="action-btn btn-arch" href="/investigacion-ia/lynx-cores-cloud-edge/">Leer Arquitectura</a>
</div>

* **Inferencia Local (Offline-First):** Modelo YOLO cuantizado (INT8) ejecutado directamente en el dispositivo móvil del agricultor, garantizando diagnósticos en menos de 200 ms sin conectividad celular.
* **Orquestación Cloud Asíncrona:** Cuando el dispositivo restablece conexión, un despachador sincroniza lotes de inferencias con metadatos geoespaciales hacia un clúster backend para trazabilidad epidemiológica regional.

### Modelado Out-of-Core para Riego Inteligente

Sistema de predicción de estrés hídrico agrícola a partir de telemetría y datos satelitales climáticos de gran escala.

<div class="action-links">
  <a class="action-btn btn-code" href="https://github.com/LuzuJ/Riego_Inteligente" target="_blank" rel="noopener">Ver Código (GitHub)</a>
  <a class="action-btn btn-arch" href="/investigacion-ia/riego-inteligente/">Leer Arquitectura</a>
</div>

* **Cómputo Fuera de Memoria:** Procesamiento con **Dask** para superar limitaciones de RAM sobre series temporales geoespaciales.
* **Modelado Híbrido:** Combinación de gradiente boosting (**XGBoost**) y redes neuronales recurrentes (**LSTM**) para capturar dependencias temporales y dinámicas climáticas.

---

## 3. [Orquestación Operativa y CI/CD](/operaciones-cicd/)

Automatización de flujos críticos de datos e infraestructura para mitigar fricción humana y garantizar consistencia de estados entre sistemas.

### Manticore Labs — Automatización Backend y Sincronización Idempotente

Plataforma de automatización de operaciones inter-departamentales integrando flujos de datos asíncronos y bases de conocimiento corporativas.

<div class="action-links">
  <a class="action-btn btn-code" href="https://github.com/LuzuJ" target="_blank" rel="noopener">Ver Código / Flujos</a>
  <a class="action-btn btn-arch" href="/operaciones-cicd/manticore-orquestacion-n8n/">Leer Arquitectura</a>
</div>

* **Orquestación con n8n:** Construcción de pipelines de eventos reactivos activados por webhooks para la ingestión y transformación de estados operativos.
* **Sincronización con Notion API:** Conectores bidireccionales con manejo de rate-limiting (429), reintentos exponenciales con jitter y control de idempotencia para sincronizar documentación técnica sin bloqueos ni inconsistencias de versión.

---

## 4. [Credenciales Auditadas](/credenciales/)

Validaciones rigurosas emitidas por entidades certificadoras y comités científicos internacionales.

| Credencial / Certificación | Organismo Emisor | Identificador / DOI | Vigencia / Fecha | Verificación |
| :--- | :--- | :--- | :--- | :--- |
| **DevSecOps Essentials (DSE)** | EC-Council | `ECC6947035182` | Jul 2026 – Jul 2029 | [Ver Certificado PDF](/docs/ECC_DSE_Certificate.pdf) |
| **Ponente & Coautor AHFE 2026** | AHFE International | Paper ID: `1410` | Julio 2026 | [Ver Certificado PDF](/docs/AHFE2026_Certificate_Jonathan_Luzuriaga.pdf) |
| **Publicación Indexada AHFE** | AHFE Open Access | `10.54941/ahfe1008074` | Julio 2026 | [Acceso DOI](https://doi.org/10.54941/ahfe1008074) |

<div class="action-links">
  <a class="action-btn btn-arch" href="/credenciales/dse-eccouncil/">Auditoría de Credenciales</a>
</div>
