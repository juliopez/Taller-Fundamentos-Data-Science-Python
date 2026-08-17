# Semana 13 — Transfer Learning y Fine-Tuning

## 1. Propósito de la sesión

Comprender cómo reutilizar modelos de Deep Learning previamente entrenados para resolver nuevos problemas de clasificación de imágenes, aplicando los conceptos de **Transfer Learning** y **Fine-Tuning** como estrategias para reducir el tiempo de entrenamiento, aprovechar representaciones visuales ya aprendidas y mejorar el desempeño cuando se dispone de conjuntos de datos limitados.

Al finalizar la sesión, el estudiante debería ser capaz de:

* Explicar qué es Transfer Learning.
* Comprender por qué un modelo entrenado sobre un gran dataset puede reutilizarse en otro problema.
* Diferenciar entrenamiento desde cero y transferencia de aprendizaje.
* Identificar las capas de extracción de características y clasificación.
* Comprender qué significa congelar capas.
* Explicar qué es Fine-Tuning.
* Diferenciar Transfer Learning con capas congeladas de Fine-Tuning.
* Identificar cuándo estas estrategias resultan apropiadas.
* Utilizar conceptualmente un modelo preentrenado mediante Keras.
* Relacionar estas estrategias con la selección del modelo definitivo del proyecto integrador.

---

# 2. ¿Es necesario entrenar siempre desde cero?

Durante la Semana 10 construimos una CNN conceptualmente desde cero.

El flujo era:

**Inicializar pesos**

↓

**Entrenar con nuestro dataset**

↓

**Aprender filtros**

↓

**Construir clasificador**

↓

**Obtener modelo**

Esta estrategia puede funcionar correctamente cuando disponemos de:

* suficientes datos;
* tiempo de entrenamiento;
* capacidad computacional;
* un problema suficientemente específico.

Sin embargo, surge una pregunta:

**¿Qué ocurre si solamente disponemos de algunas centenas o miles de imágenes?**

Podríamos intentar entrenar desde cero, pero existe mayor riesgo de:

* overfitting;
* entrenamiento lento;
* características visuales poco robustas.

Una alternativa consiste en reutilizar conocimiento previamente aprendido.

---

# 3. La idea de Transfer Learning

**Transfer Learning** o aprendizaje por transferencia consiste en aprovechar un modelo previamente entrenado en un problema y reutilizar parte de ese conocimiento para resolver otro problema relacionado.

Conceptualmente:

**Modelo entrenado previamente**

↓

**Características visuales aprendidas**

↓

**Nuevo problema**

↓

**Nuevo clasificador**

Por ejemplo:

Un modelo entrenado utilizando millones de imágenes puede haber aprendido a reconocer patrones generales como:

* bordes;
* texturas;
* formas;
* combinaciones visuales.

Podemos reutilizar estas representaciones en nuestro propio problema de clasificación.

---

# 4. ¿Qué se transfiere?

No transferimos literalmente:

**“las respuestas del modelo anterior”.**

Transferimos principalmente:

**los pesos aprendidos por sus capas internas.**

Recordemos:

**Pesos iniciales aleatorios**

↓

**Entrenamiento**

↓

**Pesos capaces de detectar patrones**

En Transfer Learning comenzamos desde:

**pesos ya entrenados**

en lugar de:

**pesos aleatorios**

Por tanto, el nuevo modelo no parte completamente desde cero.

---

# 5. ¿Por qué puede funcionar?

Las primeras capas de una CNN suelen aprender características relativamente generales.

Por ejemplo:

**bordes**

↓

**texturas**

↓

**formas**

↓

**estructuras más complejas**

Muchas de estas características pueden ser útiles para distintos problemas visuales.

Un modelo que aprendió a reconocer miles de objetos ya desarrolló filtros capaces de identificar patrones visuales generales.

Podemos reutilizarlos para problemas nuevos como:

* especies de plantas;
* residuos;
* productos;
* tipos de vehículos;
* alimentos.

---

# 6. ImageNet como fuente de modelos preentrenados

Muchos modelos utilizados en Transfer Learning han sido entrenados previamente utilizando grandes conjuntos de imágenes.

Uno de los referentes más conocidos es:

**ImageNet**

que contiene una gran cantidad de imágenes organizadas en múltiples categorías.

Modelos entrenados sobre este tipo de dataset aprendieron representaciones visuales suficientemente generales para ser reutilizadas posteriormente.

La idea es:

**Gran dataset → entrenamiento costoso realizado previamente**

↓

**Modelo preentrenado**

↓

**Reutilización en nuestro proyecto**

---

# 7. Modelos preentrenados

Existen diferentes arquitecturas ampliamente utilizadas como base para Transfer Learning.

Entre ellas:

* VGG;
* ResNet;
* MobileNet;
* EfficientNet;
* Inception.

Cada arquitectura presenta diferencias en:

* profundidad;
* cantidad de parámetros;
* velocidad;
* memoria;
* precisión;
* tamaño del modelo.

Por tanto, la elección del modelo base también debe responder al objetivo del proyecto.

---

# 8. Arquitectura general de una CNN preentrenada

Podemos dividir conceptualmente una CNN en:

### Base convolucional

Aprende y extrae características visuales.

### Clasificador

Transforma esas características en las clases específicas del problema original.

Conceptualmente:

**Imagen**

↓

**Base convolucional**

↓

**Características**

↓

**Clasificador original**

↓

**Clases originales**

En Transfer Learning normalmente conservamos:

**base convolucional**

pero reemplazamos:

**clasificador original**

por uno adaptado a nuestro problema.

---

# 9. Reemplazar el clasificador

Supongamos que un modelo original clasificaba:

**1.000 categorías**

Su última capa podría contener:

**1.000 neuronas**

Pero nuestro proyecto contiene:

**4 clases**

Por tanto, no necesitamos esa salida original.

Podemos hacer:

**Modelo preentrenado**

↓

**Eliminar clasificador original**

↓

**Agregar nuevo clasificador**

↓

**4 clases**

Por ejemplo:

**plástico / vidrio / cartón / metal**

---

# 10. Feature Extractor

Una estrategia básica consiste en utilizar el modelo preentrenado como:

**extractor de características**

Conceptualmente:

**Imagen**

↓

**Modelo preentrenado congelado**

↓

**Representación visual**

↓

**Nuevo clasificador entrenable**

↓

**Clase**

El modelo base no modifica inicialmente sus pesos.

Solo entrenamos las nuevas capas incorporadas al final.

Esta estrategia corresponde a una forma común de Transfer Learning.

---

# 11. ¿Qué significa congelar una capa?

Una capa congelada mantiene sus pesos sin modificarlos durante el nuevo entrenamiento.

Conceptualmente:

**Peso preentrenado**

↓

**NO actualizar**

En Keras:

```python
base_model.trainable = False
```

Esto indica que los parámetros del modelo base no serán actualizados durante esta etapa.

El entrenamiento modifica solamente las nuevas capas que agregamos.

---

# 12. ¿Por qué congelar las capas?

Congelar el modelo base permite:

* conservar características previamente aprendidas;
* reducir el número de parámetros entrenables;
* disminuir tiempo de entrenamiento;
* reducir requerimientos computacionales;
* disminuir riesgo de destruir conocimiento útil.

Supongamos:

**Modelo completo = 4 millones de parámetros**

pero solamente:

**100.000 parámetros pertenecen al nuevo clasificador.**

Si congelamos la base, entrenamos únicamente esos 100.000.

---

# 13. Flujo inicial de Transfer Learning

Una estrategia típica puede ser:

**1. Cargar modelo preentrenado**

↓

**2. Eliminar clasificación original**

↓

**3. Congelar modelo base**

↓

**4. Agregar nuevas capas**

↓

**5. Entrenar nuevo clasificador**

↓

**6. Evaluar**

Esta primera etapa permite adaptar rápidamente el modelo al nuevo problema.

---

# 14. Transfer Learning con Keras

Keras permite cargar modelos preentrenados.

Por ejemplo:

```python
from tensorflow.keras.applications import MobileNetV2

base_model = MobileNetV2(
    weights="imagenet",
    include_top=False,
    input_shape=(224, 224, 3)
)
```

Aquí:

**weights="imagenet"**

indica que utilizamos pesos previamente aprendidos.

**include_top=False**

elimina el clasificador original.

---

# 15. Congelar la base

Después:

```python
base_model.trainable = False
```

Esto conserva los pesos previamente aprendidos.

Podemos comprobar:

**parámetros entrenables → nuevo clasificador**

**parámetros no entrenables → base preentrenada**

Esta distinción puede observarse posteriormente mediante:

```python
modelo.summary()
```

---

# 16. Construir el nuevo clasificador

Podemos agregar:

```python
from tensorflow.keras import layers
from tensorflow import keras

modelo = keras.Sequential([
    layers.Input(shape=(224, 224, 3)),

    base_model,

    layers.GlobalAveragePooling2D(),

    layers.Dense(128, activation="relu"),

    layers.Dropout(0.3),

    layers.Dense(4, activation="softmax")
])
```

Conceptualmente:

**Imagen**

↓

**MobileNetV2**

↓

**Características aprendidas**

↓

**Global Average Pooling**

↓

**Clasificador propio**

↓

**4 clases**

---

# 17. ¿Por qué Global Average Pooling?

Durante la Semana 10 vimos dos alternativas:

**Flatten**

y:

**Global Average Pooling**

Los modelos preentrenados suelen producir múltiples mapas de características.

Global Average Pooling transforma cada mapa en un único valor promedio.

Por ejemplo:

**7 × 7 × 1280**

↓

**Global Average Pooling**

↓

**1280 valores**

Esto reduce considerablemente el número de parámetros antes de la clasificación.

---

# 18. Preprocesamiento del modelo

Un aspecto importante es que los modelos preentrenados pueden esperar un **preprocesamiento específico**.

Por ejemplo, determinadas arquitecturas pueden requerir:

* escalas concretas;
* tamaño de imagen específico;
* transformación particular de valores.

Keras ofrece funciones asociadas a cada arquitectura.

Por ejemplo:

```python
from tensorflow.keras.applications.mobilenet_v2 import preprocess_input
```

Por tanto:

**no todos los modelos preentrenados deben recibir exactamente el mismo preprocesamiento.**

---

# 19. Entrenamiento inicial

Después de construir el modelo podemos compilar:

```python
modelo.compile(
    optimizer="adam",
    loss="sparse_categorical_crossentropy",
    metrics=["accuracy"]
)
```

y entrenar:

```python
historial = modelo.fit(
    train_ds,
    validation_data=validation_ds,
    epochs=10
)
```

Durante esta etapa:

**base preentrenada → congelada**

**nuevo clasificador → aprende**

---

# 20. ¿Qué está aprendiendo el nuevo clasificador?

El modelo base convierte cada imagen en una representación interna.

Por ejemplo:

**fotografía**

↓

**modelo preentrenado**

↓

**vector de características**

El nuevo clasificador aprende:

**características → nuestras clases**

Por tanto, no necesita descubrir nuevamente desde cero patrones como:

* bordes;
* texturas;
* formas básicas.

Parte de ese conocimiento ya existe.

---

# 21. Transfer Learning versus entrenamiento desde cero

Podemos comparar:

| Aspecto                   | Desde cero       | Transfer Learning         |
| ------------------------- | ---------------- | ------------------------- |
| Pesos iniciales           | Aleatorios       | Preentrenados             |
| Datos necesarios          | Normalmente más  | Puede funcionar con menos |
| Tiempo                    | Mayor            | Generalmente menor        |
| Características iniciales | Deben aprenderse | Ya existen                |
| Riesgo con pocos datos    | Mayor            | Puede reducirse           |
| Flexibilidad              | Total            | Depende del modelo base   |

Ninguna estrategia es siempre superior.

La elección depende del problema.

---

# 22. ¿Cuándo puede ser útil Transfer Learning?

Especialmente cuando:

* disponemos de pocos datos;
* el problema está relacionado con imágenes naturales;
* necesitamos reducir tiempo de entrenamiento;
* contamos con recursos limitados;
* existe un modelo base adecuado.

Por ejemplo:

**1.500 imágenes de cuatro especies**

podrían resultar insuficientes para entrenar una CNN profunda desde cero.

Pero pueden ser suficientes para adaptar un modelo preentrenado.

---

# 23. ¿Cuándo podría ser menos útil?

Puede ser menos apropiado cuando:

* las imágenes son extremadamente diferentes del dominio original;
* disponemos de grandes cantidades de datos propios;
* necesitamos una arquitectura muy específica;
* tenemos restricciones incompatibles con el modelo seleccionado.

Por ejemplo, imágenes científicas muy particulares podrían requerir mayor adaptación.

Esto no significa que Transfer Learning no funcione, sino que debe evaluarse experimentalmente.

---

# 24. ¿Qué es Fine-Tuning?

Después de entrenar el nuevo clasificador podemos dar un paso adicional:

**Fine-Tuning**

o ajuste fino.

Consiste en descongelar parte del modelo preentrenado y continuar entrenando con nuestro dataset.

Conceptualmente:

### Transfer Learning inicial

**Base congelada**

*

**Clasificador entrenable**

### Fine-Tuning

**Parte de la base se descongela**

*

**Clasificador continúa entrenándose**

Esto permite adaptar determinadas representaciones internas al nuevo problema.

---

# 25. Diferencia fundamental

Podemos resumir:

### Transfer Learning

Reutilizamos conocimiento previo.

Normalmente:

**modelo base congelado**

### Fine-Tuning

Ajustamos parte de ese conocimiento.

Normalmente:

**algunas capas preentrenadas vuelven a ser entrenables**

Por tanto:

**Transfer Learning → reutilizar**

**Fine-Tuning → reutilizar + adaptar**

---

# 26. ¿Por qué no descongelar todo inmediatamente?

Si modificamos todos los pesos desde el comienzo, especialmente con pocos datos, podemos destruir rápidamente características útiles aprendidas previamente.

Esto recibe a veces el nombre de:

**catastrophic forgetting**

o pérdida del conocimiento aprendido.

Por ello una estrategia habitual es:

**1. Congelar base**

↓

**2. Entrenar clasificador**

↓

**3. Descongelar últimas capas**

↓

**4. Entrenar con learning rate pequeño**

---

# 27. Descongelar capas

Podemos hacer:

```python
base_model.trainable = True
```

pero después congelar las primeras capas:

```python
for layer in base_model.layers[:-30]:
    layer.trainable = False
```

Esto significa:

**primeras capas → congeladas**

**últimas 30 capas → entrenables**

La idea es conservar representaciones generales y adaptar principalmente las capas más especializadas.

---

# 28. ¿Por qué las últimas capas?

Recordemos el aprendizaje jerárquico.

Las primeras capas suelen aprender:

* bordes;
* texturas;
* patrones generales.

Las capas posteriores pueden aprender:

* combinaciones más específicas;
* estructuras complejas;
* representaciones relacionadas con las clases originales.

Por ello, las últimas capas suelen ser las primeras candidatas para *fine-tuning*.

---

# 29. Learning rate en Fine-Tuning

Durante Fine-Tuning suele utilizarse una tasa de aprendizaje menor.

Por ejemplo:

```python
modelo.compile(
    optimizer=keras.optimizers.Adam(
        learning_rate=0.00001
    ),
    loss="sparse_categorical_crossentropy",
    metrics=["accuracy"]
)
```

La razón es:

**no queremos cambiar bruscamente pesos previamente útiles.**

Buscamos:

**pequeños ajustes**

para adaptar el modelo.

---

# 30. Estrategia completa

Podemos representar:

### Etapa 1

**Cargar modelo preentrenado**

↓

**Congelar base**

↓

**Entrenar clasificador**

↓

**Evaluar**

### Etapa 2

**Descongelar algunas capas**

↓

**Reducir learning rate**

↓

**Continuar entrenamiento**

↓

**Evaluar nuevamente**

Este proceso permite comparar:

**antes del Fine-Tuning**

y:

**después del Fine-Tuning**

---

# 31. Ejemplo de resultados

Supongamos:

### CNN desde cero

```text
Train accuracy: 93%
Validation accuracy: 78%
Test accuracy: 77%
```

### Transfer Learning

```text
Train accuracy: 91%
Validation accuracy: 87%
Test accuracy: 86%
```

### Fine-Tuning

```text
Train accuracy: 93%
Validation accuracy: 90%
Test accuracy: 89%
```

Estos resultados serían consistentes con una mejora de generalización.

Sin embargo, no debemos asumir que siempre ocurrirá.

Debe comprobarse experimentalmente.

---

# 32. Fine-Tuning también puede empeorar

Supongamos:

**Antes: validation = 88%**

Después de Fine-Tuning:

**validation = 82%**

Puede haber ocurrido:

* learning rate demasiado alto;
* demasiadas capas descongeladas;
* sobreajuste;
* entrenamiento excesivo.

Por tanto:

**Fine-Tuning no significa automáticamente mejorar.**

Es un experimento que debe ser evaluado.

---

# 33. Modelos preentrenados disponibles

Keras ofrece múltiples arquitecturas.

Por ejemplo:

```python
from tensorflow.keras.applications import (
    MobileNetV2,
    ResNet50,
    EfficientNetB0
)
```

Cada una presenta diferentes compromisos.

### ResNet

Arquitectura profunda con conexiones residuales.

### MobileNet

Diseñada para ser relativamente eficiente.

### EfficientNet

Busca equilibrar tamaño y rendimiento.

La selección del modelo puede anticipar decisiones que serán importantes durante la implementación.

---

# 34. ResNet y conexiones residuales

Cuando las redes se hicieron más profundas apareció la dificultad de entrenarlas eficazmente.

ResNet introdujo las denominadas:

**conexiones residuales**

o *skip connections*.

Conceptualmente:

```text
Entrada
│
├───────────────┐
↓               │
Capas           │
↓               │
Transformación  │
│               │
└──── suma ←────┘
```

Estas conexiones facilitan el flujo de información y gradientes.

Esto permitió entrenar redes considerablemente profundas.

---

# 35. MobileNet

MobileNet fue diseñada poniendo especial atención en eficiencia.

Puede resultar útil cuando posteriormente necesitamos ejecutar el modelo en:

* dispositivos móviles;
* hardware limitado;
* Edge Computing.

Por tanto, la decisión tomada durante la Unidad 3 puede tener consecuencias en la Unidad 4.

Un modelo más liviano puede facilitar:

**implementación y optimización.**

---

# 36. EfficientNet

EfficientNet utiliza una estrategia que busca equilibrar:

* profundidad;
* ancho;
* resolución.

Su objetivo general es obtener buen desempeño utilizando recursos de manera eficiente.

No es necesario estudiar toda su arquitectura interna en esta asignatura.

Lo relevante es comprender que:

**los modelos preentrenados poseen diferentes características y costos.**

---

# 37. ¿Cuál modelo seleccionar?

La respuesta no debería ser:

**“el que tenga mayor accuracy en Internet”.**

Debemos considerar:

* tamaño del dataset;
* tipo de imágenes;
* precisión obtenida;
* velocidad;
* tamaño del modelo;
* recursos disponibles;
* objetivo posterior de implementación.

Por ejemplo:

**modelo A → 94% pero 500 MB**

**modelo B → 92% pero 20 MB**

Dependiendo del contexto:

**B podría ser más apropiado.**

---

# 38. Dataset y similitud de dominio

La transferencia funciona mejor cuando existe cierto grado de similitud entre:

**dominio original**

y:

**nuevo dominio**

Por ejemplo:

Modelo entrenado con fotografías naturales.

Nuevo problema:

**clasificación de frutas**

Existe bastante similitud visual.

En cambio:

Modelo entrenado con fotografías naturales.

Nuevo problema:

**imágenes microscópicas muy especializadas**

La distancia entre dominios puede ser mayor.

Es posible que sea necesario realizar un Fine-Tuning más profundo.

---

# 39. Data Augmentation sigue siendo relevante

Utilizar un modelo preentrenado no elimina la necesidad de:

* datos de calidad;
* augmentation;
* validación;
* control de overfitting.

Podemos utilizar:

```python
data_augmentation = keras.Sequential([
    layers.RandomFlip("horizontal"),
    layers.RandomRotation(0.1),
    layers.RandomZoom(0.1)
])
```

antes de la base preentrenada.

Por tanto:

**Transfer Learning complementa, no reemplaza, la preparación correcta de los datos.**

---

# 40. Early Stopping

También podemos utilizar:

```python
callback = keras.callbacks.EarlyStopping(
    monitor="val_loss",
    patience=3,
    restore_best_weights=True
)
```

Esto permite detener entrenamiento cuando la validación deja de mejorar.

Especialmente durante Fine-Tuning puede ser útil para evitar sobreajuste.

---

# 41. Checkpoint del mejor modelo

Otra estrategia es guardar automáticamente el mejor modelo.

Por ejemplo:

```python
checkpoint = keras.callbacks.ModelCheckpoint(
    "mejor_modelo.keras",
    monitor="val_accuracy",
    save_best_only=True
)
```

Esto permite conservar la configuración que obtuvo el mejor resultado durante entrenamiento.

El modelo seleccionado será posteriormente utilizado en la fase de implementación.

---

# 42. Comparar modelos

Por ejemplo:

| Modelo                 | Val Accuracy | Test Accuracy | Parámetros | Tamaño |
| ---------------------- | -----------: | ------------: | ---------: | -----: |
| CNN propia             |          82% |           80% |      2,3 M |  12 MB |
| MobileNetV2            |          89% |           88% |      2,4 M |  14 MB |
| MobileNetV2 Fine-Tuned |          91% |           90% |      2,4 M |  14 MB |

Esto permite justificar técnicamente la selección final.

No basta con afirmar:

**“utilizamos MobileNet porque es mejor”.**

La decisión debe respaldarse con evidencia.

---

# 43. Accuracy no es el único criterio

Podemos comparar:

* accuracy;
* precision;
* recall;
* F1;
* matriz de confusión;
* tamaño;
* velocidad;
* parámetros.

Por ejemplo:

Un modelo puede tener excelente accuracy global pero fallar sistemáticamente en una clase.

Por ello, debemos revisar:

**¿qué errores comete?**

no solamente:

**¿cuánto acierta?**

---

# 44. Matriz de confusión antes y después del Fine-Tuning

Supongamos que antes:

**vidrio ↔ metal**

se confunden frecuentemente.

Después de Fine-Tuning observamos una reducción de esa confusión.

Esto proporciona una evidencia más rica que reportar solamente:

**accuracy +2%.**

La matriz permite identificar **qué aspecto del comportamiento mejoró**.

---

# 45. Curvas de entrenamiento

También debemos revisar:

**training loss**

**validation loss**

**training accuracy**

**validation accuracy**

Durante Fine-Tuning podríamos observar:

**train sigue aumentando**

pero:

**validation comienza a caer.**

Eso indica que debemos controlar nuevamente el overfitting.

Las técnicas estudiadas en UA2 continúan siendo relevantes.

---

# 46. Transfer Learning y eficiencia

Transfer Learning también representa una estrategia de eficiencia computacional.

Entrenar grandes modelos desde cero podría requerir:

* semanas;
* múltiples GPU;
* grandes cantidades de datos.

Reutilizar modelos entrenados previamente permite aprovechar ese esfuerzo.

Conceptualmente:

**costo previo enorme**

↓

**modelo disponible**

↓

**adaptación relativamente económica**

Esto explica su enorme importancia práctica.

---

# 47. Modelo base y propiedad intelectual

Cuando utilizamos modelos preentrenados debemos considerar también:

* fuente;
* licencia;
* condiciones de uso;
* documentación;
* versión.

No todo modelo disponible puede utilizarse libremente en cualquier contexto.

Dentro de un proyecto académico también resulta conveniente documentar:

**qué modelo se utilizó y de dónde proviene.**

---

# 48. Transfer Learning no elimina el conocimiento del problema

Utilizar:

```python
MobileNetV2(weights="imagenet")
```

no significa que el proyecto esté terminado.

Todavía debemos decidir:

* clases;
* dataset;
* preprocesamiento;
* augmentation;
* arquitectura del clasificador;
* capas a descongelar;
* learning rate;
* épocas;
* evaluación;
* selección final.

Por tanto:

**reutilizar un modelo no reemplaza el diseño técnico.**

---

# 49. Producto al finalizar la Unidad 3

Al cerrar la UA3, cada dupla debería disponer de:

**Dataset definitivo**

↓

**Arquitectura seleccionada**

↓

**CNN entrenada**

↓

**Experimentos documentados**

↓

**Control de overfitting**

↓

**Evaluación en test**

↓

**Matriz de confusión**

↓

**Modelo seleccionado**

↓

**Archivo del modelo**

Este archivo será la entrada de la siguiente unidad.

---

# 50. Ejemplo conductor: clasificación de residuos

Supongamos:

**4 clases**

y:

**4.400 imágenes**

Primero entrenamos una CNN propia:

```text
Test accuracy = 82%
```

Después utilizamos:

**MobileNetV2 preentrenado**

con base congelada:

```text
Test accuracy = 88%
```

Finalmente realizamos Fine-Tuning de las últimas capas:

```text
Test accuracy = 90%
```

Además observamos:

**menos confusión entre vidrio y metal.**

La dupla podría justificar la selección:

> El modelo fine-tuned fue seleccionado porque presentó mejor desempeño general, redujo errores entre las clases más problemáticas y mantuvo un tamaño compatible con la futura implementación.

Esto constituye una decisión técnica basada en evidencia.

---

# 51. Transfer Learning y el producto final

El modelo seleccionado deberá posteriormente:

* guardarse;
* cargarse;
* optimizarse;
* integrarse en una aplicación;
* realizar inferencia sobre imágenes nuevas.

Por tanto, al seleccionar el modelo también debemos anticipar:

**¿podremos implementarlo razonablemente?**

Esta pregunta conecta directamente con la Unidad 4.

---

# 52. De accuracy a producción

Supongamos:

### Modelo A

```text
Accuracy = 94%
Tamaño = 600 MB
Inferencia = 3 segundos
```

### Modelo B

```text
Accuracy = 92%
Tamaño = 25 MB
Inferencia = 0,2 segundos
```

Si nuestra aplicación necesita responder rápidamente en un dispositivo limitado, Modelo B podría resultar más adecuado.

Esto anticipa una idea central:

**el mejor modelo académico no siempre es el mejor modelo para producción.**

---

# 53. Preguntas para discusión en clase

### Caso 1

Una dupla posee solamente 1.200 imágenes para entrenar una CNN profunda.

**Pregunta:** ¿Qué estrategia podría permitir aprovechar conocimiento previamente aprendido?

### Caso 2

Cargamos MobileNetV2 con:

```python
weights="imagenet"
```

**Pregunta:** ¿Qué estamos reutilizando realmente?

### Caso 3

Definimos:

```python
base_model.trainable = False
```

**Pregunta:** ¿Qué efecto produce durante el entrenamiento?

### Caso 4

El modelo original tiene 1.000 clases pero nuestro proyecto posee cuatro.

**Pregunta:** ¿Debemos conservar la capa original de salida?

### Caso 5

Después de entrenar el nuevo clasificador descongelamos las últimas 20 capas y continuamos entrenando con un learning rate pequeño.

**Pregunta:** ¿Qué estrategia estamos aplicando?

### Caso 6

Después del Fine-Tuning:

```text
Train = 99%
Validation = 74%
```

**Pregunta:** ¿Podemos afirmar que el Fine-Tuning mejoró el modelo?

### Caso 7

Dos modelos obtienen 91% y 90% de accuracy, pero el segundo es veinte veces más pequeño.

**Pregunta:** ¿Qué información adicional deberíamos considerar antes de seleccionar uno para producción?

### Caso 8

Una dupla utiliza un modelo preentrenado pero no documenta su origen ni configuración.

**Pregunta:** ¿Qué problema presenta esto desde el punto de vista de reproducibilidad?

---

# 54. Síntesis de la Semana 13

Al finalizar esta sesión deben quedar instaladas diez ideas fundamentales:

1. **Transfer Learning permite reutilizar representaciones aprendidas previamente por otro modelo.**
2. **Un modelo preentrenado comienza desde pesos útiles en lugar de pesos completamente aleatorios.**
3. **En clasificación de imágenes normalmente reutilizamos la base convolucional y reemplazamos el clasificador original.**
4. **Congelar una capa significa conservar sus pesos durante el nuevo entrenamiento.**
5. **Entrenar inicialmente solo el nuevo clasificador permite adaptar rápidamente el modelo al nuevo problema.**
6. **Fine-Tuning consiste en descongelar y ajustar parte del modelo preentrenado utilizando normalmente un learning rate menor.**
7. **Transfer Learning y Fine-Tuning pueden ser especialmente útiles cuando existen pocos datos o recursos limitados.**
8. **La utilidad del modelo preentrenado depende también de la similitud entre el dominio original y el nuevo problema.**
9. **La selección del modelo definitivo debe considerar rendimiento, generalización, tamaño y costo computacional, no únicamente accuracy.**
10. **El modelo seleccionado al cerrar UA3 se convertirá en el núcleo que será optimizado e implementado durante UA4.**

### Hacia la Semana 14

Con esta sesión cerramos la **Unidad 3: Deep Learning**.

La progresión fue:

**Semana 9:** fundamentos de Deep Learning.

**Semana 10:** CNN para clasificación de imágenes.

**Semana 11:** LSTM para información secuencial.

**Semana 12:** Autoencoders y GAN.

**Semana 13:** Transfer Learning y Fine-Tuning.

Al finalizar esta etapa disponemos de:

**Problema definido → Dataset preparado → CNN entrenada → Modelo definitivo**

La siguiente pregunta cambia nuevamente el foco:

**¿Cómo hacemos que ese modelo sea más pequeño, rápido y eficiente para utilizarlo fuera del entorno de entrenamiento?**

La **Semana 14** iniciará la Unidad 4: **Implementación de modelos Deep Learning**, abordando **velocidad de inferencia, optimización de recursos, cuantificación y poda de redes**.

