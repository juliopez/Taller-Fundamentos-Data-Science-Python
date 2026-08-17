## Syllabus — Machine Learning 2026

| Sesión | Unidad                                           | Contenido principal                                                                                                                                                                                                                          |
| ------ | ------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1**  | **UA1. Introducción al Machine Learning**        | **Introducción al Machine Learning:** conceptos fundamentales. Diferencias entre IA, Machine Learning y Deep Learning. Tipos de problemas abordables mediante ML.                                                                            |
| **2**  | UA1                                              | **Tipos de aprendizaje automático:** aprendizaje supervisado, no supervisado y por refuerzo. Identificación del tipo de aprendizaje según el problema.                                                                                       |
| **3**  | UA1                                              | **Aprendizaje supervisado y redes neuronales:** datos de entrada, características, etiquetas, entrenamiento y predicción. Neurona artificial, capas, pesos, funciones de activación y *backpropagation*. Python aplicado a Machine Learning. |
| **4**  | UA1                                              | **Aplicación y Evaluación 1 (15%):** desarrollo de una primera solución mediante una red neuronal básica. Definición del problema de clasificación de imágenes y prototipo inicial.                                                          |
| **5**  | **UA2. Recolección y preparación de datos**      | **Datos para Machine Learning:** tipos de datos, características, etiquetas y requerimientos de un dataset según el problema. Ciclo de vida de los datos.                                                                                    |
| **6**  | UA2                                              | **Preparación y etiquetado de datos:** obtención, limpieza, transformación, organización y etiquetado de imágenes.                                                                                                                           |
| **7**  | UA2                                              | **Generalización y validación:** conjuntos de entrenamiento, validación y prueba. Generalización, *overfitting*, validación cruzada y aumento de datos (*data augmentation*).                                                                |
| **8**  | UA2                                              | **Aplicación y Evaluación 2 (20%):** construcción, preparación, etiquetado y validación del dataset definitivo del proyecto integrador.                                                                                                      |
| **9**  | **UA3. Deep Learning**                           | **Fundamentos de Deep Learning:** redes neuronales profundas, arquitectura, capas, entrenamiento, funciones de pérdida y optimización.                                                                                                       |
| **10** | UA3                                              | **Redes neuronales convolucionales (CNN):** convolución, filtros, mapas de características, *pooling* y clasificación de imágenes.                                                                                                           |
| **11** | UA3                                              | **Arquitecturas de Deep Learning:** LSTM, autoencoders y GAN. Características, diferencias y principales ámbitos de aplicación.                                                                                                              |
| **12** | UA3                                              | **Entrenamiento y mejora de CNN:** entrenamiento, evaluación, tratamiento del *overfitting*, Transfer Learning y Fine-Tuning. Modelos preentrenados y ajuste para problemas específicos.                                                     |
| **13** | UA3                                              | **Aplicación y Evaluación 3 (40%):** diseño, entrenamiento, evaluación y selección del modelo CNN definitivo del proyecto integrador.                                                                                                        |
| **14** | **UA4. Implementación de modelos Deep Learning** | **Optimización de modelos:** velocidad de inferencia, utilización de recursos, cuantificación y poda de redes.                                                                                                                               |
| **15** | UA4    | **Interoperabilidad y ONNX:** conversión y portabilidad de modelos. Exportación del modelo CNN desarrollado en **Google Colab** y preparación para su utilización fuera del entorno de entrenamiento.                                                                       |
| **16** | UA4    | **Implementación de modelos:** desarrollo de una aplicación de clasificación de imágenes mediante **Streamlit**. Integración del modelo, carga y preprocesamiento de imágenes nuevas, inferencia y presentación de clase predicha y nivel de confianza.                     |
| **17** | UA4    | **Despliegue y operación:** versionado del proyecto mediante **GitHub** y despliegue de la aplicación mediante **Streamlit Community Cloud**. Introducción a integración continua, servicios cloud de Machine Learning y Edge Computing. Pruebas de la solución desplegada. |
| **18** | UA4    | **Aplicación y Evaluación 4 (25%):** optimización, implementación y presentación del producto final operativo. Demostración de la aplicación desplegada y accesible mediante una **URL**, ejecutando inferencias sobre imágenes nuevas.                                     |

---

## Proyecto integrador - Entregas  (Evaluaciones Parciales)

| Evaluación       |    Peso | Aporte al proyecto integrador                                            | Rol                     |
| ---------------- | ------: | ------------------------------------------------------------------------ | ----------------------- |
| **Evaluación 1** | **15%** | Definición del problema y primera aproximación mediante una red neuronal | Fundacional             |
| **Evaluación 2** | **20%** | Construcción, preparación, etiquetado y validación del dataset           | Preparación             |
| **Evaluación 3** | **40%** | Diseño, entrenamiento y evaluación del modelo Deep Learning              | **Núcleo del proyecto** |
| **Evaluación 4** | **25%** | Optimización, implementación y puesta en producción del modelo           | Implementación          |

---

## Producto final propuesto

Al finalizar la asignatura, cada **dupla** deberá presentar una **solución funcional de Deep Learning para resolver un problema de clasificación basado en imágenes**, desarrollada incrementalmente durante las cuatro evaluaciones.

La solución final debería contener cinco componentes:

1. **Problema definido y justificable.** Debe existir una necesidad concreta que pueda resolverse mediante clasificación de imágenes. Por ejemplo: clasificación de residuos, reconocimiento de tipos de plantas, identificación de productos, clasificación de señales, detección de categorías de objetos, etc.

2. **Dataset propio o adaptado.** El conjunto de datos debe encontrarse organizado, etiquetado y dividido apropiadamente para entrenamiento, validación y prueba. El estudiante deberá documentar su preparación y, cuando corresponda, aplicar *data augmentation*.

3. **Modelo Deep Learning entrenado.** El núcleo será una **CNN**, porque el descriptor exige explícitamente como evidencia de la tercera evaluación el entrenamiento de una red convolucional. El modelo deberá entrenarse, evaluarse y quedar almacenado para su posterior utilización. 

4. **Aplicación funcional que utilice el modelo.** No bastará con entregar un notebook que entrene la red. El modelo desarrollado en Google Colab deberá integrarse en una aplicación construida mediante **Streamlit**, capaz de recibir una imagen nueva y entregar la **clase predicha y su nivel de confianza**. El código y los archivos necesarios para su ejecución deberán mantenerse en un repositorio **GitHub**.

5. **Modelo implementado, optimizado y desplegado.** El modelo entrenado deberá ser optimizado considerando velocidad de inferencia y utilización de recursos y, cuando corresponda, exportado mediante **ONNX**. La aplicación será desplegada mediante **Streamlit Community Cloud** a partir del repositorio GitHub del proyecto, permitiendo acceder al producto final mediante una URL y realizar inferencias sobre imágenes nuevas. Esta etapa permitirá abordar además los conceptos de integración continua, servicios cloud y Edge Computing considerados en la UA4.

El resultado final se resume como: 

**Usuario carga una imagen → aplicación procesa la imagen → modelo CNN realiza inferencia → aplicación muestra clasificación + probabilidad/confianza.**

---

## Entorno tecnológico del proyecto integrador

El desarrollo del proyecto integrador utilizará un flujo tecnológico común durante la asignatura, orientado a facilitar el desarrollo, entrenamiento, versionado e implementación del modelo sin requerir la instalación local de herramientas especializadas.

El flujo de trabajo será:

**Google Colab → Modelo CNN → ONNX (cuando corresponda) → Streamlit → GitHub → Streamlit Community Cloud**

* **Google Colab:** entorno principal para programación en Python, preparación de datos, entrenamiento, evaluación y optimización del modelo.
* **ONNX:** formato de interoperabilidad para la exportación del modelo cuando corresponda.
* **Streamlit:** desarrollo de la aplicación que permitirá al usuario cargar una imagen y obtener la clasificación generada por el modelo.
* **GitHub:** almacenamiento y versionado del código, dependencias y archivos necesarios para la aplicación.
* **Streamlit Community Cloud:** despliegue de la aplicación desde el repositorio GitHub, permitiendo disponer del producto final mediante una URL accesible desde un navegador.

El flujo completo del proyecto será:

**Dataset → Google Colab → entrenamiento CNN → modelo entrenado/optimizado → ONNX (cuando corresponda) → aplicación Streamlit → GitHub → Streamlit Community Cloud → aplicación disponible mediante URL.**

---

## Notebooks prácticos del proyecto integrador

| NB       | Semanas asociadas | Nombre propuesto                                       | Contenidos prácticos principales                                                                                                                                                                           | Relación con proyecto                           |
| -------- | ----------------- | ------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------- |
| **NB01** | **Semanas 3–4**   | **Introducción a redes neuronales**                    | Python aplicado a ML, construcción de una red neuronal básica, capas, activaciones, función de pérdida, optimizador, entrenamiento, validación y predicción.                                               | **E1:** prototipo funcional inicial.            |
| **NB02** | **Semanas 5–8**   | **Preparación y gestión del dataset de imágenes**      | Carga y exploración de imágenes, clases y etiquetas, limpieza, redimensionamiento, normalización, organización del dataset, división train/validation/test, balance de clases y Data Augmentation.         | **E2:** dataset preparado y validado.           |
| **NB03** | **Semanas 9–10**  | **Construcción y entrenamiento de una CNN**            | Convolución, filtros, mapas de características, padding, stride, pooling, construcción de una CNN, entrenamiento, curvas de aprendizaje y evaluación inicial.                                              | Inicio técnico de **E3**.                       |
| **NB04** | **Semanas 12–13** | **Transfer Learning, Fine-Tuning y evaluación de CNN** | Modelos preentrenados, extracción de características, Transfer Learning, Fine-Tuning, regularización, evaluación con test, matriz de confusión, métricas por clase y selección del modelo definitivo.      | **E3:** modelo CNN definitivo.                  |
| **NB05** | **Semanas 14–15** | **Optimización, interoperabilidad y ONNX**             | Baseline de tamaño y latencia, cuantificación, poda, comparación antes/después, exportación/conversión a ONNX, ONNX Runtime y validación de inferencia.                                                    | Inicio de **E4:** modelo optimizado y portable. |
| **NB06** | **Semanas 16–17** | **Aplicación y despliegue del modelo**                 | Pipeline de inferencia, carga de imágenes nuevas, clase y confianza, aplicación con Streamlit, preparación de `app.py` y `requirements.txt`, repositorio GitHub y despliegue en Streamlit Community Cloud. | **E4:** producto final desplegado mediante URL. |

---

## Entregas parciales

| Evaluación       |   Pond. | Entrega / Hito                                    | Contenidos de la entrega                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ---------------- | ------: | ------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Evaluación 1** | **15%** | **Definición del problema y prototipo inicial**   | • Definición del problema de clasificación de imágenes.<br>• Contexto y justificación de la problemática.<br>• Definición de las clases que se pretende reconocer.<br>• Identificación del tipo de aprendizaje automático utilizado y justificación.<br>• Descripción del flujo de una solución de aprendizaje supervisado.<br>• Identificación de los componentes básicos de una red neuronal: entrada, capas, pesos, activaciones y salida.<br>• Explicación conceptual del entrenamiento y *backpropagation*.<br>• Implementación y entrenamiento de una **red neuronal básica como primera aproximación** al problema.<br>• Análisis preliminar de los resultados obtenidos.                                                                                                          |
| **Evaluación 2** | **20%** | **Construcción y preparación del dataset**        | • Obtención o construcción del conjunto de imágenes del proyecto.<br>• Definición y organización de las clases.<br>• Etiquetado de las imágenes.<br>• Exploración y caracterización del dataset.<br>• Detección de problemas de calidad y balance entre clases.<br>• Preprocesamiento de imágenes.<br>• Separación de datos en entrenamiento, validación y prueba.<br>• Aplicación de técnicas de **aumento de datos (*data augmentation*)**.<br>• Análisis de generalización y riesgo de *overfitting*.<br>• Estrategia de validación de los datos/modelo.<br>• Generación del **dataset definitivo** que utilizará el proyecto.                                                                                                                                                         |
| **Evaluación 3** | **40%** | **Diseño, entrenamiento y evaluación de la CNN**  | • Diseño de la arquitectura de la **Red Neuronal Convolucional (CNN)**.<br>• Definición de capas convolucionales, funciones de activación, *pooling* y capas de clasificación.<br>• Configuración del proceso de entrenamiento.<br>• Entrenamiento de la CNN utilizando el dataset preparado en E2.<br>• Seguimiento del entrenamiento y validación.<br>• Evaluación del comportamiento de entrenamiento frente a validación.<br>• Identificación y tratamiento del *overfitting*.<br>• Experimentación y ajuste del modelo.<br>• Aplicación de **Fine-Tuning**, cuando corresponda.<br>• Evaluación del modelo utilizando datos de prueba.<br>• Análisis e interpretación de resultados.<br>• Selección y almacenamiento del **modelo definitivo**.                                      |
| **Evaluación 4** | **25%** | **Optimización, implementación y producto final** | • Recuperación del modelo generado en E3.<br>• Análisis de requerimientos para su implementación.<br>• Optimización considerando velocidad de inferencia y utilización de recursos.<br>• Aplicación o análisis de **cuantificación** y **poda de redes**.<br>• Conversión/interoperabilidad mediante **ONNX**, cuando corresponda.<br>• Desarrollo de una aplicación mediante **Streamlit** que permita ingresar una imagen nueva.<br>• Integración del modelo con la aplicación.<br>• Presentación de la clase predicha y nivel de confianza.<br>• Organización y versionado del proyecto mediante **GitHub**.<br>• Despliegue mediante **Streamlit Community Cloud**.<br>• Disponibilidad de la aplicación mediante una **URL funcional**.<br>• Consideraciones de servicios cloud, integración continua y **Edge Computing**.<br>• Pruebas funcionales utilizando imágenes nuevas.<br>• Presentación y demostración del **producto final operativo**. |

