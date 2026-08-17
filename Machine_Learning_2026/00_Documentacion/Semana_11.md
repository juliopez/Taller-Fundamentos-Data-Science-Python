# Semana 11 — Redes para datos secuenciales: LSTM

## 1. Propósito de la sesión

Comprender el funcionamiento general de las **redes neuronales para datos secuenciales**, con especial énfasis en las redes **LSTM (Long Short-Term Memory)**, identificando por qué determinados problemas requieren considerar el orden y el contexto de los datos.

Al finalizar la sesión, el estudiante debería ser capaz de:

* Explicar qué se entiende por dato secuencial.
* Identificar problemas en los que el orden de las observaciones es relevante.
* Comprender las limitaciones de una red neuronal tradicional frente a secuencias.
* Explicar conceptualmente qué es una red neuronal recurrente.
* Comprender el concepto de estado oculto.
* Reconocer el problema de las dependencias de largo plazo.
* Explicar conceptualmente cómo una LSTM mantiene y actualiza información.
* Identificar las principales compuertas de una LSTM.
* Reconocer aplicaciones de LSTM en texto y series temporales.
* Diferenciar el tipo de problema abordado por una CNN respecto de una LSTM.

---

# 2. ¿Qué es un dato secuencial?

Un **dato secuencial** es aquel en el que el orden de los elementos posee significado.

Por ejemplo:

**“El perro persigue al gato”**

no representa exactamente lo mismo que:

**“El gato persigue al perro”**

Las mismas palabras aparecen en ambas frases, pero el orden cambia el significado.

Esto también ocurre con:

* series temporales;
* señales;
* audio;
* secuencias de eventos;
* texto;
* datos financieros;
* registros de sensores.

Por tanto:

**en una secuencia, la posición y el contexto importan.**

---

# 3. Ejemplos de datos secuenciales

Podemos encontrar datos secuenciales en múltiples contextos.

### Texto

```text
Machine Learning permite aprender patrones.
```

Cada palabra depende parcialmente de las anteriores y posteriores.

### Temperatura

```text
18°, 19°, 21°, 22°, 20°, 18°
```

El valor actual puede estar relacionado con valores anteriores.

### Ventas diarias

```text
120, 135, 128, 160, 175, 190
```

Existe un orden temporal.

### Audio

Una señal sonora corresponde a una sucesión de valores a través del tiempo.

Por tanto, en todos estos casos:

**intercambiar arbitrariamente los elementos puede destruir información relevante.**

---

# 4. Datos independientes versus datos secuenciales

Consideremos un problema tabular tradicional.

| Edad | Ingreso | Compras |
| ---: | ------: | ------: |
|   25 |  800000 |       4 |
|   37 | 1200000 |      12 |
|   48 | 1700000 |      20 |

En muchos casos, cada fila puede analizarse de manera relativamente independiente.

Ahora consideremos:

```text
Día 1 → Día 2 → Día 3 → Día 4
```

El valor del Día 4 puede depender de lo ocurrido antes.

En este caso:

**la secuencia contiene información adicional.**

---

# 5. Limitación de una red neuronal tradicional

Una red neuronal densa procesa normalmente una entrada y produce una salida sin mantener explícitamente memoria de entradas anteriores.

Conceptualmente:

**Entrada actual → Red → Salida**

Después:

**Nueva entrada → Red → Nueva salida**

No existe necesariamente una memoria interna que conecte ambas observaciones.

Esto puede resultar insuficiente cuando necesitamos responder:

**¿Qué ocurrió antes?**

Por ejemplo, al procesar una frase palabra por palabra, necesitamos mantener información del contexto previo.

---

# 6. Redes neuronales recurrentes

Las **Redes Neuronales Recurrentes (RNN)** fueron diseñadas para trabajar con secuencias.

Su característica principal es que incorporan información del estado anterior.

Conceptualmente:

**Entrada actual + Estado anterior → Nuevo estado + Salida**

Por tanto, una RNN posee una forma de memoria.

Podemos representarla como:

```text
x₁ → RNN → h₁
          ↓
x₂ → RNN → h₂
          ↓
x₃ → RNN → h₃
```

donde:

* **x** corresponde a las entradas;
* **h** corresponde al estado oculto.

---

# 7. Estado oculto

El **estado oculto** o *hidden state* representa información que la red mantiene mientras procesa una secuencia.

Por ejemplo:

**Palabra 1**

↓

**Estado h₁**

↓

**Palabra 2 + h₁**

↓

**Estado h₂**

↓

**Palabra 3 + h₂**

El estado actúa como una representación resumida del contexto anterior.

Esto permite que la red considere información de elementos previos al procesar el siguiente elemento.

---

# 8. Ejemplo con texto

Consideremos:

**“El estudiante rindió la prueba y obtuvo una excelente...”**

Al recibir la palabra:

**“excelente”**

el contexto anterior puede ayudar a predecir qué palabra podría aparecer después.

Por ejemplo:

**“calificación”**

Una red secuencial intenta utilizar información acumulada del contexto para procesar la secuencia.

Por tanto:

**las entradas anteriores influyen sobre la interpretación de las siguientes.**

---

# 9. Desplegar una RNN en el tiempo

Una RNN puede representarse conceptualmente como una misma estructura utilizada repetidamente.

```text
t1          t2          t3          t4

x1 → RNN → x2 → RNN → x3 → RNN → x4
      ↓          ↓          ↓
      h1         h2         h3
```

La misma red procesa diferentes pasos de tiempo.

Esto permite compartir parámetros y mantener contexto.

La variable:

**t**

representa normalmente el paso temporal o posición dentro de la secuencia.

---

# 10. Secuencias de entrada y salida

Las redes recurrentes pueden utilizarse en diferentes configuraciones.

### Uno a uno

**Entrada → Salida**

Es similar a una red tradicional.

### Muchos a uno

**Secuencia → Una salida**

Ejemplo:

**Texto completo → sentimiento positivo/negativo**

### Uno a muchos

**Una entrada → Secuencia**

### Muchos a muchos

**Secuencia → Secuencia**

Ejemplo conceptual:

**secuencia de palabras → secuencia transformada**

La arquitectura dependerá del problema.

---

# 11. El problema de las dependencias largas

Las RNN tradicionales pueden presentar dificultades cuando deben recordar información que apareció muchos pasos antes.

Consideremos:

> “El estudiante, después de revisar durante varias horas todos los documentos disponibles en la biblioteca, finalmente entregó su...”

Para interpretar correctamente el final de la oración puede ser necesario mantener información desde el comienzo.

Cuando una secuencia es extensa, una RNN tradicional puede perder gradualmente información relevante.

Esto se relaciona con problemas durante el entrenamiento como:

**vanishing gradient**

o gradiente desvaneciente.

---

# 12. Vanishing gradient

Recordemos que las redes aprenden mediante:

**Backpropagation**

En redes recurrentes, este proceso se extiende a través de los pasos de la secuencia.

Cuando la información debe propagarse por muchos pasos, los gradientes pueden volverse extremadamente pequeños.

Conceptualmente:

**Gradiente**

↓

**Paso 1**

↓

**Paso 2**

↓

**Paso 3**

↓

**...**

↓

**valor cada vez menor**

Esto dificulta que la red aprenda dependencias de largo plazo.

---

# 13. Backpropagation Through Time

En una RNN, el entrenamiento utiliza una variante conocida como:

**Backpropagation Through Time (BPTT)**

Conceptualmente, desplegamos la red a través de la secuencia:

```text
t1 → t2 → t3 → t4 → t5
```

y calculamos cómo los errores afectan los parámetros compartidos a través de esos pasos.

Esto permite aprender relaciones temporales.

Sin embargo, cuando la secuencia es extensa aparecen las dificultades asociadas al gradiente.

Una de las arquitecturas diseñadas para enfrentar este problema es:

**LSTM**

---

# 14. ¿Qué es una LSTM?

**LSTM** significa:

**Long Short-Term Memory**

Es un tipo especializado de red neuronal recurrente diseñado para mantener información relevante durante períodos más largos.

La idea principal consiste en incorporar mecanismos que permitan decidir:

* qué información conservar;
* qué información olvidar;
* qué información incorporar;
* qué información utilizar como salida.

Estos mecanismos reciben el nombre de:

**compuertas o gates.**

---

# 15. La memoria de una LSTM

Una LSTM utiliza dos elementos importantes:

### Estado oculto

Habitualmente representado como:

**hₜ**

### Estado de celda

Habitualmente representado como:

**Cₜ**

El estado de celda puede entenderse conceptualmente como una vía mediante la cual la información relevante puede mantenerse a través de la secuencia.

Esto permite que una LSTM gestione mejor relaciones de largo plazo que una RNN básica.

---

# 16. Las compuertas de una LSTM

Una LSTM incorpora principalmente tres mecanismos:

**Forget gate**

**Input gate**

**Output gate**

Cada uno controla una parte diferente del flujo de información.

Podemos pensar en ellos como decisiones internas:

**¿Qué debo olvidar?**

**¿Qué nueva información debo guardar?**

**¿Qué información debo utilizar ahora?**

---

# 17. Forget Gate

La **forget gate** o compuerta de olvido determina qué información del estado anterior debe mantenerse o descartarse.

Conceptualmente:

**Memoria anterior**

↓

**¿Sigue siendo relevante?**

↓

**Sí → conservar**

**No → reducir/eliminar**

Esto evita que la red mantenga indefinidamente información que ya no resulta útil.

---

# 18. Input Gate

La **input gate** determina qué nueva información debe incorporarse al estado de memoria.

Conceptualmente:

**Nueva entrada**

↓

**Evaluar relevancia**

↓

**Actualizar memoria**

Por tanto, la LSTM no incorpora toda la información con la misma importancia.

Aprende qué aspectos de la entrada actual deberían modificar su estado interno.

---

# 19. Output Gate

La **output gate** determina qué parte del estado interno debe utilizarse para generar la salida actual.

Conceptualmente:

**Memoria actual**

↓

**Seleccionar información relevante**

↓

**Estado oculto / salida**

Por tanto, las tres compuertas pueden resumirse como:

**Forget → qué olvidar**

**Input → qué incorporar**

**Output → qué utilizar**

---

# 20. Flujo conceptual de una LSTM

Podemos representar una unidad LSTM de forma simplificada:

**Entrada actual xₜ**

*

**Estado anterior hₜ₋₁**

*

**Memoria anterior Cₜ₋₁**

↓

**Forget Gate**

↓

**Input Gate**

↓

**Actualizar memoria Cₜ**

↓

**Output Gate**

↓

**Nuevo estado hₜ**

El proceso se repite para cada elemento de la secuencia.

---

# 21. ¿Las compuertas están programadas manualmente?

No.

No escribimos reglas como:

> “si aparece esta palabra, recuerda durante cinco pasos”.

Las compuertas contienen parámetros que se aprenden durante el entrenamiento.

Por tanto:

**las decisiones de conservar, olvidar o utilizar información también son aprendidas.**

El modelo ajusta estos parámetros mediante:

**loss + backpropagation + optimización**

igual que otras redes neuronales.

---

# 22. Ejemplo conceptual con una frase

Consideremos:

> “La película comenzó lentamente, pero finalmente fue excelente.”

Si queremos determinar el sentimiento general, las primeras palabras:

**“comenzó lentamente”**

podrían sugerir una evaluación negativa.

Sin embargo:

**“pero finalmente fue excelente”**

cambia fuertemente el contexto.

Una LSTM puede mantener y actualizar información a medida que procesa la oración completa.

El resultado podría ser:

**sentimiento positivo**

---

# 23. Texto como secuencia

Un computador no procesa directamente palabras como conceptos lingüísticos.

Primero debemos convertirlas en representaciones numéricas.

Por ejemplo:

```text
"machine"
"learning"
"es"
"útil"
```

podrían transformarse en identificadores:

```text
[15, 87, 4, 231]
```

Posteriormente cada palabra puede representarse mediante vectores numéricos.

El proceso general es:

**Texto**

↓

**Tokenización**

↓

**Representación numérica**

↓

**Modelo secuencial**

↓

**Predicción**

---

# 24. Tokenización

La **tokenización** consiste en dividir un texto en unidades.

Por ejemplo:

```text
"Machine Learning es interesante"
```

puede convertirse en:

```text
["Machine", "Learning", "es", "interesante"]
```

Estas unidades reciben el nombre de:

**tokens**

Dependiendo del sistema, un token puede corresponder a:

* una palabra;
* parte de una palabra;
* un carácter;
* otra unidad textual.

La tokenización constituye una etapa previa al procesamiento mediante redes de texto.

---

# 25. Embeddings

Representar palabras únicamente mediante números enteros no expresa similitud semántica.

Por ejemplo:

```text
gato = 12
perro = 13
computador = 14
```

El hecho de que 12 y 13 estén próximos no significa automáticamente que “gato” y “perro” sean conceptos similares.

Los **embeddings** representan tokens mediante vectores numéricos aprendidos.

Conceptualmente:

**Palabra → Vector**

Esto permite que el modelo aprenda relaciones entre términos.

---

# 26. Arquitectura simple para texto

Una arquitectura conceptual podría ser:

**Texto**

↓

**Tokenización**

↓

**Embedding**

↓

**LSTM**

↓

**Dense**

↓

**Clasificación**

Por ejemplo:

**Comentario de cliente**

↓

**LSTM**

↓

**Positivo / Negativo**

Esta estructura combina una representación de palabras con procesamiento secuencial.

---

# 27. LSTM con Keras

Una arquitectura sencilla puede representarse como:

```python
from tensorflow import keras
from tensorflow.keras import layers

modelo = keras.Sequential([
    layers.Embedding(
        input_dim=10000,
        output_dim=128
    ),

    layers.LSTM(64),

    layers.Dense(
        1,
        activation="sigmoid"
    )
])
```

Conceptualmente:

**Tokens**

↓

**Embeddings**

↓

**LSTM**

↓

**Clasificación binaria**

---

# 28. Parámetros básicos de una LSTM

En:

```python
layers.LSTM(64)
```

el número:

**64**

indica la cantidad de unidades internas de la capa LSTM.

Una cantidad mayor puede aumentar la capacidad para representar relaciones complejas.

Sin embargo, también puede:

* aumentar parámetros;
* incrementar tiempo de entrenamiento;
* aumentar riesgo de overfitting.

Nuevamente:

**más capacidad no significa automáticamente mejor modelo.**

---

# 29. Return sequences

Una LSTM puede devolver:

### Solo el último estado

```python
layers.LSTM(64)
```

Esto puede resultar adecuado cuando queremos:

**secuencia completa → una clasificación**

### Toda la secuencia de estados

```python
layers.LSTM(
    64,
    return_sequences=True
)
```

Esto resulta útil cuando una capa posterior necesita recibir información correspondiente a cada paso temporal.

La configuración depende de la arquitectura.

---

# 30. LSTM apiladas

También podemos construir múltiples capas LSTM.

Por ejemplo:

```python
modelo = keras.Sequential([
    layers.Embedding(10000, 128),

    layers.LSTM(
        64,
        return_sequences=True
    ),

    layers.LSTM(32),

    layers.Dense(
        1,
        activation="sigmoid"
    )
])
```

Conceptualmente:

**Secuencia**

↓

**Representación secuencial 1**

↓

**Representación secuencial 2**

↓

**Clasificación**

Esto aumenta la profundidad del modelo.

---

# 31. Bidirectional LSTM

En algunos problemas es útil analizar una secuencia en ambas direcciones.

Por ejemplo:

```python
layers.Bidirectional(
    layers.LSTM(64)
)
```

Conceptualmente:

**Izquierda → derecha**

y:

**Derecha → izquierda**

Esto permite que una representación utilice información proveniente de ambos contextos.

Puede ser especialmente útil en tareas de procesamiento de texto donde conocemos la secuencia completa.

---

# 32. Ejemplo de clasificación de sentimiento

Supongamos comentarios de clientes:

```text
"Excelente producto y entrega rápida"
```

Etiqueta:

**positivo**

Otro:

```text
"El producto llegó dañado y con retraso"
```

Etiqueta:

**negativo**

Dataset:

| Texto                | Etiqueta |
| -------------------- | -------- |
| Excelente producto   | positivo |
| Muy mala experiencia | negativo |
| Llegó rápido         | positivo |
| Producto defectuoso  | negativo |

La LSTM aprende relaciones entre secuencias de palabras y sus respectivas categorías.

Esto corresponde a:

**aprendizaje supervisado de clasificación**

igual que nuestro proyecto de imágenes, aunque utilizando una arquitectura diferente.

---

# 33. LSTM y series temporales

Las LSTM también pueden utilizarse con datos numéricos ordenados temporalmente.

Por ejemplo:

```text
Ventas mes 1
Ventas mes 2
Ventas mes 3
Ventas mes 4
...
```

El modelo puede utilizar una secuencia de valores anteriores para intentar predecir:

**un valor futuro**

Conceptualmente:

**historial**

↓

**LSTM**

↓

**predicción siguiente período**

Este sería un problema de regresión secuencial.

---

# 34. Ventanas temporales

Para utilizar series temporales podemos crear secuencias o ventanas.

Por ejemplo:

```text
Datos:
10, 12, 15, 18, 20, 24
```

Podríamos construir:

```text
[10,12,15] → 18
[12,15,18] → 20
[15,18,20] → 24
```

Cada entrada contiene información de pasos anteriores.

La LSTM intenta aprender relaciones temporales dentro de estas secuencias.

---

# 35. El orden no puede alterarse arbitrariamente

En problemas tabulares podemos mezclar registros antes de dividirlos.

En series temporales, esto debe analizarse cuidadosamente.

Supongamos:

**2024 → entrenamiento**

**2025 → prueba**

Esto representa una situación temporal realista:

**aprender del pasado → predecir el futuro**

Si mezclamos aleatoriamente valores del futuro dentro del entrenamiento, podríamos generar:

**data leakage**

Por tanto, los datos secuenciales requieren estrategias de validación apropiadas para su naturaleza.

---

# 36. Padding de secuencias

Los textos pueden tener longitudes diferentes.

Por ejemplo:

```text
Texto A → 5 tokens
Texto B → 12 tokens
Texto C → 30 tokens
```

Para procesarlos en lotes puede ser necesario llevarlos hacia una longitud consistente.

Una estrategia es:

**padding**

Por ejemplo:

```text
[5, 8, 2]
```

podría convertirse en:

```text
[5, 8, 2, 0, 0]
```

El padding agrega valores para igualar longitudes.

---

# 37. Truncamiento

También puede ocurrir que una secuencia sea demasiado larga.

Podemos establecer:

**longitud máxima = 100 tokens**

Si un texto posee:

**160 tokens**

podemos truncarlo.

Sin embargo, esto implica eliminar información.

Por tanto, la longitud máxima constituye una decisión que debe analizarse según el problema y la distribución real de las secuencias.

---

# 38. LSTM y overfitting

Al igual que las CNN, una LSTM puede sobreajustarse.

Señales:

```text
Train accuracy: 98%
Validation accuracy: 72%
```

Podemos aplicar estrategias como:

* dropout;
* reducir unidades;
* obtener más datos;
* early stopping;
* regularización.

Keras permite incluso configurar dropout dentro de una LSTM.

Por ejemplo:

```python
layers.LSTM(
    64,
    dropout=0.2
)
```

---

# 39. CNN versus LSTM

Las arquitecturas responden a diferentes estructuras de datos.

| Aspecto                  | CNN                 | LSTM                      |
| ------------------------ | ------------------- | ------------------------- |
| Foco principal           | Estructura espacial | Estructura secuencial     |
| Dato típico              | Imagen              | Texto / serie temporal    |
| Relación importante      | Vecindad espacial   | Orden temporal/secuencial |
| Operación característica | Convolución         | Recurrencia y memoria     |
| Proyecto integrador      | **Sí**              | No como modelo central    |

Por tanto:

**CNN no es “mejor” que LSTM**

ni:

**LSTM es “más avanzada” que CNN**

Son arquitecturas diseñadas para problemas diferentes.

---

# 40. ¿Podrían combinarse CNN y LSTM?

Sí.

Existen problemas donde ambos tipos de estructura aparecen simultáneamente.

Por ejemplo:

**Video**

contiene:

* estructura espacial en cada imagen;
* estructura temporal entre los cuadros.

Conceptualmente:

**CNN → extrae características visuales de cada frame**

↓

**LSTM → analiza la secuencia temporal**

Otro ejemplo podría involucrar:

**secuencias de imágenes médicas**

o:

**análisis de eventos visuales en el tiempo**

Esto muestra que las arquitecturas pueden combinarse.

---

# 41. CNN y texto

Las CNN también pueden aplicarse a determinados problemas de texto.

Del mismo modo, las arquitecturas modernas para secuencias han evolucionado ampliamente.

Por tanto, la asociación:

**CNN = únicamente imágenes**

**LSTM = únicamente texto**

es una simplificación pedagógica.

Sin embargo, resulta útil como primera orientación:

**CNN → patrones espaciales**

**LSTM → dependencias secuenciales**

---

# 42. Limitaciones de las LSTM

Aunque las LSTM mejoraron significativamente el procesamiento de dependencias largas respecto de RNN básicas, presentan limitaciones.

Entre ellas:

* procesamiento secuencial menos paralelizable;
* entrenamiento potencialmente lento;
* dificultad creciente con secuencias muy largas;
* mayor complejidad que una RNN simple.

Además, actualmente muchas tareas de lenguaje utilizan arquitecturas basadas en:

**Transformers**

Sin embargo, LSTM sigue siendo fundamental para comprender la evolución y lógica del aprendizaje secuencial.

---

# 43. LSTM y Transformers

Los **Transformers** utilizan un enfoque diferente basado principalmente en mecanismos de atención.

Han adquirido una enorme importancia en:

* procesamiento de lenguaje natural;
* modelos generativos;
* visión;
* tareas multimodales.

Sin embargo, el descriptor de esta asignatura establece específicamente **LSTM** dentro de los contenidos mínimos de Deep Learning. 

Por ello, esta sesión se concentra en comprender:

**memoria + recurrencia + secuencia**

como paradigma fundamental.

---

# 44. ¿Qué debemos retener para el curso?

No necesitamos convertir el proyecto integrador en un proyecto LSTM.

La finalidad de esta sesión es reconocer que diferentes tipos de datos requieren arquitecturas diferentes.

Hasta ahora:

**Imagen → CNN**

Ahora:

**Secuencia → LSTM**

Posteriormente veremos:

**Representación comprimida → Autoencoder**

y:

**Generación de datos → GAN**

Esto permite construir un mapa más amplio de Deep Learning.

---

# 45. Aplicación indirecta al proyecto integrador

Aunque el modelo final seguirá siendo una CNN, esta semana entrega una competencia importante:

**seleccionar una arquitectura en función de la estructura del problema.**

Nuestro proyecto posee:

**imágenes independientes**

y busca:

**clasificación visual**

Por ello seleccionamos:

**CNN**

Si el problema hubiera sido:

**clasificar comentarios de usuarios**

podríamos evaluar una arquitectura secuencial.

Si fuera:

**predecir demanda a partir de una secuencia histórica**

también podríamos considerar modelos orientados a secuencias.

El punto central es:

**la arquitectura debe responder al dato y al objetivo.**

---

# 46. Ejemplo conductor: tres problemas diferentes

### Problema A

Clasificar residuos utilizando una fotografía.

Entrada:

**imagen**

Arquitectura principal:

**CNN**

### Problema B

Clasificar una reseña como positiva o negativa.

Entrada:

**secuencia de palabras**

Arquitectura posible:

**LSTM**

### Problema C

Predecir las ventas de la próxima semana utilizando las últimas 12 semanas.

Entrada:

**secuencia temporal**

Arquitectura posible:

**LSTM**

Los tres son problemas de Machine Learning, pero la naturaleza de la entrada cambia la arquitectura apropiada.

---

# 47. Preguntas para discusión en clase

### Caso 1

Tenemos fotografías independientes de diferentes especies de plantas.

**Pregunta:** ¿Existe una razón evidente para utilizar una LSTM en lugar de una CNN?

### Caso 2

Queremos analizar comentarios escritos por clientes.

**Pregunta:** ¿Qué característica del dato hace relevante considerar una arquitectura secuencial?

### Caso 3

En una oración intercambiamos completamente el orden de las palabras.

**Pregunta:** ¿Por qué esto demuestra que el texto posee estructura secuencial?

### Caso 4

Una RNN necesita utilizar información que apareció 50 pasos antes.

**Pregunta:** ¿Qué dificultad puede aparecer?

### Caso 5

Una LSTM decide internamente conservar determinada información durante varios pasos.

**Pregunta:** ¿Qué componente conceptual permite gestionar esa memoria?

### Caso 6

Queremos utilizar las ventas de enero a noviembre para estimar diciembre.

**Pregunta:** ¿Sería razonable mezclar aleatoriamente diciembre dentro del conjunto de entrenamiento? ¿Por qué?

### Caso 7

Un problema requiere analizar simultáneamente qué aparece en cada frame de un video y cómo cambia a través del tiempo.

**Pregunta:** ¿Podría tener sentido combinar CNN y LSTM?

---

# 48. Síntesis de la Semana 11

Al finalizar esta sesión deben quedar instaladas nueve ideas fundamentales:

1. **Los datos secuenciales poseen un orden que contiene información relevante.**
2. **Las redes densas tradicionales no mantienen explícitamente un estado que represente entradas anteriores.**
3. **Las RNN incorporan recurrencia y un estado oculto para procesar secuencias.**
4. **Las RNN tradicionales pueden presentar dificultades para aprender dependencias de largo plazo.**
5. **LSTM fue diseñada para gestionar mejor información relevante a través de secuencias extensas.**
6. **Las compuertas de olvido, entrada y salida controlan qué información se conserva, incorpora y utiliza.**
7. **LSTM puede aplicarse a texto, series temporales y otros datos secuenciales.**
8. **CNN y LSTM responden a estructuras diferentes: principalmente espacial y secuencial, respectivamente.**
9. **Seleccionar una arquitectura adecuada requiere comprender primero la naturaleza de los datos y del problema.**

### Hacia la Semana 12

Dentro de la Unidad 3 hemos revisado:

**Semana 9:** fundamentos de Deep Learning.

**Semana 10:** CNN para aprender patrones espaciales y clasificar imágenes.

**Semana 11:** LSTM para aprender dependencias dentro de secuencias.

La siguiente sesión ampliará nuevamente el mapa de arquitecturas mediante dos enfoques diferentes:

**¿Puede una red aprender una representación comprimida de los datos?**

y:

**¿Puede una red aprender a generar ejemplos nuevos?**

La **Semana 12** abordará **Autoencoders y Redes Generativas Adversarias (GAN)**, revisando su funcionamiento conceptual, diferencias y principales aplicaciones.

