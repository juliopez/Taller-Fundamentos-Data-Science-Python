# Diccionario de datos — NB06_CIFAR10

## 1. Identificación

| Campo | Descripción |
|---|---|
| Nombre | NB06_CIFAR10 |
| Dataset de origen | CIFAR-10 |
| Fuente oficial | [Canadian Institute for Advanced Research / Universidad de Toronto](https://www.cs.toronto.edu/~kriz/cifar.html) |
| Referencia | Krizhevsky, A. (2009). *Learning Multiple Layers of Features from Tiny Images*. |
| Actividad | NB06 — Evaluación de modelos de clasificación de imágenes preentrenados |
| Tipo de datos | Imágenes RGB etiquetadas |
| Formato previsto | PNG |
| Resolución original | 32 × 32 píxeles |
| Unidad de análisis | Una imagen |
| Selección prevista | 20 imágenes (2 por categoría, 10 categorías) |
| Estado del inventario | Verificado en GitHub: 20 imágenes PNG, 2 por cada una de las 10 categorías, más `etiquetas.csv` |

## 2. Propósito pedagógico

Este subconjunto se empleará en el NB06 para realizar inferencia con **MobileNetV2 preentrenada en ImageNet**, sin reentrenar el modelo. El objetivo es contrastar las predicciones del modelo con las etiquetas de referencia, explorar aciertos y errores y discutir las limitaciones de resolución y de correspondencia entre los conjuntos de clases.

La segunda parte del NB06 permitirá repetir la experiencia con imágenes del dataset propio desarrollado por cada estudiante en NB05.

**Importante:** las categorías de CIFAR-10 y las clases de ImageNet no son equivalentes uno a uno. La evaluación de las predicciones requiere interpretar las etiquetas y no se reduce a comparar textos idénticos.

## 3. Diccionario de variables

| Campo | Tipo | Descripción | Ejemplo |
|---|---|---|---|
| `archivo` | Texto | Ruta relativa del archivo de imagen dentro del conjunto | `gato/gato_01.png` |
| `categoria` | Texto categórico | Etiqueta de referencia CIFAR-10 de la imagen | `gato` |
| `imagen` | Imagen RGB | Matriz de píxeles de 32 × 32 × 3 | Archivo PNG |

Las variables `archivo` y `categoria` están registradas en `etiquetas.csv`, incorporado junto con las imágenes.

## 4. Categorías previstas

| Código CIFAR-10 | Categoría original | Nombre de carpeta propuesto | Imágenes previstas |
|---:|---|---|---:|
| 0 | airplane | avion | 2 |
| 1 | automobile | automovil | 2 |
| 2 | bird | pajaro | 2 |
| 3 | cat | gato | 2 |
| 4 | deer | ciervo | 2 |
| 5 | dog | perro | 2 |
| 6 | frog | rana | 2 |
| 7 | horse | caballo | 2 |
| 8 | ship | barco | 2 |
| 9 | truck | camion | 2 |
| **Total** | **10 categorías** | | **20** |

## 5. Estructura de archivos verificada

```text
NB06_CIFAR10/
├── README.md
├── avion/
│   ├── avion_01.png
│   └── avion_02.png
├── automovil/
├── pajaro/
├── gato/
├── ciervo/
├── perro/
├── rana/
├── caballo/
├── barco/
└── camion/
```

Cada carpeta representa la categoría de referencia. Para el análisis se puede obtener esa etiqueta a partir del nombre de la carpeta, sin exigir que el estudiante abra un ZIP ni manipule un CSV.

## 6. Procedencia y criterio de selección

La muestra planificada procede de CIFAR-10, que contiene imágenes de diez clases. Se ha definido una selección pequeña de **dos ejemplos por categoría** para privilegiar la observación, comparación y discusión de resultados sobre el volumen de datos.

La selección se realizó mediante un script a partir del lote de prueba de CIFAR-10, tomando las primeras dos imágenes encontradas de cada clase. **Se verificó en GitHub la existencia de las diez carpetas y de dos archivos PNG por categoría.**

## 7. Consideraciones metodológicas

- **Resolución:** las imágenes originales son pequeñas (32 × 32). Para MobileNetV2 se redimensionarán al tamaño de entrada requerido. Esto **no recupera** detalle perdido en la imagen original.
- **Etiquetas:** MobileNetV2 con pesos ImageNet predice clases de ImageNet, no directamente las diez categorías de CIFAR-10.
- **Confianza:** las probabilidades reportadas por el modelo no garantizan por sí solas que la clasificación sea correcta ni constituyen necesariamente una confianza calibrada.
- **Evaluación:** distinguir entre predicción coherente, parcialmente relacionada o incorrecta, justificando la decisión con la imagen, la etiqueta real y la salida del modelo.
- **Alcance:** 20 imágenes sirven para una actividad didáctica exploratoria; **no** representan una evaluación estadística robusta del rendimiento del modelo.

## 8. Uso en NB06

1. Cargar automáticamente desde GitHub las imágenes organizadas por categoría.
2. Mostrar imágenes y categorías de referencia.
3. Ejecutar inferencia con MobileNetV2.
4. Comparar predicciones y analizar aciertos, errores y confianza.
5. Repetir el procedimiento con un dataset propio de NB05.

> **Control de integridad realizado:** se verificó en GitHub la presencia de diez carpetas, dos imágenes PNG por carpeta y el archivo `etiquetas.csv`.
