# Semana 10 — Redes Neuronales Convolucionales (CNN)

## 1. Propósito de la sesión

Comprender el funcionamiento de las **Redes Neuronales Convolucionales (CNN)** y su aplicación en clasificación de imágenes, identificando el papel que cumplen las convoluciones, los filtros, los mapas de características, las capas de *pooling* y las capas finales de clasificación.

Al finalizar la sesión, el estudiante debería ser capaz de:

* Explicar por qué las CNN son especialmente adecuadas para trabajar con imágenes.
* Comprender el concepto de convolución.
* Identificar el papel de los filtros o *kernels*.
* Comprender qué es un mapa de características.
* Explicar cómo una CNN aprende filtros durante el entrenamiento.
* Comprender la función de las capas de *pooling*.
* Identificar la progresión desde características simples hacia representaciones más complejas.
* Comprender la estructura general de una CNN para clasificación.
* Implementar conceptualmente una CNN sencilla utilizando Keras.
* Relacionar esta arquitectura con el núcleo del proyecto integrador.

---

# 2. El problema de utilizar redes densas con imágenes

Durante la Semana 4 construimos conceptualmente una red neuronal básica.

Supongamos una imagen:

**224 × 224 × 3**

Esto corresponde a:

**150.528 valores de entrada**

Si conectáramos directamente esos valores a una capa densa de:

**1.000 neuronas**

tendríamos aproximadamente:

**150.528 × 1.000 = 150.528.000 pesos**

sin contar los sesgos.

Esto implica:

* enorme cantidad de parámetros;
* mayor costo computacional;
* mayor necesidad de datos;
* mayor riesgo de overfitting.

Pero existe además otro problema.

Al convertir una imagen en un vector, perdemos explícitamente parte de su **estructura espacial**.

---

# 3. La estructura espacial de una imagen

Los píxeles de una imagen no son observaciones independientes.

Su posición relativa contiene información.

Por ejemplo:

un conjunto de píxeles vecinos puede formar:

**un borde**

Varios bordes pueden formar:

**una esquina**

Diversas formas pueden constituir:

**una parte de un objeto**

Por tanto:

**la relación espacial entre píxeles es importante.**

Las CNN están diseñadas precisamente para aprovechar esta característica.

En lugar de conectar cada píxel con todas las neuronas, utilizan operaciones locales que analizan pequeñas regiones de la imagen.

---

# 4. ¿Qué es una CNN?

Una **Convolutional Neural Network** o **Red Neuronal Convolucional** es una arquitectura de Deep Learning especialmente diseñada para procesar información con estructura espacial, como las imágenes.

Una arquitectura típica puede representarse como:

**Imagen**

↓

**Convolución**

↓

**Activación**

↓

**Pooling**

↓

**Convolución**

↓

**Activación**

↓

**Pooling**

↓

**Representación aprendida**

↓

**Clasificación**

↓

**Clase predicha**

Las primeras capas se encargan principalmente de **extraer características**.

Las capas finales utilizan esas características para realizar la clasificación.

---

# 5. Dos grandes componentes de una CNN

Podemos dividir conceptualmente una CNN en dos partes.

### Extracción de características

Incluye normalmente:

* convoluciones;
* funciones de activación;
* pooling.

Su objetivo es transformar la imagen original en representaciones útiles.

### Clasificación

Incluye normalmente:

* flatten o global pooling;
* capas densas;
* capa de salida.

Su objetivo es utilizar las características aprendidas para determinar la clase.

Por tanto:

**Imagen → Extracción de características → Clasificador → Clase**

---

# 6. ¿Qué es una convolución?

La **convolución** consiste en aplicar una pequeña matriz, denominada **filtro** o *kernel*, sobre diferentes regiones de la imagen.

Por ejemplo, un filtro puede tener tamaño:

**3 × 3**

y desplazarse progresivamente sobre la imagen.

En cada posición:

1. toma una pequeña región;
2. combina los valores de esa región con los valores del filtro;
3. genera un nuevo valor.

Al repetir esta operación obtenemos una nueva representación denominada:

**mapa de características** o *feature map*.

---

# 7. Ejemplo conceptual

Supongamos una pequeña región de imagen:

|  1 |  1 |  0 |
| -: | -: | -: |
|  1 |  0 |  0 |
|  0 |  0 |  0 |

y un filtro:

|  1 |  0 | -1 |
| -: | -: | -: |
|  1 |  0 | -1 |
|  1 |  0 | -1 |

La operación combina los valores posición por posición.

Conceptualmente:

**Región de imagen × Filtro → suma → valor de salida**

El filtro se desplaza después a otra posición y repite la operación.

No necesitamos realizar manualmente todas estas operaciones durante el proyecto.

Lo importante es comprender que el filtro analiza **pequeñas regiones locales** de la imagen.

---

# 8. Filtros o kernels

Un **filtro** es una pequeña matriz de valores.

Ejemplos habituales de tamaño:

**3 × 3**

**5 × 5**

En procesamiento tradicional de imágenes pueden diseñarse filtros manualmente para detectar:

* bordes verticales;
* bordes horizontales;
* cambios de intensidad;
* texturas.

La gran ventaja de una CNN es que normalmente:

**los filtros no deben definirse manualmente.**

Sus valores se convierten en **parámetros entrenables**.

La red aprende qué filtros son útiles para resolver el problema.

---

# 9. Los filtros también aprenden

Recordemos el proceso:

**Forward propagation**

↓

**Loss**

↓

**Backpropagation**

↓

**Optimización**

En una CNN, los valores internos de los filtros participan en este mismo proceso.

Inicialmente pueden comenzar con valores aleatorios.

Durante el entrenamiento:

**Filtro inicial**

↓

**Predicción**

↓

**Error**

↓

**Backpropagation**

↓

**Actualización del filtro**

Por tanto, la red aprende progresivamente **qué patrones visuales debe buscar**.

---

# 10. Feature map

El resultado de aplicar un filtro sobre una imagen recibe el nombre de:

**feature map**

o:

**mapa de características**

Si un filtro aprende a responder ante determinados bordes, el mapa correspondiente presentará valores altos en las regiones donde aparezca ese patrón.

Por ejemplo:

**Imagen original**

↓

**Filtro detector de bordes**

↓

**Mapa de bordes**

Diferentes filtros producen distintos mapas.

Por ello, una capa convolucional suele utilizar **múltiples filtros simultáneamente**.

---

# 11. Múltiples filtros

Supongamos una capa:

```python
layers.Conv2D(
    32,
    kernel_size=(3, 3),
    activation="relu"
)
```

El número:

**32**

indica que la capa aprende:

**32 filtros diferentes**

Por tanto, produce:

**32 mapas de características**

Cada filtro puede aprender a responder ante patrones distintos.

Conceptualmente:

**Imagen**

↓

**Filtro 1 → patrón A**

**Filtro 2 → patrón B**

**Filtro 3 → patrón C**

...

**Filtro 32 → patrón diferente**

La siguiente capa recibirá todas estas representaciones.

---

# 12. ¿Qué pueden aprender las primeras capas?

Las primeras capas convolucionales suelen responder a patrones relativamente simples.

Por ejemplo:

* bordes;
* orientaciones;
* cambios de contraste;
* líneas;
* texturas básicas.

Conceptualmente:

**Píxeles → bordes**

Las capas siguientes reciben esos mapas y pueden combinar patrones.

Por ejemplo:

**bordes → formas**

Posteriormente:

**formas → partes de objetos**

Finalmente:

**partes → objetos**

Esta progresión constituye el aprendizaje jerárquico de representaciones estudiado durante la Semana 9.

---

# 13. Receptive field

Cada neurona de una capa convolucional observa solamente una región de la entrada.

Esta región recibe el nombre de:

**campo receptivo** o *receptive field*.

En las primeras capas el campo receptivo puede ser pequeño.

Pero a medida que avanzamos:

**cada representación depende indirectamente de regiones más grandes de la imagen.**

Esto permite que las primeras capas identifiquen detalles locales y las capas posteriores integren información más global.

---

# 14. Parámetros compartidos

Una característica fundamental de la convolución es que **el mismo filtro se utiliza en múltiples posiciones de la imagen**.

Esto se denomina:

**parameter sharing**

o compartición de parámetros.

Supongamos un filtro que aprende a detectar determinado borde.

El mismo filtro puede detectar ese patrón:

* arriba;
* abajo;
* izquierda;
* derecha.

No necesitamos un conjunto diferente de pesos para cada ubicación.

Esta propiedad reduce considerablemente la cantidad de parámetros respecto de una red completamente densa.

---

# 15. Ventaja de los parámetros compartidos

Consideremos una imagen grande.

Una red densa podría aprender:

**“este patrón en posición exacta 1”**

con parámetros distintos de:

**“este mismo patrón en posición exacta 2”**

Una CNN reutiliza el mismo filtro.

Conceptualmente:

**Mismo patrón + diferentes posiciones → mismo detector**

Esto resulta muy útil en imágenes porque un objeto puede aparecer en distintas ubicaciones.

Por ejemplo:

un gato continúa siendo gato aunque aparezca:

* en el centro;
* a la izquierda;
* en la parte inferior.

---

# 16. Stride

El **stride** indica cuántos píxeles se desplaza el filtro en cada paso.

Por ejemplo:

### Stride = 1

El filtro avanza un píxel.

### Stride = 2

Avanza dos píxeles.

Un stride mayor reduce más rápidamente las dimensiones espaciales del mapa resultante.

En Keras:

```python
layers.Conv2D(
    32,
    (3, 3),
    strides=1
)
```

Para nuestras primeras CNN normalmente utilizaremos:

**stride = 1**

salvo que exista una razón específica para modificarlo.

---

# 17. Padding

Cuando aplicamos un filtro, el tamaño espacial de la salida puede disminuir.

El **padding** permite agregar valores alrededor de los bordes de la imagen antes de aplicar la convolución.

Dos opciones habituales son:

### valid

No se agrega padding.

La salida disminuye.

### same

Se agrega padding de manera que, con stride 1, la salida mantenga aproximadamente las mismas dimensiones espaciales.

Ejemplo:

```python
layers.Conv2D(
    32,
    (3, 3),
    padding="same"
)
```

---

# 18. Convolución + activación

Después de la convolución normalmente aplicamos una función de activación.

Por ejemplo:

```python
layers.Conv2D(
    32,
    (3, 3),
    activation="relu"
)
```

ReLU transforma:

**valores negativos → 0**

y mantiene:

**valores positivos**

La combinación:

**Convolución + ReLU**

es extremadamente habitual en CNN.

Conceptualmente:

**buscar patrón**

↓

**respuesta del filtro**

↓

**aplicar no linealidad**

↓

**mapa de características**

---

# 19. ¿Qué es pooling?

Después de varias operaciones convolucionales podemos reducir el tamaño espacial de los mapas mediante:

**pooling**

Una operación ampliamente utilizada es:

**Max Pooling**

Por ejemplo:

```python
layers.MaxPooling2D((2, 2))
```

Esta operación analiza pequeñas regiones:

**2 × 2**

y conserva el valor máximo.

---

# 20. Ejemplo de Max Pooling

Supongamos:

|  1 |  4 |
| -: | -: |
|  3 |  2 |

Max Pooling selecciona:

**4**

Otro bloque:

|  8 |  5 |
| -: | -: |
|  2 |  1 |

produce:

**8**

Por tanto, una matriz:

**4 × 4**

podría reducirse a:

**2 × 2**

Esto disminuye progresivamente las dimensiones de la representación.

---

# 21. ¿Por qué utilizar pooling?

Pooling puede aportar varios beneficios.

### Reduce dimensiones

Disminuye la cantidad de información que deben procesar las capas posteriores.

### Reduce costo computacional

Representaciones más pequeñas requieren menos operaciones.

### Concentra características relevantes

Max Pooling conserva las respuestas más intensas de una región.

### Aporta cierta tolerancia espacial

Pequeños desplazamientos pueden producir representaciones similares.

Por tanto:

**Pooling → representación más compacta**

---

# 22. Convolución y pooling trabajando juntos

Una secuencia típica puede ser:

**Imagen 224 × 224 × 3**

↓

**Conv2D — 32 filtros**

↓

**224 × 224 × 32**

↓

**MaxPooling**

↓

**112 × 112 × 32**

↓

**Conv2D — 64 filtros**

↓

**112 × 112 × 64**

↓

**MaxPooling**

↓

**56 × 56 × 64**

A medida que disminuyen:

**alto y ancho**

podemos aumentar:

**cantidad de filtros**

Esto permite construir representaciones más profundas y abstractas.

---

# 23. Una arquitectura CNN básica

Una CNN sencilla podría ser:

```python
from tensorflow import keras
from tensorflow.keras import layers

modelo = keras.Sequential([
    layers.Input(shape=(224, 224, 3)),

    layers.Conv2D(32, (3, 3), activation="relu"),
    layers.MaxPooling2D((2, 2)),

    layers.Conv2D(64, (3, 3), activation="relu"),
    layers.MaxPooling2D((2, 2)),

    layers.Conv2D(128, (3, 3), activation="relu"),
    layers.MaxPooling2D((2, 2))
])
```

Hasta este punto tenemos principalmente:

**extractor de características**

Todavía falta incorporar el clasificador.

---

# 24. De mapas de características a clasificación

Después de las capas convolucionales obtenemos una representación tridimensional.

Por ejemplo:

```text
26 × 26 × 128
```

Necesitamos convertir esta representación en una forma que pueda utilizar la parte final del clasificador.

Una alternativa tradicional es:

**Flatten**

Por ejemplo:

```python
layers.Flatten()
```

Esto transforma:

**estructura multidimensional**

en:

**vector**

Posteriormente podemos utilizar capas densas.

---

# 25. Flatten

Supongamos:

**10 × 10 × 64**

Esto contiene:

**6.400 valores**

Flatten transforma:

```text
10 × 10 × 64
```

en:

```text
6400
```

Después:

```python
layers.Dense(128, activation="relu")
```

puede procesar esa representación.

Finalmente:

```python
layers.Dense(4, activation="softmax")
```

produce las probabilidades de las cuatro clases.

---

# 26. CNN completa sencilla

Podemos unir los componentes:

```python
modelo = keras.Sequential([
    layers.Input(shape=(224, 224, 3)),

    layers.Conv2D(32, (3, 3), activation="relu"),
    layers.MaxPooling2D((2, 2)),

    layers.Conv2D(64, (3, 3), activation="relu"),
    layers.MaxPooling2D((2, 2)),

    layers.Conv2D(128, (3, 3), activation="relu"),
    layers.MaxPooling2D((2, 2)),

    layers.Flatten(),

    layers.Dense(128, activation="relu"),
    layers.Dense(4, activation="softmax")
])
```

Conceptualmente:

**Imagen**

↓

**Características simples**

↓

**Características más complejas**

↓

**Representación final**

↓

**Clasificación**

---

# 27. Inspeccionar la arquitectura

Podemos utilizar:

```python
modelo.summary()
```

El resumen permite observar:

* capas;
* dimensiones de salida;
* parámetros;
* parámetros entrenables.

El estudiante debería ser capaz de identificar:

**¿cuántas capas convolucionales existen?**

**¿cuántos filtros posee cada una?**

**¿dónde se reduce el tamaño espacial?**

**¿cuántas clases tiene la salida?**

No basta con ejecutar el código. Debemos interpretar la arquitectura.

---

# 28. Compilar la CNN

Después de definir la arquitectura debemos configurar su entrenamiento.

Por ejemplo:

```python
modelo.compile(
    optimizer="adam",
    loss="sparse_categorical_crossentropy",
    metrics=["accuracy"]
)
```

Los conceptos son los mismos estudiados anteriormente.

### Adam

Actualiza los parámetros.

### Loss

Mide el error de clasificación.

### Accuracy

Permite observar la proporción de predicciones correctas.

Las convoluciones no modifican la lógica general del aprendizaje.

---

# 29. Entrenar la CNN

Podemos utilizar:

```python
historial = modelo.fit(
    train_ds,
    validation_data=validation_ds,
    epochs=20
)
```

Internamente ocurre:

**Imagen**

↓

**Convoluciones**

↓

**Pooling**

↓

**Clasificación**

↓

**Predicción**

↓

**Loss**

↓

**Backpropagation**

↓

**Actualización de filtros y demás pesos**

Por tanto:

**los filtros también se entrenan.**

---

# 30. ¿Qué aprende realmente la CNN?

No programamos reglas como:

```text
si hay cuatro patas → perro
si hay bigotes → gato
```

Tampoco indicamos manualmente:

```text
Filtro 1 debe detectar orejas
Filtro 2 debe detectar ojos
```

Entregamos:

**imágenes + etiquetas**

y mediante el entrenamiento la red ajusta los filtros que resultan útiles para disminuir la pérdida.

El aprendizaje es:

**data-driven**

o dirigido por los datos.

Por esta razón, la calidad del dataset construido durante UA2 resulta fundamental.

---

# 31. Visualizar mapas de características

Es posible inspeccionar las activaciones internas de una CNN.

Por ejemplo:

**Imagen original**

↓

**Primera capa convolucional**

↓

**32 feature maps**

Algunos pueden responder a:

* bordes;
* contraste;
* texturas.

Las capas profundas serán más difíciles de interpretar directamente porque representan combinaciones más abstractas.

Esta visualización ayuda a comprender que la red transforma progresivamente la información.

---

# 32. Clasificación binaria versus multiclase

La capa final depende del problema.

### Clasificación binaria

Ejemplo:

**perro / gato**

podría utilizar:

```python
layers.Dense(1, activation="sigmoid")
```

### Clasificación multiclase

Ejemplo:

**plástico / vidrio / cartón / metal**

utiliza típicamente:

```python
layers.Dense(4, activation="softmax")
```

El número de neuronas de salida debe corresponder con el problema que estamos resolviendo.

---

# 33. Número de clases y salida

Supongamos tres proyectos.

### Proyecto A

2 especies.

Salida:

**2 clases**

### Proyecto B

5 categorías.

Salida:

**5 clases**

### Proyecto C

12 tipos de objetos.

Salida:

**12 clases**

La arquitectura no puede copiarse mecánicamente sin revisar el problema.

El estudiante deberá adaptar especialmente:

* entrada;
* número de clases;
* salida;
* estrategia de entrenamiento.

---

# 34. CNN y overfitting

Una CNN puede poseer una gran capacidad.

Con pocos datos puede aparecer rápidamente:

**Train accuracy → 99%**

**Validation accuracy → 70%**

Por ello siguen siendo importantes las estrategias estudiadas durante UA2:

* data augmentation;
* dropout;
* early stopping;
* diversidad del dataset;
* validación adecuada.

Construir una CNN no elimina el problema de generalización.

---

# 35. Dropout en una CNN

Podemos incorporar:

```python
layers.Dropout(0.5)
```

por ejemplo después de una capa densa.

Una arquitectura podría incluir:

```python
layers.Flatten(),
layers.Dense(128, activation="relu"),
layers.Dropout(0.5),
layers.Dense(4, activation="softmax")
```

Durante entrenamiento, Dropout desactiva temporalmente parte de las unidades.

Su objetivo es reducir dependencia excesiva entre determinadas activaciones y contribuir a combatir el sobreajuste.

---

# 36. Data augmentation + CNN

Podemos incorporar el augmentation directamente antes de las convoluciones.

Por ejemplo:

```python
data_augmentation = keras.Sequential([
    layers.RandomFlip("horizontal"),
    layers.RandomRotation(0.1),
    layers.RandomZoom(0.1)
])
```

Arquitectura conceptual:

**Imagen**

↓

**Data augmentation**

↓

**Convolución**

↓

**Pooling**

↓

**...**

↓

**Clasificación**

Esta combinación será especialmente relevante para el proyecto integrador.

---

# 37. Normalización + CNN

También podemos incorporar:

```python
layers.Rescaling(1./255)
```

antes de las capas convolucionales.

Ejemplo:

```python
modelo = keras.Sequential([
    layers.Input(shape=(224, 224, 3)),
    layers.Rescaling(1./255),

    layers.Conv2D(32, 3, activation="relu"),
    layers.MaxPooling2D(),

    ...
])
```

Así el pipeline mantiene una transformación consistente de los valores de entrada.

---

# 38. Una primera CNN completa para el proyecto

Una arquitectura inicial podría ser:

```python
modelo = keras.Sequential([
    layers.Input(shape=(224, 224, 3)),

    layers.Rescaling(1./255),

    layers.RandomFlip("horizontal"),
    layers.RandomRotation(0.1),

    layers.Conv2D(32, 3, activation="relu"),
    layers.MaxPooling2D(),

    layers.Conv2D(64, 3, activation="relu"),
    layers.MaxPooling2D(),

    layers.Conv2D(128, 3, activation="relu"),
    layers.MaxPooling2D(),

    layers.Flatten(),

    layers.Dense(128, activation="relu"),
    layers.Dropout(0.5),

    layers.Dense(4, activation="softmax")
])
```

Esta arquitectura no debe interpretarse como:

**“la arquitectura correcta para todos los proyectos”**

sino como:

**un punto de partida para experimentar.**

---

# 39. Diseñar antes de copiar

Cada dupla debería poder justificar preguntas como:

**¿Cuál es el tamaño de entrada?**

**¿Cuántas clases existen?**

**¿Cuántas capas convolucionales utilizaremos?**

**¿Cuántos filtros tendrá cada capa?**

**¿Qué tamaño de kernel utilizaremos?**

**¿Dónde utilizaremos pooling?**

**¿Aplicaremos dropout?**

**¿Qué augmentation resulta válido?**

---

# 40. Profundidad progresiva

Una estrategia común consiste en aumentar el número de filtros a medida que disminuyen las dimensiones espaciales.

Por ejemplo:

```text
224 × 224 × 3
↓
Conv 32
↓
Pooling
↓
Conv 64
↓
Pooling
↓
Conv 128
↓
Pooling
```

Conceptualmente:

**espacio grande + pocas características**

↓

**espacio más pequeño + más representaciones**

Esto permite que capas posteriores aprendan combinaciones más complejas.

---

# 41. Global Average Pooling

Además de Flatten existe otra alternativa:

**Global Average Pooling**

Por ejemplo:

```python
layers.GlobalAveragePooling2D()
```

En lugar de convertir todos los valores en un gran vector, calcula una representación global de cada mapa de características.

Puede reducir considerablemente la cantidad de parámetros respecto de Flatten.

Esta técnica será especialmente frecuente cuando posteriormente utilicemos modelos preentrenados y *transfer learning*.

---

# 42. Flatten versus Global Average Pooling

Supongamos una salida convolucional:

```text
7 × 7 × 512
```

Con Flatten:

**7 × 7 × 512 = 25.088 valores**

Con Global Average Pooling:

**512 valores**

Esto puede reducir:

* parámetros;
* memoria;
* riesgo de overfitting.

No existe una regla que establezca que siempre debemos utilizar una u otra.

La decisión forma parte del diseño del modelo.

---

# 43. Matriz de confusión

Accuracy entrega una medida global, pero en clasificación multiclase conviene observar también **qué clases se confunden entre sí**.

Por ejemplo:

| Real / Predicha | Plástico | Vidrio | Cartón | Metal |
| --------------- | -------: | -----: | -----: | ----: |
| Plástico        |       90 |      2 |      6 |     2 |
| Vidrio          |        3 |     85 |      2 |    10 |
| Cartón          |        5 |      1 |     92 |     2 |
| Metal           |        1 |     11 |      2 |    86 |

Podemos observar que:

**vidrio y metal**

se confunden con mayor frecuencia.

Esto puede orientar el análisis del modelo y del dataset.

---

# 44. Accuracy puede ocultar problemas

Supongamos:

```text
Clase A: 95%
Clase B: 94%
Clase C: 92%
Clase D: 51%
```

El accuracy global podría parecer razonable.

Sin embargo, la clase D presenta problemas importantes.

Por ello, en el análisis posterior podremos considerar:

* accuracy;
* matriz de confusión;
* precision;
* recall;
* F1-score.

En esta sesión interesa instalar la idea de que **la evaluación debe observar también el comportamiento por clase**.

---

# 45. Predicción sobre una imagen nueva

Una vez entrenado podemos realizar inferencia.

Conceptualmente:

```python
prediccion = modelo.predict(imagen)
```

Podríamos obtener:

```text
[0.03, 0.88, 0.05, 0.04]
```

Si las clases son:

```text
0 → plástico
1 → vidrio
2 → cartón
3 → metal
```

la predicción será:

**Vidrio — 88%**

Este flujo es exactamente el que utilizará la aplicación final del proyecto integrador.

---

# 46. Confianza versus certeza

Cuando Softmax produce:

**Vidrio → 88%**

conviene evitar interpretar automáticamente:

> “Existe un 88% de certeza absoluta de que es vidrio.”

El valor corresponde a la salida probabilística del modelo según su representación y entrenamiento.

Un modelo puede estar:

**muy confiado y equivocado.**

Por ello, el nivel de confianza debe interpretarse junto con:

* evaluación del modelo;
* calidad de datos;
* contexto del problema.

---

# 47. Errores como fuente de información

Las imágenes incorrectamente clasificadas son especialmente útiles.

Después de entrenar deberíamos revisar:

**¿Qué imágenes falla?**

**¿Qué clases confunde?**

**¿Existen patrones comunes en los errores?**

Por ejemplo:

* fotografías oscuras;
* objetos parcialmente visibles;
* determinadas posiciones;
* clases visualmente similares.

El análisis de errores puede indicar si debemos modificar:

* datos;
* augmentation;
* arquitectura;
* entrenamiento.

---

# 48. Iteración del modelo

El desarrollo de una CNN debería seguir un ciclo:

**CNN inicial**

↓

**Entrenar**

↓

**Evaluar**

↓

**Examinar curvas**

↓

**Analizar matriz de confusión**

↓

**Inspeccionar errores**

↓

**Modificar**

↓

**Volver a entrenar**

Por tanto:

**la primera CNN rara vez debe considerarse automáticamente el modelo definitivo.**

---

# 49. Aplicación al proyecto integrador

A partir de esta semana comienza directamente la construcción del **núcleo técnico del proyecto integrador**.

Cada dupla utilizará:

**dataset preparado**

↓

**CNN**

↓

**entrenamiento**

↓

**validación**

↓

**ajuste**

↓

**modelo definitivo**

---

# 50. Ejemplo conductor: CNN para clasificación de residuos

Entrada:

```text
224 × 224 × 3
```

Clases:

```text
plástico
vidrio
cartón
metal
```

Arquitectura inicial:

```text
Imagen
↓
Rescaling
↓
Data augmentation
↓
Conv 32 + ReLU
↓
Pooling
↓
Conv 64 + ReLU
↓
Pooling
↓
Conv 128 + ReLU
↓
Pooling
↓
Global Average Pooling
↓
Dense
↓
Softmax — 4 clases
```

Entrenamos y obtenemos, por ejemplo:

```text
Train accuracy: 91%
Validation accuracy: 87%
Test accuracy: 86%
```

Después analizamos:

* curvas;
* errores;
* clases problemáticas;
* posibles mejoras.

Este modelo será una primera versión antes de experimentar con alternativas más avanzadas.

---

# 51. Preguntas para discusión en clase

### Caso 1

Una imagen tiene tamaño:

**224 × 224 × 3**

y una red pretende conectarla directamente con 2.000 neuronas densas.

**Pregunta:** ¿Qué problema de cantidad de parámetros podría aparecer y qué ventaja ofrece una CNN?

### Caso 2

Una capa utiliza:

```python
Conv2D(64, (3,3))
```

**Pregunta:** ¿Qué representa el número 64?

### Caso 3

Un filtro aprende a detectar cierto borde.

**Pregunta:** ¿Debe existir un filtro diferente para detectar ese mismo borde en cada posición de la imagen?

### Caso 4

Después de Max Pooling:

```text
112 × 112 × 32
```

se convierte aproximadamente en:

```text
56 × 56 × 32
```

**Pregunta:** ¿Qué dimensión está reduciendo principalmente el pooling?

### Caso 5

Una CNN obtiene:

```text
Train = 99%
Validation = 68%
```

**Pregunta:** ¿Qué fenómeno parece estar ocurriendo y qué estrategias estudiadas anteriormente podrían utilizarse?

### Caso 6

Una dupla utiliza cuatro clases pero su capa final posee diez neuronas con Softmax.

**Pregunta:** ¿Qué inconsistencia existe?

### Caso 7

El modelo tiene buen accuracy global, pero clasifica correctamente solo la mitad de las imágenes de una categoría.

**Pregunta:** ¿Qué herramienta permitiría observar mejor este problema?

---

# 52. Síntesis de la Semana 10

Al finalizar esta sesión deben quedar instaladas diez ideas fundamentales:

1. **Las CNN están especialmente diseñadas para aprovechar la estructura espacial de las imágenes.**
2. **La convolución aplica filtros locales sobre distintas regiones de la entrada.**
3. **Los filtros son parámetros entrenables y la CNN aprende automáticamente qué patrones resultan útiles.**
4. **Cada filtro produce un mapa de características que representa su respuesta ante determinados patrones.**
5. **Las capas iniciales pueden aprender características simples y las posteriores combinar estas representaciones en patrones más complejos.**
6. **Pooling reduce las dimensiones espaciales de los mapas y ayuda a construir representaciones más compactas.**
7. **Una CNN combina una etapa de extracción de características con una etapa final de clasificación.**
8. **El número de clases debe reflejarse correctamente en la capa de salida.**
9. **El modelo debe evaluarse no solo mediante accuracy global, sino también analizando su comportamiento por clase y sus errores.**
10. **La CNN constituye el núcleo técnico del proyecto integrador y deberá ser diseñada, entrenada, evaluada y posteriormente almacenada como modelo definitivo.**

### Hacia la Semana 11

Hasta ahora, dentro de la Unidad 3, hemos avanzado desde:

**Semana 9:** ¿Qué es Deep Learning y cómo se estructura una red profunda?

hacia:

**Semana 10:** ¿Cómo puede una CNN aprender características visuales y clasificar imágenes?

La siguiente sesión ampliará el panorama hacia otro tipo de información:

**¿Qué ocurre cuando los datos poseen un orden temporal o secuencial y el contexto anterior influye en el siguiente elemento?**

La **Semana 11** abordará **redes para datos secuenciales, particularmente LSTM, y su aplicación en texto y secuencias**, manteniendo la CNN como arquitectura central del proyecto integrador.

