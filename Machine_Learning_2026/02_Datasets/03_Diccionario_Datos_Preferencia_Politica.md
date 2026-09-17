# Diccionario de datos
## Dataset sintético de preferencia política — Evaluación 2

### 1. Propósito
Este conjunto de datos fue creado **exclusivamente con fines pedagógicos** para una actividad de Machine Learning de clasificación multiclase. Contiene **5.000 observaciones completamente sintéticas** y no corresponde a personas reales, encuestados reales ni registros administrativos.

La variable objetivo es `preferencia_politica`, con tres clases: **Izquierda, Centro y Derecha**.

La clasificación política del dataset **no debe interpretarse como una afirmación empírica sobre la población chilena**, ni las asociaciones incorporadas deben utilizarse para inferir cómo piensan o votan personas reales.

### 2. Inspiración conceptual
La definición de las tres categorías se inspira, de manera general, en escalas de autoposicionamiento político izquierda–derecha utilizadas en estudios de opinión pública chilenos. Como referencia conceptual, la Encuesta CEP utiliza una escala de 1 a 10, donde 1 representa izquierda y 10 derecha, y en sus reportes agrupa **1–4 como Izquierda, 5–6 como Centro y 7–10 como Derecha**.

**Importante:** este dataset **no utiliza microdatos de la Encuesta CEP**, no reproduce sus distribuciones ni resultados y no pretende simular estadísticamente una encuesta CEP. La referencia se limita a la idea general de representar orientación política mediante categorías izquierda, centro y derecha.

### 3. Variables

| Variable | Tipo | Rango / categorías | Descripción |
|---|---|---|---|
| `edad` | Numérica discreta | 18–85 | Edad sintética expresada en años. |
| `ingreso_mensual` | Numérica | $350.000–$5.500.000 aprox. | Ingreso mensual sintético en pesos chilenos. |
| `confianza_mercado` | Ordinal | 1–10 | Puntaje sintético de confianza en mecanismos de mercado. |
| `apoyo_redistribucion` | Ordinal | 1–10 | Puntaje sintético de apoyo a políticas redistributivas. |
| `valoracion_orden_publico` | Ordinal | 1–10 | Puntaje sintético asociado a valoración del orden público. |
| `preferencia_politica` | Categórica nominal — objetivo | Izquierda / Centro / Derecha | Clase sintética que debe predecir el modelo. |

### 4. Cómo se generaron los datos
Las variables fueron producidas mediante generación aleatoria reproducible y factores latentes sintéticos. Posteriormente se construyó internamente un **puntaje latente artificial** que combina las cinco variables predictoras con distintos pesos, una interacción y ruido.

En términos exclusivamente **generativos y pedagógicos**:
- mayor `apoyo_redistribucion` desplaza el puntaje sintético hacia Izquierda;
- mayor `confianza_mercado` lo desplaza hacia Derecha;
- mayor `valoracion_orden_publico` incorpora una asociación sintética más moderada hacia Derecha;
- `edad` e `ingreso_mensual` poseen efectos sintéticos deliberadamente débiles;
- se incorporó ruido para generar solapamiento y evitar reglas deterministas.

Estas relaciones **fueron inventadas para crear una señal aprendible por un algoritmo**. No constituyen resultados de investigación ni evidencia sobre asociaciones políticas reales.

### 5. Distribución de la variable objetivo
El conjunto fue diseñado para evitar una clase extremadamente dominante:
- Izquierda: aproximadamente 32 %
- Centro: aproximadamente 36 %
- Derecha: aproximadamente 32 %

Por ello, un baseline que prediga siempre la clase mayoritaria debería alcanzar aproximadamente **36 % de accuracy**.

### 6. Uso pedagógico previsto
El dataset permite trabajar identificación de `X` e `y`, preparación y escalamiento, codificación del objetivo, separación entrenamiento/prueba, diseño de una red neuronal multiclase, selección de capas ocultas y neuronas, hiperparámetros, baseline, experimentación y comparación de configuraciones.

### 7. Advertencia de interpretación
Este recurso debe tratarse como un **problema artificial de Machine Learning**. Un modelo entrenado con estos datos aprende únicamente las reglas estadísticas sintéticas incorporadas durante su generación. **No debe utilizarse para clasificar, perfilar o inferir la orientación política de personas reales.**
