---
title: "Taxonomía de Optimizadores de Deep Learning (AHFE 2026)"
description: "Marco analítico de eficiencia computacional y memoria en optimizadores emergentes (Lion, Sophia, Adan) para el escalado de modelos masivos."
date: 2026-09-25
weight: 20
tags: ["Deep Learning", "Optimizers", "Research", "AHFE 2026", "Python", "VRAM Efficiency"]
---

<div class="action-links">
  <a class="action-btn btn-code" href="https://github.com/LuzuJ/Experimentos-LLMs" target="_blank" rel="noopener">Ver Código (GitHub)</a>
  <a class="action-btn btn-code" href="https://openaccess.cms-conferences.org/publications/book/978-1-964867-79-3/article/978-1-964867-79-3_15" target="_blank" rel="noopener">Publicación (Open Access)</a>
  <a class="action-btn btn-doc" href="/docs/AHFE2026_Certificate_Jonathan_Luzuriaga.pdf" target="_blank">Certificado de Presentación (PDF)</a>
</div>

## 1. Contexto de Investigación y Publicación

* **Conferencia:** 17th International Conference on Applied Human Factors and Ergonomics (AHFE 2026), Fenerbahçe University, Estambul, Turquía.
* **Artículo:** *A Unified Taxonomy of Deep Learning Optimizers for Scalable and Efficient AI Systems*.
* **Publicación Oficial:** AHFE Open Access, Vol. 203.
* **DOI:** [10.54941/ahfe1008074](https://doi.org/10.54941/ahfe1008074)
* **Paper ID:** 1410

## 2. El Cuello de Botella: El Muro de Memoria en el Entrenamiento de IA

El entrenamiento distribuido de modelos de lenguaje (LLMs) y modelos de visión a gran escala está acotado fundamentalmente por el **ancho de banda y la capacidad de la memoria de la GPU (VRAM)** más que por la capacidad pura de FLOPS de cómputo.

El optimizador hegemónico en la industria, **AdamW**, requiere mantener dos estados en memoria por cada parámetro del modelo en precisión simple (FP32):
* Primer momento (media móvil del gradiente, $m_t$): 4 bytes por parámetro.
* Segundo momento (media móvil del gradiente al cuadrado, $v_t$): 4 bytes por parámetro.

Para un modelo de 7 mil millones de parámetros (7B), los estados del optimizador demandan **56 GB de memoria pura**, obligando a recurrir a técnicas de particionado complejas (ZeRO Stage 3, FSDP) que incrementan sustancialmente el tráfico de comunicación entre nodos.

## 3. Justificación Mecanística de Optimizadores Emergentes

Nuestra investigación estructuró una taxonomía unificada clasificando algoritmos por su orden matemático, consumo de memoria y adaptabilidad a arquitecturas *IO-aware* (como FlashAttention):

| Optimizador | Orden | Estados de Memoria | Coste VRAM vs AdamW | Mecanismo de Estabilidad |
| :--- | :--- | :--- | :--- | :--- |
| **AdamW** | 1er Orden | $m_t, v_t$ (2 tensores) | 1.0x (Línea base) | Normalización $L_2$ adaptativa por coordenada |
| **Lion** | 1er Orden | $c_t$ (1 tensor) | **0.5x (-50% VRAM)** | Operación signo no lineal $\text{sign}(\beta_1 m_t + (1-\beta_1) g_t)$ |
| **Sophia** | 2do Orden | $m_t, \hat{h}_t$ (2 tensores) | 1.0x (convergencia acelerada) | Precondicionador diagonal de Hessiano con clipping agresivo |
| **Adan** | 1er Orden | $m_t, v_t, n_t$ (3 tensores) | 1.5x | Aceleración Nesterov + diferencias finitas de gradiente |

### 3.1. Lion (EvoLved Sign Momentum)
Lion elimina completamente el tracking del segundo momento $v_t$. La regla de actualización se formula como:
$$c_t = \beta_1 m_{t-1} + (1 - \beta_1) g_t$$
$$\theta_t \leftarrow \theta_{t-1} - \eta_t (\text{sign}(c_t) + \lambda \theta_{t-1})$$
$$m_t = \beta_2 m_{t-1} + (1 - \beta_2) g_t$$

* **Ventaja Operativa:** Al usar la función $\text{sign}(\cdot)$, cada coordenada se actualiza con una magnitud uniforme, actuando como una regularización implícita. Esto reduce el consumo de memoria a la mitad y permite incrementar el tamaño de lote efectivo (*batch size*).

### 3.2. Sophia (Second-order Clipped Stochastic Hessian)
A diferencia de los métodos tradicionales de segundo orden cuyo cálculo del Hessiano completo tiene complejidad $\mathcal{O}(d^2)$, Sophia calcula una aproximación diagonal estocástica $\hat{h}_t$ usando el estimador de Hutchinson cada $k$ pasos:
$$\theta_t \leftarrow \theta_{t-1} - \eta_t \cdot \text{clip}\left( \frac{m_t}{\max(\hat{h}_t, \epsilon)}, \rho \right)$$

* **Ventaja Operativa:** El operador de recorte $\text{clip}(\cdot, \rho)$ garantiza que gradientes erráticos en regiones de curvatura negativa o valles estrechos no desestabilicen el entrenamiento, logrando convergencia con hasta un 50% menos de pasos totales en modelos de lenguaje.

## 4. Descubrimiento Algorítmico Simbólico (Experimentos-LLMs)

Como soporte empírico de la investigación, implementamos un entorno de búsqueda evolutiva en Python (`Experimentos-LLMs`):
* **Generador de Grafos de Computación:** Un árbol sintáctico abstracto (AST) formula algebraicamente nuevas combinaciones de operaciones aritméticas elementales sobre gradientes históricos y estados de momentum.
* **Evaluador Aislado:** Cada candidato es evaluado automáticamente sobre problemas de juguete convexos y no convexos para medir tasa de convergencia, divergencia numérica y coste computacional.
