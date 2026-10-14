# Dataset de imágenes para la primera CNN: Piedra, papel o tijera

Este directorio contiene el conjunto de imágenes que utilizaremos en la actividad práctica de **introducción a las redes neuronales convolucionales (CNN)** del curso *Machine Learning 2026*.

En experiencias anteriores trabajamos con **redes neuronales densas (MLP) y datos numéricos estructurados**. En esta actividad utilizaremos **imágenes** y aprenderemos cómo las capas convolucionales extraen características visuales antes de la clasificación.

## 1. Descripción del dataset

El problema consiste en reconocer tres gestos de la mano:

| Etiqueta | Categoría | Imágenes de entrenamiento | Imágenes de prueba | Total |
|---|---|---:|---:|---:|
| `rock` | Piedra | 150 | 50 | 200 |
| `paper` | Papel | 150 | 50 | 200 |
| `scissors` | Tijera | 150 | 50 | 200 |
| **Total** | | **450** | **150** | **600** |

Las imágenes están en formato **PNG**. Se trata de una selección reducida y equilibrada del dataset público *Rock, Paper, Scissors* utilizado en ejemplos de TensorFlow; la selección se realizó con semilla aleatoria fija (`42`).

**Origen de los datos:** [dataset de entrenamiento](https://storage.googleapis.com/download.tensorflow.org/data/rps.zip) y [dataset de prueba](https://storage.googleapis.com/download.tensorflow.org/data/rps-test-set.zip).

## 2. Archivos disponibles

Para permitir su publicación mediante la interfaz web de GitHub, el dataset está distribuido en **tres archivos ZIP independientes**, uno por categoría:

| Archivo | Tamaño aproximado | Imágenes |
|---|---:|---:|
| [dataset_cnn_rock.zip](dataset_cnn_rock.zip) | 14,5 MiB | 200 |
| [dataset_cnn_paper.zip](dataset_cnn_paper.zip) | 15,4 MiB | 200 |
| [dataset_cnn_scissors.zip](dataset_cnn_scissors.zip) | 15,6 MiB | 200 |

**Es necesario utilizar los tres archivos.** No existe un único `dataset_cnn.zip` publicado en esta carpeta.

Cada archivo comprimido contiene su categoría dentro de la misma estructura de directorios. Al extraerlos en una ubicación común, se reconstruye automáticamente:

```text
dataset_cnn/
├── train/
│   ├── rock/        # 150 imágenes
│   ├── paper/       # 150 imágenes
│   └── scissors/    # 150 imágenes
└── test/
    ├── rock/        # 50 imágenes
    ├── paper/       # 50 imágenes
    └── scissors/    # 50 imágenes
```

## 3. Actividad del estudiante: cargar imágenes en Google Colab

En esta experiencia **no utilizaremos Google Drive**. Descargaremos los archivos del repositorio y los cargaremos manualmente al entorno temporal de Google Colab.

1. Descarga los tres ZIP desde los enlaces de la sección anterior (en la página de cada archivo, utiliza la opción de descarga).
2. Abre el notebook de la actividad en **Google Colab**.
3. Ejecuta la celda de carga de archivos; cuando aparezca el selector, elige los **tres ZIP** descargados.
4. Ejecuta la celda que descomprime los archivos en `/content/`.
5. Verifica la existencia de las carpetas `train/rock`, `train/paper`, `train/scissors` y sus equivalentes en `test/`.

Como referencia, este código permite **cargar manualmente** los ZIP desde el computador:

```python
from google.colab import files

archivos = files.upload()  # Seleccionar los tres ZIP
```

Y el siguiente permite reconstruir las carpetas del dataset:

```python
from pathlib import Path
from zipfile import ZipFile

destino = Path("/content")

for nombre in [
    "dataset_cnn_rock.zip",
    "dataset_cnn_paper.zip",
    "dataset_cnn_scissors.zip",
]:
    if nombre not in archivos:
        raise FileNotFoundError(f"Falta cargar: {nombre}")
    with ZipFile(nombre) as zip_file:
        zip_file.extractall(destino)

dataset_dir = destino / "dataset_cnn"
print("Directorio creado:", dataset_dir)
```

**Nota:** el almacenamiento de Colab es temporal. Si se reinicia el entorno, normalmente será necesario volver a cargar los archivos.

## 4. Uso previsto en el notebook CNN

- **Entrenamiento:** se partirá de las 450 imágenes de `train/`. En el notebook se reservará un **20 % para validación** (360 entrenamiento y 90 validación).
- **Prueba:** las 150 imágenes de `test/` permanecerán separadas y se utilizarán para evaluar el modelo después del entrenamiento.
- **Preprocesamiento:** redimensionamiento y normalización, según las instrucciones del notebook.
- **Modelo:** primera CNN sencilla, con explicaciones en Markdown de `Conv2D`, `MaxPooling2D`, `Flatten` y las capas densas.

**Importante:** no mezclar imágenes de `test/` con `train/` durante el entrenamiento ni durante la selección de parámetros del modelo.

## 5. Propósito pedagógico

Este dataset permite practicar una nueva forma de ingresar información al modelo. A diferencia de los ejercicios previos de MLP con datos tabulares, las imágenes tienen una **estructura espacial** (alto, ancho y canales) que las CNN aprovechan mediante convoluciones.

El objetivo de la actividad es **comprender el proceso completo de construcción de una primera CNN**, no alcanzar necesariamente la máxima precisión de clasificación.
