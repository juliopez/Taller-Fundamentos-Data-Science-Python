# Semana 9 — Fundamentos de Deep Learning

## 1. Propósito de la sesión

Comprender los fundamentos del **Deep Learning**, reconociendo sus diferencias respecto de una red neuronal básica y analizando cómo la profundidad, la arquitectura, las capas, la función de pérdida y el proceso de optimización permiten construir modelos capaces de aprender representaciones complejas a partir de los datos.

Al finalizar la sesión, el estudiante debería ser capaz de:

* Explicar qué se entiende por Deep Learning.
* Diferenciar una red neuronal básica de una red neuronal profunda.
* Comprender qué significa la profundidad de una red.
* Identificar los principales componentes de una arquitectura de Deep Learning.
* Comprender cómo las capas sucesivas pueden aprender representaciones cada vez más complejas.
* Relacionar profundidad y cantidad de parámetros con capacidad del modelo.
* Reconocer la función de la pérdida y del optimizador dentro del entrenamiento.
* Comprender el papel de los hiperparámetros.
* Identificar las principales etapas necesarias para entrenar una solución Deep Learning.
* Relacionar estos conceptos con la futura construcción de la CNN del proyecto integrador.

---

# 2. Desde las redes neuronales hacia Deep Learning

Durante la Semana 4 estudiamos los componentes fundamentales de una red neuronal:

* entradas;
* pesos;
* sesgos;
* funciones de activación;
* capas;
* función de pérdida;
* *backpropagation*;
* optimización.

La estructura básica podía representarse como:

**Entrada**
↓
**Capa oculta**
↓
**Salida**

Deep Learning mantiene estos mismos principios.

La diferencia fundamental es que utiliza **redes neuronales con múltiples capas capaces de aprender representaciones progresivamente más complejas de los datos**.

Conceptualmente:

**Datos de entrada**
↓
**Representación 1**
↓
**Representación 2**
↓
**Representación 3**
↓
**...**
↓
**Predicción**

Por ello hablamos de redes neuronales **profundas**.

---

# 3. ¿Qué significa “profunda”?

La palabra **deep** hace referencia principalmente a la existencia de múltiples capas de procesamiento entre la entrada y la salida.

Por ejemplo:

### Red sencilla

**Entrada → Capa oculta → Salida**

### Red más profunda

**Entrada**
↓
**Capa 1**
↓
**Capa 2**
↓
**Capa 3**
↓
**Capa 4**
↓
**Salida**

Cada capa transforma la representación recibida desde la capa anterior.

La profundidad permite construir una sucesión de transformaciones.

Sin embargo:

**más capas no significa automáticamente mejor modelo.**

Una arquitectura debe ser apropiada para:

* el problema;
* los datos;
* la cantidad de ejemplos;
* los recursos computacionales disponibles.

---

# 4. Aprendizaje jerárquico de representaciones

Una de las ideas centrales de Deep Learning es el **aprendizaje jerárquico de representaciones**.

En lugar de definir manualmente todas las características que debería reconocer el modelo, la red aprende representaciones progresivamente más abstractas.

En imágenes podemos imaginar:

**Píxeles**

↓

**Bordes**

↓

**Texturas**

↓

**Formas**

↓

**Partes de objetos**

↓

**Objeto completo**

Por ejemplo:

**píxeles → líneas → formas curvas → ojos/orejas → gato**

Esta representación es conceptual. La red no recibe instrucciones explícitas como:

> “busca primero bordes y después orejas”.

Los patrones emergen durante el entrenamiento a partir de los datos.

---

# 5. Feature engineering tradicional versus Deep Learning

En muchos enfoques tradicionales de Machine Learning, una persona debe decidir previamente qué características utilizar.

Por ejemplo, para clasificar frutas podríamos definir manualmente:

* color promedio;
* diámetro;
* circularidad;
* textura;
* peso.

Después:

**Características construidas manualmente → Modelo ML → Predicción**

En Deep Learning, especialmente con imágenes:

**Imagen → Red profunda → Características aprendidas → Predicción**

Esto reduce la necesidad de definir manualmente todas las características visuales.

El modelo aprende representaciones útiles directamente a partir de los datos.

---

# 6. ¿Por qué Deep Learning resulta especialmente útil con imágenes?

Una imagen puede contener una enorme cantidad de información.

Por ejemplo:

**224 × 224 × 3**

corresponde a:

**150.528 valores**

Definir manualmente reglas capaces de reconocer objetos bajo:

* distintos ángulos;
* diferentes iluminaciones;
* fondos variados;
* escalas distintas;
* posiciones diferentes;

resultaría extremadamente complejo.

Deep Learning permite aprender estas representaciones mediante arquitecturas especializadas.

En nuestro proyecto utilizaremos posteriormente una:

**Red Neuronal Convolucional (CNN)**

diseñada específicamente para explotar la estructura espacial de las imágenes.

---

# 7. Arquitectura de una red neuronal

La **arquitectura** describe cómo está organizada una red neuronal.

Incluye decisiones como:

* número de capas;
* tipo de capas;
* cantidad de neuronas;
* conexiones;
* funciones de activación;
* dimensión de las entradas;
* estructura de la salida.

Por ejemplo:

```text
Input
↓
Dense 256 + ReLU
↓
Dense 128 + ReLU
↓
Dense 64 + ReLU
↓
Softmax
```

Esta secuencia representa una arquitectura.

Dos modelos pueden trabajar con los mismos datos, pero tener arquitecturas completamente diferentes.

---

# 8. Arquitectura y problema

No existe una única arquitectura adecuada para cualquier tipo de información.

Distintas estructuras han sido diseñadas para distintos problemas.

El descriptor de la asignatura considera especialmente: 

### CNN

Especialmente relevantes para imágenes.

### LSTM

Diseñadas para trabajar con información secuencial.

### Autoencoders

Permiten aprender representaciones comprimidas de los datos.

### GAN

Utilizan redes que compiten durante el entrenamiento para generar nuevas muestras.

Durante las próximas semanas conoceremos estas arquitecturas.

Nuestro proyecto integrador tendrá como núcleo:

**CNN → clasificación de imágenes**

---

# 9. Capas en Deep Learning

Una red profunda se construye combinando diferentes capas.

Cada capa recibe una entrada, aplica una transformación y produce una salida.

Conceptualmente:

**Entrada de la capa**

↓

**Transformación**

↓

**Activación**

↓

**Salida de la capa**

La salida de una capa normalmente se convierte en la entrada de la siguiente.

Por tanto:

**Capa 1 → Capa 2 → Capa 3 → ...**

Esto permite construir transformaciones progresivamente más complejas.

---

# 10. Capas densas

Una **capa densa** o *Dense layer* conecta cada una de sus neuronas con las salidas de la capa anterior.

Por ejemplo:

```python
layers.Dense(128, activation="relu")
```

Esto representa:

**128 neuronas**

utilizando:

**ReLU**

como función de activación.

Las capas densas resultan útiles en múltiples arquitecturas y normalmente también aparecen al final de una red de clasificación.

Sin embargo, trabajar exclusivamente con capas densas no aprovecha de manera eficiente la estructura espacial de las imágenes.

Por esta razón posteriormente incorporaremos capas convolucionales.

---

# 11. Profundidad y capacidad

Agregar capas y neuronas aumenta normalmente la **capacidad** del modelo.

Una red con mayor capacidad puede representar relaciones más complejas.

Sin embargo, también puede:

* requerir más datos;
* necesitar mayor tiempo de entrenamiento;
* utilizar más memoria;
* aumentar el riesgo de overfitting;
* ser más difícil de optimizar.

Por tanto:

**mayor capacidad ≠ mejor solución automáticamente**

El diseño de una arquitectura requiere equilibrio.

---

# 12. Cantidad de parámetros

Cada conexión y sesgo puede contribuir con parámetros entrenables.

Supongamos una capa:

**Entrada: 100 valores**

**Salida: 50 neuronas**

Cada neurona recibe 100 entradas.

Entonces existen aproximadamente:

**100 × 50 = 5.000 pesos**

más:

**50 sesgos**

Total:

**5.050 parámetros**

En redes profundas, la cantidad total puede crecer rápidamente.

Modelos modernos pueden contener:

* miles;
* millones;
* cientos de millones;

de parámetros.

---

# 13. Parámetros entrenables

Los parámetros son aquellos valores que el modelo modifica durante el entrenamiento.

Principalmente:

* pesos;
* sesgos.

El proceso sigue la misma lógica aprendida en la Semana 4:

**Forward propagation**

↓

**Predicción**

↓

**Loss**

↓

**Backpropagation**

↓

**Actualización de parámetros**

↓

**Nueva iteración**

Deep Learning no reemplaza este proceso.

Lo amplía hacia arquitecturas con mayor profundidad y capacidad.

---

# 14. Hiperparámetros

Además de los parámetros aprendidos existen decisiones configuradas antes o durante el entrenamiento.

Entre ellas:

* número de capas;
* cantidad de neuronas;
* learning rate;
* batch size;
* cantidad de épocas;
* optimizador;
* dropout;
* funciones de activación.

Estas decisiones son **hiperparámetros**.

Por tanto:

**Parámetros → los aprende la red**

**Hiperparámetros → controlan arquitectura y entrenamiento**

Parte importante del desarrollo de un modelo consiste en experimentar con estos valores.

---

# 15. Funciones de activación

Las funciones de activación continúan siendo fundamentales en redes profundas.

Sin activaciones no lineales, múltiples capas lineales podrían terminar comportándose como una transformación lineal equivalente.

Entre las funciones más utilizadas encontramos:

### ReLU

Frecuente en capas internas.

### Sigmoid

Puede utilizarse en determinadas salidas binarias.

### Softmax

Habitual para clasificación multiclase.

En nuestro clasificador final, una salida típica será:

```text
Clase 1 → 0,04
Clase 2 → 0,07
Clase 3 → 0,84
Clase 4 → 0,05
```

Predicción:

**Clase 3**

---

# 16. Función de pérdida

La función de pérdida determina qué tan diferente es la predicción del resultado esperado.

Durante el entrenamiento:

**Predicción**

*

**Etiqueta real**

↓

**Loss**

El objetivo general consiste en:

**minimizar la pérdida**

Para clasificación multiclase es habitual utilizar funciones relacionadas con:

**categorical crossentropy**

o:

**sparse categorical crossentropy**

según cómo estén representadas las etiquetas.

---

# 17. Ejemplo conceptual de pérdida

Supongamos que la etiqueta correcta es:

**vidrio**

La red produce:

```text
plástico → 0,60
vidrio   → 0,15
cartón   → 0,15
metal    → 0,10
```

La predicción es incorrecta.

La función de pérdida entregará una señal elevada.

Después del aprendizaje podría producir:

```text
plástico → 0,05
vidrio   → 0,85
cartón   → 0,06
metal    → 0,04
```

Ahora la predicción coincide con la etiqueta correcta y la pérdida debería disminuir.

---

# 18. Optimización

La **optimización** consiste en modificar los parámetros para disminuir la función de pérdida.

El proceso conceptual es:

**Calcular error**

↓

**Backpropagation**

↓

**Obtener gradientes**

↓

**Optimizador modifica parámetros**

↓

**Nueva predicción**

Entre los optimizadores habituales encontramos:

* SGD;
* RMSprop;
* Adam.

Durante el proyecto utilizaremos herramientas que implementan automáticamente estas operaciones.

---

# 19. Adam

**Adam** es uno de los optimizadores más utilizados en redes neuronales.

Por ejemplo:

```python
modelo.compile(
    optimizer="adam",
    loss="sparse_categorical_crossentropy",
    metrics=["accuracy"]
)
```

Aquí estamos definiendo:

**cómo se actualizan los parámetros → Adam**

**cómo se calcula el error → loss**

**qué métrica observaremos → accuracy**

Esta configuración determina una parte importante del entrenamiento.

---

# 20. Learning rate

La tasa de aprendizaje determina el tamaño aproximado de las actualizaciones realizadas por el optimizador.

### Muy alta

Puede provocar:

* oscilaciones;
* inestabilidad;
* dificultad para converger.

### Muy baja

Puede provocar:

* aprendizaje excesivamente lento;
* necesidad de muchas épocas.

La tasa de aprendizaje es uno de los hiperparámetros más relevantes en Deep Learning.

---

# 21. Batch size

El **batch size** indica cuántos ejemplos se procesan antes de realizar una actualización de los parámetros.

Por ejemplo:

**Dataset = 6.400 imágenes**

**Batch size = 32**

El entrenamiento utilizará grupos de:

**32 imágenes**

El tamaño del batch influye en:

* memoria utilizada;
* velocidad;
* estabilidad del entrenamiento.

No existe un tamaño universalmente correcto.

---

# 22. Épocas

Una **época** corresponde a una pasada completa del conjunto de entrenamiento.

Por ejemplo:

**Train = 6.400 imágenes**

Después de procesar las 6.400:

**1 época**

Si entrenamos:

```python
epochs=20
```

la red tendrá múltiples oportunidades para ajustar sus parámetros.

Pero entrenar durante más épocas no garantiza mejorar continuamente.

Como vimos en la Semana 7, puede aparecer:

**overfitting**.

---

# 23. El ciclo de entrenamiento en Deep Learning

Podemos representar una época de entrenamiento así:

**Batch de datos**

↓

**Forward propagation**

↓

**Predicción**

↓

**Cálculo de loss**

↓

**Backpropagation**

↓

**Optimizador**

↓

**Actualización de pesos**

↓

**Siguiente batch**

Después de procesar todos los batches:

**termina una época**

y comienza la siguiente.

---

# 24. Entrenamiento y validación

Durante cada época podemos observar dos comportamientos:

### Training

El modelo aprende directamente de estos ejemplos.

### Validation

El modelo se evalúa con datos separados.

Por ejemplo:

```text
Epoch 1
train accuracy: 62%
val accuracy:   60%

Epoch 5
train accuracy: 80%
val accuracy:   77%

Epoch 10
train accuracy: 92%
val accuracy:   86%
```

Estas métricas permiten estudiar la capacidad de generalización.

---

# 25. Deep Learning requiere experimentación

Construir un modelo no consiste únicamente en escribir una arquitectura y ejecutarla una vez.

Habitualmente se desarrolla un ciclo:

**Diseñar**

↓

**Entrenar**

↓

**Evaluar**

↓

**Analizar**

↓

**Modificar**

↓

**Volver a entrenar**

Este proceso puede involucrar cambios en:

* arquitectura;
* hiperparámetros;
* augmentation;
* learning rate;
* cantidad de capas;
* regularización.

La experimentación debe ser controlada y documentada.

---

# 26. Experimentos comparables

Supongamos que probamos tres configuraciones.

| Modelo | Capas | Learning rate | Val Accuracy |
| ------ | ----: | ------------: | -----------: |
| A      |     2 |         0,001 |          81% |
| B      |     3 |         0,001 |          87% |
| C      |     5 |          0,01 |          72% |

Estos resultados permiten comparar alternativas.

Sin embargo, una comparación válida requiere mantener controladas otras condiciones:

* mismo dataset;
* misma división;
* mismas métricas;
* condiciones equivalentes.

Esto permite atribuir mejor los cambios a la modificación realizada.

---

# 27. Reproducibilidad de experimentos

Durante la construcción del modelo deberíamos registrar:

* versión del dataset;
* arquitectura;
* número de épocas;
* batch size;
* optimizador;
* learning rate;
* augmentation;
* métricas obtenidas.

Por ejemplo:

```text
Experimento CNN-01

Dataset: versión 2
Resolución: 224 × 224
Batch size: 32
Optimizer: Adam
Learning rate: 0.001
Epochs: 20
Val accuracy: 86.4%
```

Esto permite comprender posteriormente por qué un modelo funcionó mejor que otro.

---

# 28. Recursos computacionales

Deep Learning puede requerir una cantidad considerable de recursos.

Durante el entrenamiento intervienen principalmente:

### CPU

Ejecuta procesamiento general.

### RAM

Mantiene datos y procesos en memoria.

### GPU

Puede acelerar significativamente operaciones matriciales y entrenamiento de redes profundas.

### Almacenamiento

Contiene datasets, checkpoints y modelos.

La cantidad de recursos necesaria depende del tamaño:

* de las imágenes;
* del modelo;
* del batch;
* del dataset.

---

# 29. CPU versus GPU

Una CPU contiene relativamente pocos núcleos optimizados para tareas generales.

Una GPU posee una gran cantidad de unidades capaces de realizar operaciones en paralelo.

Las redes neuronales utilizan numerosas operaciones matriciales.

Por ello:

**GPU → puede acelerar considerablemente el entrenamiento**

especialmente en modelos grandes y procesamiento de imágenes.

Esto no significa que toda red requiera obligatoriamente GPU, pero constituye una herramienta importante en Deep Learning.

---

# 30. El papel de TensorFlow y Keras

Para construir redes utilizaremos herramientas que abstraen gran parte de las operaciones matemáticas.

**TensorFlow** proporciona infraestructura para cálculo y entrenamiento de modelos.

**Keras** permite construir arquitecturas mediante una interfaz de alto nivel.

Por ejemplo:

```python
from tensorflow import keras
from tensorflow.keras import layers
```

Después podemos construir un modelo:

```python
modelo = keras.Sequential([
    layers.Input(shape=(224, 224, 3)),
    ...
])
```

Las capas específicas de nuestro clasificador serán desarrolladas durante la Semana 10.

---

# 31. Modelo secuencial

Una forma sencilla de construir redes en Keras es mediante:

```python
keras.Sequential
```

Este enfoque representa una secuencia:

**Capa 1 → Capa 2 → Capa 3 → Salida**

Por ejemplo:

```python
modelo = keras.Sequential([
    layers.Input(shape=(100,)),
    layers.Dense(64, activation="relu"),
    layers.Dense(32, activation="relu"),
    layers.Dense(4, activation="softmax")
])
```

Esta red posee dos capas ocultas densas y una salida de cuatro clases.

---

# 32. Resumen del modelo

Keras permite inspeccionar la arquitectura:

```python
modelo.summary()
```

Puede mostrar información como:

* nombre de cada capa;
* forma de salida;
* cantidad de parámetros;
* parámetros entrenables.

Comprender este resumen será importante cuando construyamos nuestra CNN.

No debería utilizarse simplemente como salida automática.

El estudiante debe interpretar:

**qué arquitectura construyó y cuántos parámetros contiene.**

---

# 33. Entrenamiento mediante fit

Una vez compilado:

```python
historial = modelo.fit(
    train_ds,
    validation_data=validation_ds,
    epochs=20
)
```

Este comando ejecuta el ciclo estudiado conceptualmente:

**Forward propagation**

↓

**Loss**

↓

**Backpropagation**

↓

**Optimización**

↓

**Actualización de parámetros**

↓

**Validación**

La biblioteca automatiza el cálculo, pero no elimina la necesidad de comprender qué está ocurriendo.

---

# 34. Evaluación

Después del entrenamiento podemos evaluar:

```python
modelo.evaluate(test_ds)
```

El resultado puede entregar:

* loss;
* accuracy;
* otras métricas configuradas.

El test debe utilizarse al final del proceso de selección, manteniendo la lógica estudiada en la Unidad 2:

**Train → aprender**

**Validation → seleccionar**

**Test → evaluar finalmente**

---

# 35. Guardar el modelo

Una vez entrenado podemos conservarlo:

```python
modelo.save("modelo.keras")
```

Esto guarda los elementos necesarios para reutilizarlo posteriormente.

Conceptualmente:

**Entrenamiento**

↓

**Modelo aprendido**

↓

**Archivo**

↓

**Carga posterior**

↓

**Inferencia**

---

# 36. Del modelo al producto

El proyecto no termina cuando obtenemos buenas métricas.

La progresión será:

**Dataset definitivo**

↓

**CNN**

↓

**Entrenamiento**

↓

**Evaluación**

↓

**Modelo definitivo**

↓

**Guardar modelo**

↓

**Integrarlo en una aplicación**

↓

**Realizar inferencia**

Por esta razón, la Unidad 3 tiene como objetivo construir el **núcleo inteligente** del producto final.

La Unidad 4 se ocupará posteriormente de su implementación.

---

# 37. Arquitecturas que estudiaremos en la UA3

Durante esta unidad recorreremos distintas arquitecturas contempladas en el descriptor. 

### CNN

Foco principal del proyecto.

### LSTM

Arquitectura orientada a secuencias.

### Autoencoders

Aprendizaje de representaciones.

### GAN

Generación de nuevos datos mediante redes adversarias.

### Transfer Learning y Fine-Tuning

Reutilización y adaptación de modelos previamente entrenados.

No todas formarán parte del producto final.

Sin embargo, permiten comprender que **Deep Learning no corresponde a una única arquitectura**.

---

# 38. ¿Por qué CNN será el núcleo del proyecto?

Nuestro problema presenta:

**Entrada → imagen**

**Salida → clase**

La imagen contiene estructura espacial:

* píxeles vecinos;
* bordes;
* texturas;
* formas;
* objetos.

Las CNN incorporan operaciones especialmente diseñadas para aprovechar esta estructura.

Por ello:

**Imagen → CNN → características visuales → clasificación**

Será el modelo central del proyecto integrador.

La Semana 10 estará dedicada específicamente a comprender su funcionamiento.

---

# 39. Aplicación al proyecto integrador

Al comenzar la Unidad 3, cada dupla debería poseer:

**Problema definido**

*

**Clases definidas**

*

**Dataset definitivo**

*

**Train / Validation / Test**

*

**Estrategia de augmentation**

A partir de ahora el foco pasa a:

**diseñar y entrenar el modelo definitivo.**

Durante esta etapa será necesario justificar:

* arquitectura;
* capas;
* hiperparámetros;
* proceso de entrenamiento;
* comportamiento train/validation;
* control del overfitting;
* experimentos;
* resultados finales.

---

# 40. Ejemplo conductor: primera arquitectura del clasificador de residuos

Recordemos nuestro dataset:

```text
4 clases

plástico
vidrio
cartón
metal
```

Entrada:

```text
224 × 224 × 3
```

Objetivo:

**clasificación multiclase**

Una arquitectura conceptual podría ser:

**Imagen 224 × 224 × 3**

↓

**Extracción de características**

↓

**Representación interna**

↓

**Capa de clasificación**

↓

**Softmax**

↓

```text
plástico → 0,03
vidrio   → 0,89
cartón   → 0,05
metal    → 0,03
```

Resultado:

**Vidrio — 89%**

Durante la próxima semana reemplazaremos la idea general de:

**“extracción de características”**

por operaciones concretas:

**convolución + filtros + pooling**

---

# 41. ¿Qué significa que un modelo sea adecuado?

El criterio no debería ser únicamente:

**“funciona”**

Un modelo adecuado debe analizarse considerando:

* desempeño;
* generalización;
* complejidad;
* datos disponibles;
* recursos necesarios;
* estabilidad;
* objetivo del problema.

Por ejemplo:

Un modelo con:

**90% test accuracy**

y:

**2 millones de parámetros**

podría resultar preferible a otro con:

**91% test accuracy**

pero:

**150 millones de parámetros**

si posteriormente necesitamos ejecutarlo en un dispositivo con pocos recursos.

Esta idea anticipa la Unidad 4.

---

# 42. Precisión versus complejidad

En Deep Learning aparecen frecuentemente compromisos.

Por ejemplo:

**Mayor arquitectura**

puede significar:

* mejor capacidad;
* mayor precisión potencial;

pero también:

* mayor memoria;
* entrenamiento más lento;
* inferencia más lenta;
* mayor riesgo de overfitting.

Por tanto, la mejor solución no siempre corresponde al modelo más grande.

El diseño debe responder al problema completo.

---

# 43. Deep Learning como proceso

Podemos sintetizar el desarrollo de una solución mediante:

**1. Definir problema**

↓

**2. Preparar datos**

↓

**3. Seleccionar arquitectura**

↓

**4. Configurar hiperparámetros**

↓

**5. Entrenar**

↓

**6. Validar**

↓

**7. Analizar errores**

↓

**8. Modificar**

↓

**9. Volver a entrenar**

↓

**10. Evaluar en test**

↓

**11. Guardar modelo**

↓

**12. Implementar**

Este proceso conecta directamente las cuatro unidades de la asignatura.

---

# 44. Preguntas para discusión en clase

### Caso 1

Una red posee diez capas ocultas.

**Pregunta:** ¿Qué característica permite considerarla más profunda que una red con una única capa oculta?

### Caso 2

Un modelo tiene millones de parámetros y solamente 200 imágenes disponibles.

**Pregunta:** ¿Qué problema estudiado anteriormente podría aparecer?

### Caso 3

Una red obtiene:

```text
Train: 99%
Validation: 67%
```

**Pregunta:** ¿Agregar inmediatamente más capas parece una solución razonable? ¿Por qué?

### Caso 4

Un estudiante cambia al mismo tiempo:

* arquitectura;
* learning rate;
* batch size;
* augmentation.

El modelo mejora.

**Pregunta:** ¿Puede determinar con claridad qué modificación produjo la mejora?

### Caso 5

Un clasificador utiliza `softmax` para cuatro categorías.

**Pregunta:** ¿Qué representa cada uno de los cuatro valores de salida?

### Caso 6

Una CNN demora tres horas en entrenarse y se utiliza *5-fold cross-validation*.

**Pregunta:** ¿Qué aspecto práctico debería analizarse antes de aplicar esta estrategia?

### Caso 7

El modelo definitivo se entrenó correctamente, pero no fue guardado.

**Pregunta:** ¿Qué problema producirá esto cuando llegue la etapa de implementación?

---

# 45. Síntesis de la Semana 9

Al finalizar esta sesión deben quedar instaladas nueve ideas fundamentales:

1. **Deep Learning utiliza redes neuronales con múltiples capas para aprender representaciones complejas.**
2. **La profundidad permite desarrollar transformaciones progresivamente más abstractas de los datos.**
3. **La arquitectura define cómo se organizan y conectan las capas del modelo.**
4. **Aumentar capas y parámetros incrementa la capacidad, pero también puede aumentar el costo computacional y el riesgo de overfitting.**
5. **Los parámetros se aprenden durante el entrenamiento, mientras que los hiperparámetros controlan la arquitectura y el proceso de aprendizaje.**
6. **La función de pérdida, backpropagation y el optimizador constituyen elementos centrales del entrenamiento de una red profunda.**
7. **El desarrollo de un modelo requiere experimentar, evaluar y comparar configuraciones de manera controlada.**
8. **La Unidad 3 transformará el dataset preparado durante UA2 en el modelo Deep Learning definitivo del proyecto.**
9. **Para clasificación de imágenes utilizaremos principalmente una CNN, debido a su capacidad para aprender representaciones espaciales y visuales.**

### Hacia la Semana 10

Hasta este momento disponemos de:

**Problema → Dataset → Fundamentos de Deep Learning**

Ahora debemos responder:

**¿Cómo puede una red aprender automáticamente bordes, texturas, formas y patrones visuales presentes en una imagen?**

La **Semana 10** abordará el núcleo técnico del proyecto integrador:

**Redes Neuronales Convolucionales (CNN)**

incluyendo **convolución, filtros, mapas de características, pooling y clasificación de imágenes**.

