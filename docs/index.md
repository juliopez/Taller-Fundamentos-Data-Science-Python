---
title: Taller Fundamentos de Data Science con Python
---

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.18705759.svg)](https://doi.org/10.5281/zenodo.18705759)

# Taller Fundamentos de Data Science con Python

Repositorio oficial del taller **“Fundamentos de Data Science con Python”**, orientado a académicas/os, docentes y estudiantes que desean incorporar herramientas de análisis de datos y modelos de IA en su práctica educativa.

---

## Cómo citar este material

López-Núñez, J. (2026). *Taller Fundamentos de Data Science con Python (Versión 1.0.0).* Zenodo. https://doi.org/10.5281/zenodo.18705759

---

## Licencia

Este material se distribuye bajo licencia **Creative Commons CC BY 4.0**, permitiendo uso y adaptación con atribución:  
https://creativecommons.org/licenses/by/4.0/

---

## 🎥 Lista de reproducción (YouTube)

Las clases grabadas del taller se encuentran disponibles en la siguiente lista de reproducción:

👉 [Ver Lista de Reproducción](https://www.youtube.com/playlist?list=PLrc3rKEj3Qc8MUhUOp6fYlg-f3W3lHX5z)

Se recomienda seguir el orden de las sesiones y trabajar en paralelo con los notebooks disponibles en este repositorio.

---

<nav class="main-nav">
  <a href="index">Inicio</a> |
  <a href="sesiones">Sesiones</a> |
  <a href="materiales">Materiales</a>
</nav>
<script>
  /* Detectar página activa */
  const path = window.location.pathname;

  document.querySelectorAll('.main-nav .nav-link').forEach(link => {
    if (path.includes(link.getAttribute('href'))) {
      link.classList.add('active');
    }
  });
</script>



## Navegación rápida

- 📘 **Sesiones**: [sesiones](sesiones.html)
- 📦 **Materiales**: [materiales](materiales.html)
---

## Público objetivo

- Docentes de educación superior, docentes escolares, profesionales o asistentes académicos.
- Interés en análisis de datos, evaluación, investigación educativa o innovación docente.
- No se requiere experiencia previa en programación avanzada, pero sí disposición a trabajar con ejemplos prácticos en Python.

---

## Objetivos del taller

Al finalizar el taller, las y los participantes serán capaces de:

- Comprender los conceptos fundamentales de **Data Science** y su ciclo de vida.  
- Manipular datos con **pandas** y **numpy**.  
- Realizar **Análisis Exploratorio de Datos (EDA)** y generar visualizaciones con `matplotlib` y `seaborn`.  
- Implementar modelos básicos de **Machine Learning** con `scikit-learn`.  
- Entrenar modelos sencillos de redes neuronales (**MLP**, **CNN**, **Transfer Learning**) con TensorFlow/Keras.  
- Reflexionar sobre aspectos éticos, sesgos y riesgos en el uso de IA.

---

## Estructura del repositorio

```text
Taller-Fundamentos-Data-Science-Python/
│
├── README.md
├── LICENSE
│
├── 00_Documentacion/
│   └── Descriptor-Taller.pdf
├── 01_Presentaciones/
│   ├── Sesion1_Introduccion_Python.pdf
│   ├── Sesion2_EDA.pdf
│   ├── Sesion3_ML.pdf
│   ├── Sesion4_DL.pdf
│   ├── Sesion5_CNN.pdf
│   └── Sesion6_Proyecto-Final.pdf
│
├── 02_Datasets/
│   ├── datos_estudiantes_200.csv
│   ├── datos_estudiantes_200_eda.csv
│   ├── datos_estudiantes_practica.csv
│   ├── datos_taller_integrador_STEM.csv
│   ├── Ejemplo1.csv
│   └── Ejemplo2.csv
│
├── 03_Notebooks/
│   ├── 01_Sesion_01.ipynb
│   ├── 02_Sesion_02.ipynb
│   ├── 03_Sesion_03.ipynb
│   ├── 04_Sesion_04.ipynb
│   └── 05_Sesion_05.ipynb
│
├── 04_Ejercicios_Practicos/
│   ├── 01_Sesion1_Guion_Practica.ipynb
│   ├── 02_Sesion2_Guion_Practica.ipynb
│   ├── 03_Sesion3_Guion_Practica.ipynb
│   ├── 04_Sesion4_Guion_Practica.ipynb
│   └── 05_Sesion5_Guion_Practica.ipynb
│
├── 05_Imagenes/
│   └── Set Imágenes para Clasificación
│
└── 06_Actividad_Final/
    ├── README.md
    ├── Taller_Integrador_STEM_PLANTILLA.ipynb
    └── RUBRICA.md
```
## Cómo usar este repositorio

### 1. Clonar o descargar el repositorio

```bash
git clone https://github.com/juliopez/Taller-Fundamentos-Data-Science-Python.git
cd Taller-Fundamentos-Data-Science-Python
```
### 2. Abrir los notebooks

Puedes abrir los notebooks:

- Localmente (Jupyter Notebook / VS Code), o  
- Directamente en **Google Colab**, recomendado para evitar configuraciones locales.

---

### 3. Revisar los datasets

Accede a la carpeta: `02_Datasets/`

Allí encontrarás los archivos `.csv` utilizados en cada sesión.

---

Para más detalles por sesión, revisa la sección [Sesiones](sesiones.md)

Para acceso directo a PDFs, datasets y notebooks, visita  · [Materiales](materiales.md)

---
## Autoría

**Autor:** Dr. Julio Lopez-Nunez  
**Institución:** Universidad de las Américas - Chile  
**Año:** 2025  

Para consultas o comentarios:  
📩 julio.lopez-nunez@uni-konstanz.de
