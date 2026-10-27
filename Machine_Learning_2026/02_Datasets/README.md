# Datasets para aprender Machine Learning

Este directorio reúne **conjuntos de datos de práctica** y su documentación para explorar, limpiar, preparar y modelar datos. Los materiales son abiertos y pueden utilizarse de forma autónoma, sin seguir un calendario ni participar en un curso formal.

Algunos datasets contienen **datos sintéticos** o inconsistencias intencionales con fines de aprendizaje. Revisa siempre el diccionario correspondiente antes de formular conclusiones.

## Catálogo de datos

| Dataset | Documentación | Uso sugerido |
|---|---|---|
| [Clientes de retail — limpieza de datos](01_clientes_retail_video_limpieza.csv) | [Diccionario de datos](01_Diccionaro_Datos_clientes_retail_video_limpieza.md) | Detectar valores faltantes, duplicados, categorías inconsistentes y valores atípicos. |
| [Encuesta de participación y opinión ciudadana (datos ficticios)](02_Encuesta_Participacion_Opinion_Ciudadana_2026.csv) | [Diccionario de datos](02_Diccionario_Datos_Encuesta_Participacion_Opinion_Ciudadana_2026.md) | Explorar variables numéricas y categóricas, distribuciones y preparación de datos. |
| [Preferencia política sintética](03_Preferencia_Politica_Sintetica.csv) | [Diccionario de datos](03_Diccionario_Datos_Preferencia_Politica.md) | Practicar clasificación multiclase con datos completamente artificiales. |
| [Imágenes de piedra, papel o tijeras](dataset_cnn/) | [Descripción y guía de carga](dataset_cnn/README.md) | Construir y evaluar una primera red neuronal convolucional (CNN). |

## Cómo utilizar estos recursos

1. **Selecciona un problema:** exploración, limpieza, clasificación o procesamiento de imágenes.
2. **Lee el diccionario o la guía:** identifica las variables, clases, estructura y advertencias de interpretación.
3. **Inspecciona los datos:** revisa formatos, valores faltantes, distribuciones, etiquetas y posibles inconsistencias.
4. **Prepara el conjunto:** documenta las transformaciones y separa entrenamiento, validación y prueba cuando corresponda.
5. **Experimenta e interpreta:** aplica tus métodos y justifica los resultados con evidencia.

## Consideraciones importantes

- Los datasets de ejemplo no deben confundirse con estadísticas oficiales ni con datos de personas reales cuando se identifican como sintéticos.
- Algunas anomalías de calidad están incorporadas deliberadamente para facilitar ejercicios de limpieza.
- Los archivos de imágenes se distribuyen en tres ZIP por categoría; consulta su [guía específica](dataset_cnn/README.md) antes de utilizarlos.
- Para conservar la reproducibilidad, registra la procedencia de los datos, las decisiones de preparación y los criterios de evaluación.

## Más recursos

- [Notebooks y ruta de aprendizaje](../03_Notebooks/README.md)
- [Recursos de evaluación de modelos](../04_Recursos/README.md)
- [Portal educativo](https://juliopez.github.io/Taller-Fundamentos-Data-Science-Python/)

**Este catálogo es un punto de partida flexible:** puedes utilizar los datasets por separado o combinarlos con las actividades de los notebooks según tus intereses y conocimientos.
