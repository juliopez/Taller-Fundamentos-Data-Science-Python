---
layout: default
title: Machine Learning 2026
---

# Machine Learning 2026

Material docente de la asignatura **Machine Learning 2026**, organizada en 18 semanas y articulada mediante un **Proyecto Integrador de clasificación de imágenes utilizando Redes Neuronales Convolucionales (CNN)**.

[← Volver al portal general](index.html)

---

## Ruta de aprendizaje

**Modelos de clasificación → Redes neuronales densas (MLP) → Preparación de imágenes → Redes convolucionales (CNN) → Evaluación y experimentación → Aplicación y despliegue**

Los notebooks y simuladores se complementan: primero ayudan a comprender los conceptos y después permiten implementarlos, evaluar resultados y fundamentar decisiones.

## Simuladores interactivos

### 1. Explorador de redes neuronales

[**Abrir simulador de redes neuronales →**](Simulador_Redes_Neuronales.html)

Explora capas densas, neuronas, funciones de activación e hiperparámetros como épocas, batch size y learning rate.

### 2. Introducción a redes convolucionales (CNN)

[**Abrir simulador introductorio CNN →**](Simulador_CNN.html)

Visualiza las operaciones `Conv2D`, `ReLU`, `MaxPooling2D`, `Flatten`, `Dense` y `Softmax`, así como los cambios de dimensiones y la configuración de una CNN. Recomendado **antes de construir la red en NB04**.

### 3. Laboratorio de optimización y generalización de CNN

[**Abrir laboratorio de optimización CNN →**](Simulador_Laboratorio_Optimizacion_CNN.html)

Interpreta resultados, modifica parámetros, formula hipótesis y compara métricas obtenidas en Google Colab. Recomendado para la **actividad autónoma de la sección 13 del NB04**.

> Los simuladores apoyan la comprensión y la experimentación conceptual. No sustituyen el entrenamiento real ni generan métricas predictivas automáticamente.

---

## Notebooks prácticos

Ejercicios progresivos en Google Colab, desde los fundamentos hasta la construcción y evaluación de modelos.

- [**NB04 — Introducción práctica a las CNN (abrir en Colab)**](https://colab.research.google.com/github/juliopez/Taller-Fundamentos-Data-Science-Python/blob/main/Machine_Learning_2026/03_Notebooks/NB04_Introduccion_Practica_CNN.ipynb): carga de imágenes, arquitectura convolucional, entrenamiento, matriz de confusión y actividad autónoma de experimentación.
- [**Ver directorio completo de notebooks**](https://github.com/juliopez/Taller-Fundamentos-Data-Science-Python/tree/main/Machine_Learning_2026/03_Notebooks)

## Datasets

Conjuntos de datos para las actividades prácticas, incluidos los recursos de clasificación de imágenes.

- [**Dataset CNN — piedra, papel o tijeras**](https://github.com/juliopez/Taller-Fundamentos-Data-Science-Python/tree/main/Machine_Learning_2026/02_Datasets/dataset_cnn)
- [**Ver todos los datasets**](https://github.com/juliopez/Taller-Fundamentos-Data-Science-Python/tree/main/Machine_Learning_2026/02_Datasets)

## Presentaciones

Material correspondiente a las semanas de clases.

[Ver presentaciones](https://github.com/juliopez/Taller-Fundamentos-Data-Science-Python/tree/main/Machine_Learning_2026/01_Presentaciones)

## Evaluaciones

Material público asociado a las cuatro evaluaciones y los hitos del Proyecto Integrador.

[Ver evaluaciones](https://github.com/juliopez/Taller-Fundamentos-Data-Science-Python/tree/main/Machine_Learning_2026/04_Evaluaciones)

## Recursos complementarios

[Ver recursos](https://github.com/juliopez/Taller-Fundamentos-Data-Science-Python/tree/main/Machine_Learning_2026/05_Recursos)

[Ver archivos fuente de los simuladores](https://github.com/juliopez/Taller-Fundamentos-Data-Science-Python/tree/main/Machine_Learning_2026/06_Simuladores)

---

## Proyecto Integrador

El proyecto se desarrolla progresivamente durante el semestre:

**Problema → Dataset → CNN → Modelo definitivo → Optimización → Aplicación → Despliegue**

### Flujo tecnológico

**Google Colab → Modelo CNN → ONNX (cuando corresponda) → Streamlit → GitHub → Streamlit Community Cloud**

El producto final corresponde a una aplicación funcional de clasificación de imágenes, desplegada y accesible mediante una URL.

---

[← Volver al portal general](index.html)
