# Ruta de aprendizaje: Machine Learning y Deep Learning

Este directorio reúne **notebooks prácticos y abiertos** para aprender, experimentar y desarrollar soluciones con Machine Learning y redes neuronales. Puedes seguirlos **a tu propio ritmo**, tanto si estudias por cuenta propia como si participas en una actividad de formación. **No requieren estar inscrito en una asignatura ni siguen un calendario académico o una pauta de evaluaciones.**

Los recursos están organizados para avanzar desde la preparación y exploración de datos hacia las redes neuronales, la clasificación de imágenes y la puesta en funcionamiento de modelos. Cada notebook incluye explicaciones y actividades de práctica; algunos emplean datos públicos y otros permiten trabajar con un dataset propio.

## ¿Por dónde empezar?

- **Si comienzas desde cero:** revisa los dos notebooks de nivelación y exploración de datos; después sigue NB01 y NB02.
- **Si ya conoces las redes neuronales básicas:** comienza en NB03 y continúa con NB04 y NB05.
- **Si ya tienes una CNN entrenada:** explora NB06, NB07 y NB08 según tu objetivo.

## Notebooks disponibles

| Recurso | ¿Qué aprenderás? | Abrir en Colab |
|---|---|---|
| **NB00 — Nivelación de datos y Machine Learning** | Revisar conceptos fundamentales de datos y preparación para abordar los modelos de aprendizaje automático. | [Abrir notebook](https://colab.research.google.com/github/juliopez/Taller-Fundamentos-Data-Science-Python/blob/main/Machine_Learning_2026/03_Notebooks/NB00_Nivelacion_Datos_Machine_Learning.ipynb) |
| **NB00 — Exploración automática de datos** | Explorar perfiles, distribuciones y características de datasets con herramientas de análisis exploratorio. | [Abrir notebook](https://colab.research.google.com/github/juliopez/Taller-Fundamentos-Data-Science-Python/blob/main/Machine_Learning_2026/03_Notebooks/NB00_EDA_Automatico_ydata_profiling.ipynb) |
| **NB01 — Introducción a redes neuronales** | Comprender y experimentar con redes neuronales multicapa (MLP). | [Abrir notebook](https://colab.research.google.com/github/juliopez/Taller-Fundamentos-Data-Science-Python/blob/main/Machine_Learning_2026/03_Notebooks/NB01_Introduccion_Redes_Neuronales.ipynb) |
| **NB02 — Clasificación multiclase guiada** | Aplicar preparación de datos, entrenamiento e interpretación de un modelo de clasificación. | [Abrir notebook](https://colab.research.google.com/github/juliopez/Taller-Fundamentos-Data-Science-Python/blob/main/Machine_Learning_2026/03_Notebooks/NB02_Ejercicio_Guiado_Clasificacion_Multiclase.ipynb) |
| **NB03 — Preparación y gestión de imágenes** | Inspeccionar la calidad de las imágenes, revisar clases, preparar particiones y construir un pipeline de datos. | [Abrir notebook](https://colab.research.google.com/github/juliopez/Taller-Fundamentos-Data-Science-Python/blob/main/Machine_Learning_2026/03_Notebooks/NB03_Preparacion_Gestion_Dataset_Imagenes.ipynb) |
| **NB04 — Primera CNN: práctica guiada** | Construir y evaluar una CNN sencilla con imágenes de piedra, papel o tijeras. | [Abrir notebook](https://colab.research.google.com/github/juliopez/Taller-Fundamentos-Data-Science-Python/blob/main/Machine_Learning_2026/03_Notebooks/NB04_Introduccion_Practica_CNN.ipynb) |
| **NB05 — CNN con imágenes propias** | Adaptar el procedimiento a un dataset elegido por quien aprende y comparar arquitecturas. | [Abrir notebook](https://colab.research.google.com/github/juliopez/Taller-Fundamentos-Data-Science-Python/blob/main/Machine_Learning_2026/03_Notebooks/NB05_Construccion_Entrenamiento_CNN.ipynb) |
| **NB06 — Transfer Learning y Fine-Tuning** | Reutilizar modelos preentrenados, ajustar capas y evaluar el comportamiento de la CNN. | [Abrir notebook](https://colab.research.google.com/github/juliopez/Taller-Fundamentos-Data-Science-Python/blob/main/Machine_Learning_2026/03_Notebooks/NB06_Transfer_Learning_Fine_Tuning_Evaluacion_CNN.ipynb) |
| **NB07 — Optimización e interoperabilidad** | Explorar técnicas de optimización y exportación del modelo para su uso en otros entornos. | [Abrir notebook](https://colab.research.google.com/github/juliopez/Taller-Fundamentos-Data-Science-Python/blob/main/Machine_Learning_2026/03_Notebooks/NB07_Optimizacion_Interoperabilidad_ONNX.ipynb) |
| **NB08 — Aplicación y despliegue** | Integrar el modelo en una aplicación y abordar su publicación y uso. | [Abrir notebook](https://colab.research.google.com/github/juliopez/Taller-Fundamentos-Data-Science-Python/blob/main/Machine_Learning_2026/03_Notebooks/NB08_Aplicacion_Despliegue_Modelo.ipynb) |

## Ruta sugerida

**Fundamentos y exploración de datos → MLP y clasificación → preparación de imágenes → primera CNN guiada → CNN con imágenes propias → Transfer Learning → optimización → aplicación y despliegue**

### Una ruta práctica para aprender CNN

Los notebooks **NB03, NB04 y NB05** forman una progresión especialmente útil:

1. **NB03 — Preparar los datos:** conocer un dataset público de flores, verificar su calidad, organizar las clases y preparar los conjuntos de entrenamiento, validación y prueba. **Aquí todavía no se entrena una CNN.**
2. **NB04 — Comprender el modelo:** construir y evaluar paso a paso una CNN pequeña con un dataset común de piedra, papel o tijeras. El cambio de dataset es deliberado para concentrarse en comprender las capas y el entrenamiento.
3. **NB05 — Aplicar lo aprendido:** utilizar imágenes propias, construir una CNN adecuada al problema y comparar configuraciones a partir de sus resultados.

Los tres recursos están relacionados conceptualmente, **aunque no utilizan obligatoriamente el mismo dataset**. Puedes repetir, adaptar o ampliar cualquier etapa antes de continuar.

### Continuar después de la primera CNN

- **NB06:** explorar modelos preentrenados y estrategias de ajuste (*Transfer Learning* y *Fine-Tuning*).
- **NB07:** estudiar optimización e interoperabilidad, incluyendo ONNX.
- **NB08:** avanzar hacia una aplicación que utilice el modelo y su despliegue.

## Cómo utilizar los notebooks en Google Colab

1. Selecciona **Abrir notebook** en la tabla.
2. En Google Colab, crea una copia en tu Google Drive para conservar tus cambios.
3. Lee las explicaciones antes de ejecutar cada celda y comprueba los resultados obtenidos.
4. Sigue las indicaciones de cada notebook para descargar datos públicos o conectar tus imágenes en Drive cuando sea necesario.
5. Registra qué decisiones tomaste, qué cambiaste y cómo interpretas los resultados; no te limites a ejecutar el código.

Para comenzar puedes utilizar Google Colab desde el navegador, sin instalar Python localmente. Algunas actividades necesitan conexión a Internet, archivos de datos o permisos para acceder a Google Drive. La disponibilidad de CPU o GPU puede influir en los tiempos de entrenamiento.

## Recursos complementarios

- [Repositorio principal](../../../README.md)
- [Directorio de datasets](../02_Datasets/)
- [Simuladores interactivos](../06_Simuladores/)
- [Portal de recursos educativos](https://juliopez.github.io/Taller-Fundamentos-Data-Science-Python/)

**Sugerencia:** utiliza esta colección como un itinerario flexible. El propósito es comprender cada etapa, experimentar y ser capaz de aplicar los procedimientos a nuevos conjuntos de datos.
