# Semana 4 — Introducción a Redes Neuronales

## 1. Propósito de la sesión

Comprender los fundamentos de una **red neuronal artificial**, identificando sus principales componentes y explicando conceptualmente cómo una red aprende mediante el ajuste de pesos y sesgos durante el entrenamiento y el proceso de *backpropagation*.

Al finalizar la sesión, el estudiante debería ser capaz de:

* Explicar qué es una red neuronal artificial.
* Identificar los componentes básicos de una neurona artificial.
* Comprender la función de entradas, pesos, sesgo y función de activación.
* Diferenciar las capas de entrada, ocultas y de salida.
* Explicar conceptualmente la propagación hacia adelante (*forward propagation*).
* Comprender el papel de la función de pérdida durante el entrenamiento.
* Explicar conceptualmente el proceso de *backpropagation*.
* Reconocer la función del optimizador en el ajuste de los parámetros.
* Identificar las principales etapas necesarias para entrenar una red neuronal.
* Relacionar estos conceptos con el primer prototipo del proyecto integrador.

---

# 2. ¿Qué es una red neuronal artificial?

Una **red neuronal artificial** es un modelo computacional compuesto por unidades interconectadas denominadas neuronas artificiales.

Estas unidades reciben información, realizan operaciones matemáticas sobre ella y generan una salida.

Una red neuronal puede aprender relaciones complejas entre los datos de entrada y los resultados esperados mediante el ajuste progresivo de sus parámetros.

De manera general:

**Datos de entrada**
↓
**Red neuronal**
↓
**Predicción**

Durante el entrenamiento:

**Datos de entrada + Resultado conocido**
↓
**Red neuronal**
↓
**Predicción**
↓
**Cálculo del error**
↓
**Ajuste de parámetros**

Este proceso se repite numerosas veces hasta obtener un modelo capaz de realizar adecuadamente la tarea para la cual fue entrenado.

---

# 3. De Machine Learning a redes neuronales

Durante las semanas anteriores establecimos el flujo general del aprendizaje supervisado:

**Datos etiquetados → Entrenamiento → Modelo → Predicción**

Una red neuronal constituye una posible implementación del componente **modelo**.

Por ejemplo:

**Imagen**
↓
**Red neuronal**
↓
**Perro / Gato**

La red recibe una representación numérica de la imagen y genera una salida que permite realizar una clasificación.

Durante el entrenamiento conocemos la respuesta correcta, por lo que podemos comparar:

**Predicción del modelo**

con:

**Etiqueta real**

y utilizar esa diferencia para modificar los parámetros internos de la red.

---

# 4. La neurona artificial

La unidad fundamental de una red neuronal es la **neurona artificial**.

Una neurona recibe una o más entradas:

**x₁, x₂, x₃, ..., xₙ**

Cada entrada está asociada con un **peso**:

**w₁, w₂, w₃, ..., wₙ**

La neurona combina esta información y agrega normalmente un término adicional denominado **sesgo o bias**.

Conceptualmente:

**Entradas × Pesos + Sesgo → Función de activación → Salida**

Matemáticamente puede representarse de forma simplificada como:

**z = w₁x₁ + w₂x₂ + ... + wₙxₙ + b**

Posteriormente:

**salida = f(z)**

donde:

* **x** representa las entradas;
* **w** representa los pesos;
* **b** representa el sesgo;
* **f** representa la función de activación.

---

# 5. ¿Qué representan los pesos?

Los **pesos** determinan la influencia que cada entrada tiene sobre el resultado producido por una neurona.

Supongamos una neurona con tres entradas:

**x₁ → w₁**

**x₂ → w₂**

**x₃ → w₃**

Si un peso posee una magnitud mayor, la entrada asociada puede ejercer una influencia diferente sobre el cálculo realizado por la neurona.

Los pesos constituyen algunos de los principales **parámetros aprendidos** por una red neuronal.

Al comenzar el entrenamiento, la red todavía no conoce qué valores deberían tener.

Durante el aprendizaje:

**Pesos iniciales**
↓
**Predicción**
↓
**Cálculo del error**
↓
**Actualización de pesos**
↓
**Nueva predicción**

Por tanto, cuando afirmamos que una red neuronal **aprende**, una parte fundamental de ese aprendizaje corresponde al ajuste de sus pesos.

---

# 6. El sesgo o bias

Además de los pesos, las neuronas suelen incorporar un parámetro denominado **sesgo** o *bias*.

Conceptualmente:

**z = Σ(wᵢxᵢ) + b**

El sesgo permite modificar el resultado de la combinación de entradas y pesos antes de aplicar la función de activación.

Podemos pensar en él como un parámetro adicional que entrega mayor flexibilidad al modelo para aprender relaciones presentes en los datos.

Al igual que los pesos, el sesgo se ajusta durante el entrenamiento.

Por tanto:

**Parámetros aprendidos = pesos + sesgos**

---

# 7. Función de activación

Después de combinar las entradas, pesos y sesgo, la neurona aplica normalmente una **función de activación**.

Conceptualmente:

**Entradas**
↓
**Combinación ponderada**
↓
**Función de activación**
↓
**Salida**

Las funciones de activación permiten introducir comportamientos no lineales en una red.

Esto resulta fundamental porque muchos problemas reales presentan relaciones complejas que no pueden representarse adecuadamente mediante transformaciones exclusivamente lineales.

Existen diferentes funciones de activación.

Entre las más conocidas encontramos:

* ReLU;
* Sigmoid;
* Tanh;
* Softmax.

La elección dependerá de la arquitectura y del problema que queremos resolver.

---

# 8. Función ReLU

Una función ampliamente utilizada en redes neuronales es **ReLU (Rectified Linear Unit)**.

Conceptualmente:

**ReLU(x) = max(0, x)**

Esto significa:

* si el valor es negativo → devuelve 0;
* si el valor es positivo → mantiene el valor.

Por ejemplo:

| Entrada | Salida ReLU |
| ------: | ----------: |
|      -5 |           0 |
|      -1 |           0 |
|       0 |           0 |
|       2 |           2 |
|       7 |           7 |

ReLU se utiliza ampliamente en capas internas de redes neuronales.

Posteriormente volveremos a encontrarla cuando construyamos redes neuronales profundas y CNN.

---

# 9. Sigmoid y Softmax

Existen funciones especialmente útiles en las capas de salida.

### Sigmoid

Produce valores entre **0 y 1**.

Puede utilizarse, por ejemplo, en determinados problemas de clasificación binaria.

Conceptualmente:

**Imagen → Red → 0,92**

El resultado podría interpretarse como una alta probabilidad asociada a determinada clase.

### Softmax

Se utiliza habitualmente cuando existen múltiples clases mutuamente excluyentes.

Por ejemplo:

| Clase    | Resultado |
| -------- | --------: |
| plástico |      0,05 |
| vidrio   |      0,10 |
| cartón   |      0,80 |
| metal    |      0,05 |

La suma de los valores es:

**1,00**

La predicción correspondería a:

**cartón — 80%**

Esto se relaciona directamente con el producto final que desarrollaremos durante el semestre.

---

# 10. De una neurona a una red neuronal

Una sola neurona posee capacidades limitadas.

Las redes neuronales combinan múltiples neuronas organizadas normalmente en **capas**.

Una estructura básica puede representarse como:

**Capa de entrada**
↓
**Capa oculta**
↓
**Capa de salida**

Una red puede contener:

* una capa oculta;
* varias capas ocultas;
* una gran cantidad de capas.

Cuando utilizamos redes con múltiples capas hablamos de arquitecturas más profundas, dando origen al concepto de **Deep Learning**.

---

# 11. Capa de entrada

La **capa de entrada** recibe la información que será procesada por la red.

En un problema tabular podría recibir:

* edad;
* ingresos;
* número de compras;
* antigüedad.

En imágenes, las entradas se relacionan con la representación numérica de los píxeles.

Supongamos una pequeña imagen en escala de grises de:

**28 × 28 píxeles**

Esta imagen contiene:

**28 × 28 = 784 valores**

Una red neuronal básica podría transformar esos valores en un vector que posteriormente será procesado por las capas internas.

Este enfoque será útil como primera aproximación, aunque posteriormente veremos que las **CNN procesan las imágenes de una forma especialmente diseñada para aprovechar su estructura espacial**.

---

# 12. Capas ocultas

Las capas ubicadas entre la entrada y la salida reciben el nombre de **capas ocultas**.

Estas capas realizan transformaciones sucesivas sobre la información.

Conceptualmente:

**Entrada**
↓
**Transformación 1**
↓
**Transformación 2**
↓
**Transformación 3**
↓
**Salida**

Cada capa puede aprender representaciones diferentes de los datos.

La combinación de numerosas transformaciones permite modelar relaciones más complejas.

Esta característica es una de las bases del **Deep Learning**.

---

# 13. Capa de salida

La **capa de salida** produce el resultado final de la red.

Su estructura depende del problema.

### Clasificación binaria

Por ejemplo:

**perro / gato**

podría utilizar una salida que represente la probabilidad de pertenecer a una de las clases.

### Clasificación multiclase

Por ejemplo:

* plástico;
* vidrio;
* cartón;
* metal.

La red podría generar:

**[0,03; 0,08; 0,85; 0,04]**

Interpretación:

**plástico → 3%**
**vidrio → 8%**
**cartón → 85%**
**metal → 4%**

Predicción:

**cartón**

---

# 14. Forward propagation

Cuando una entrada atraviesa la red desde la primera capa hasta producir una salida hablamos de **propagación hacia adelante** o *forward propagation*.

Conceptualmente:

**Entrada**
↓
**Capa 1**
↓
**Capa 2**
↓
**...**
↓
**Capa de salida**
↓
**Predicción**

Durante este proceso, cada capa utiliza sus pesos, sesgos y funciones de activación para transformar la información recibida.

Por ejemplo:

**Imagen**
↓
**Red neuronal**
↓
**Predicción: gato — 72%**

La propagación hacia adelante permite obtener una predicción, pero todavía necesitamos saber si esa predicción fue correcta.

---

# 15. La función de pérdida

Durante el entrenamiento conocemos la etiqueta correcta.

Por tanto, podemos comparar:

**Predicción**

con:

**Resultado esperado**

La **función de pérdida** o *loss function* cuantifica qué tan diferente es la predicción respecto del resultado correcto.

Conceptualmente:

**Predicción + Etiqueta real → Función de pérdida → Error**

Por ejemplo:

**Etiqueta correcta: gato**

**Predicción: perro**

La función de pérdida generará una señal que permitirá determinar que los parámetros actuales de la red deben modificarse.

El objetivo general del entrenamiento será:

**minimizar la función de pérdida**.

---

# 16. ¿Cómo aprende la red del error?

Ya conocemos dos etapas:

### Primera

La red realiza una predicción:

**Entrada → Red → Predicción**

### Segunda

Calculamos el error:

**Predicción + Resultado correcto → Pérdida**

Ahora aparece la pregunta fundamental:

**¿Cómo utilizamos ese error para modificar los pesos y sesgos de la red?**

Aquí aparece uno de los conceptos centrales del entrenamiento de redes neuronales:

**Backpropagation**

o:

**retropropagación del error**.

---

# 17. Backpropagation

**Backpropagation** es el procedimiento mediante el cual la información asociada al error se propaga hacia atrás a través de la red para determinar cómo deben ajustarse sus parámetros.

Podemos visualizar el proceso completo:

### Forward propagation

**Entrada → Capas → Predicción**

↓

### Cálculo de pérdida

**Predicción ↔ Resultado correcto**

↓

### Backpropagation

**Error → capas anteriores → cálculo de ajustes**

↓

### Actualización

**Pesos y sesgos modificados**

Por tanto, la información viaja conceptualmente en dos direcciones:

**Hacia adelante → producir predicción**

**Hacia atrás → aprender del error**

---

# 18. Backpropagation no significa simplemente “enviar el error”

Aunque podemos explicarlo inicialmente como una propagación del error, técnicamente el proceso determina **cómo contribuyó cada parámetro al valor de la función de pérdida**.

Para ello se utilizan derivadas y la regla de la cadena del cálculo diferencial.

No es necesario desarrollar todavía toda la formulación matemática.

La idea fundamental que debe comprenderse es:

**cada peso debe saber en qué dirección y magnitud aproximada debería modificarse para contribuir a reducir la pérdida.**

Backpropagation permite calcular esta información.

Posteriormente un algoritmo de optimización utiliza esos resultados para actualizar los parámetros.

---

# 19. Optimización y descenso de gradiente

Una vez calculada la información necesaria mediante *backpropagation*, debemos actualizar los parámetros.

Una de las ideas fundamentales utilizadas para ello es el **descenso de gradiente**.

Podemos imaginar que la función de pérdida forma una superficie y queremos encontrar una zona donde el error sea menor.

Conceptualmente:

**Error alto**
↓
**Calcular dirección de mejora**
↓
**Modificar parámetros**
↓
**Error menor**

Este proceso se repite durante el entrenamiento.

Existen diferentes algoritmos de optimización, entre ellos:

* SGD;
* Adam;
* RMSprop.

Durante las primeras implementaciones utilizaremos normalmente optimizadores ya disponibles en las bibliotecas de Deep Learning.

---

# 20. Learning rate

Un concepto importante en la actualización de los parámetros es la **tasa de aprendizaje** o *learning rate*.

Esta determina el tamaño de los ajustes realizados durante el entrenamiento.

Conceptualmente:

### Learning rate demasiado grande

Los cambios pueden ser excesivos y dificultar la convergencia hacia una buena solución.

### Learning rate demasiado pequeño

El aprendizaje puede resultar excesivamente lento.

Por tanto, la tasa de aprendizaje constituye un **hiperparámetro** que debe ser definido antes o durante la configuración del entrenamiento.

Esta idea permite introducir una diferencia importante:

**Parámetros → aprendidos por el modelo**

**Hiperparámetros → configurados para controlar el proceso de aprendizaje**

---

# 21. Parámetros versus hiperparámetros

Los conceptos pueden diferenciarse de la siguiente manera:

### Parámetros

Son aprendidos durante el entrenamiento.

Ejemplos:

* pesos;
* sesgos.

### Hiperparámetros

Son decisiones de configuración del proceso o de la arquitectura.

Ejemplos:

* tasa de aprendizaje;
* cantidad de neuronas;
* número de capas;
* cantidad de épocas;
* tamaño del lote.

Durante el desarrollo de un modelo será necesario experimentar con diferentes configuraciones para encontrar una solución adecuada.

---

# 22. Épocas y lotes

Durante el entrenamiento aparecerán dos conceptos adicionales.

### Época (*epoch*)

Una época corresponde, de manera general, a una pasada completa del conjunto de entrenamiento por el proceso de aprendizaje.

Por ejemplo:

**Dataset = 5.000 imágenes**

Después de que las 5.000 imágenes hayan participado en el entrenamiento:

**1 época**

Normalmente entrenamos durante múltiples épocas.

### Lote (*batch*)

En lugar de procesar necesariamente todos los ejemplos simultáneamente, podemos dividirlos en grupos más pequeños denominados *batches*.

Por ejemplo:

**5.000 imágenes**

con:

**batch size = 100**

produce múltiples grupos que serán procesados durante cada época.

---

# 23. El ciclo completo de entrenamiento

Podemos ahora representar el aprendizaje de una red neuronal mediante un ciclo completo:

**1. Seleccionar un conjunto de ejemplos**

↓

**2. Forward propagation**

La red genera predicciones.

↓

**3. Calcular la pérdida**

Se comparan las predicciones con las etiquetas correctas.

↓

**4. Backpropagation**

Se calcula cómo deben modificarse los parámetros.

↓

**5. Optimización**

Se actualizan pesos y sesgos.

↓

**6. Repetir**

El proceso continúa con nuevos lotes y épocas.

En forma resumida:

**Predecir → medir error → retropropagar → ajustar → repetir**

Esta es una de las ideas fundamentales de toda la Unidad 1.

---

# 24. Primera red neuronal en Python

Utilizando TensorFlow/Keras, una red neuronal sencilla puede representarse conceptualmente mediante:

```python
from tensorflow import keras
from tensorflow.keras import layers

modelo = keras.Sequential([
    layers.Input(shape=(784,)),
    layers.Dense(128, activation="relu"),
    layers.Dense(4, activation="softmax")
])
```

Esta estructura contiene:

**Entrada → 784 valores**

↓

**Capa oculta → 128 neuronas + ReLU**

↓

**Salida → 4 clases + Softmax**

Todavía falta indicar cómo será entrenada.

---

# 25. Configuración del entrenamiento

Podemos configurar el proceso indicando elementos como:

```python
modelo.compile(
    optimizer="adam",
    loss="sparse_categorical_crossentropy",
    metrics=["accuracy"]
)
```

Aquí aparecen conceptos estudiados durante esta semana:

**optimizer="adam"**

Determina cómo se actualizarán los parámetros.

**loss="sparse_categorical_crossentropy"**

Define cómo mediremos el error en este problema de clasificación.

**metrics=["accuracy"]**

Permite observar qué proporción de ejemplos está siendo clasificada correctamente.

Posteriormente:

```python
modelo.fit(
    X_train,
    y_train,
    epochs=10
)
```

inicia el entrenamiento.

Conceptualmente:

**Datos → Forward propagation → Loss → Backpropagation → Adam → Actualización**

El framework realiza automáticamente gran parte de estas operaciones.

---

# 26. ¿Por qué una red neuronal básica antes de una CNN?

Nuestro proyecto integrador terminará utilizando una **Red Neuronal Convolucional (CNN)**.

Sin embargo, durante esta primera unidad trabajaremos inicialmente con una red neuronal básica.

Esto permite comprender los componentes comunes:

* neuronas;
* capas;
* pesos;
* sesgos;
* funciones de activación;
* función de pérdida;
* entrenamiento;
* *backpropagation*;
* optimización.

Posteriormente las CNN incorporarán capas especializadas para trabajar con imágenes.

La progresión será:

**Red neuronal básica**
↓
**Comprender el entrenamiento**
↓
**Preparar correctamente las imágenes**
↓
**Red Neuronal Convolucional**
↓
**Clasificador definitivo**

Por tanto, el primer prototipo **no corresponde todavía al modelo final del proyecto**.

---

# 27. Caso conductor: clasificación de residuos

Retomemos nuestro ejemplo de clasificación de residuos.

Las clases son:

* plástico;
* vidrio;
* cartón;
* metal.

Podemos pensar en una primera red:

**Imagen preparada como entrada**
↓
**Capa de entrada**
↓
**Capa oculta + ReLU**
↓
**Capa de salida + Softmax**
↓
**4 probabilidades**

Ejemplo de salida:

**Plástico → 0,06**

**Vidrio → 0,09**

**Cartón → 0,81**

**Metal → 0,04**

Predicción:

**Cartón — 81%**

Durante el entrenamiento, si la etiqueta correcta fuera **cartón**, la red utilizaría esta información para calcular la pérdida y ajustar sus parámetros.

---

# 28. Preguntas para discusión en clase

### Caso 1

Una neurona recibe cinco entradas diferentes.

**Pregunta:** ¿Qué función cumplen los pesos asociados a esas entradas?

### Caso 2

Una red debe clasificar fotografías en cuatro categorías diferentes.

**Pregunta:** ¿Qué función de activación podría resultar apropiada para su capa de salida: ReLU o Softmax? ¿Por qué?

### Caso 3

Una red produce una predicción incorrecta.

**Pregunta:** ¿Qué elementos permiten transformar ese error en modificaciones de los pesos?

### Caso 4

Un modelo utiliza `epochs=20`.

**Pregunta:** ¿Qué significa conceptualmente una época?

### Caso 5

Durante el entrenamiento utilizamos Adam.

**Pregunta:** ¿Adam corresponde a un peso, una función de activación o un optimizador?

### Caso 6

Un usuario carga una fotografía en una aplicación que utiliza un modelo previamente entrenado.

**Pregunta:** ¿Se está ejecutando *backpropagation* o inferencia?

---

# 29. Síntesis de la Semana 4

Al finalizar esta sesión deben quedar instaladas ocho ideas fundamentales:

1. **Una red neuronal está formada por neuronas artificiales organizadas en capas.**
2. **Cada neurona combina entradas, pesos y sesgos y aplica una función de activación.**
3. **Los pesos y sesgos son parámetros que la red aprende durante el entrenamiento.**
4. **La propagación hacia adelante permite transformar una entrada en una predicción.**
5. **La función de pérdida cuantifica la diferencia entre la predicción y el resultado esperado.**
6. **Backpropagation permite determinar cómo deben modificarse los parámetros para reducir la pérdida.**
7. **El optimizador utiliza esta información para actualizar los parámetros durante múltiples lotes y épocas.**
8. **La red neuronal básica constituye una primera aproximación; posteriormente utilizaremos una CNN específicamente diseñada para trabajar con imágenes.**

### Hacia la Semana 5

Con esta sesión cerramos conceptualmente la **Unidad 1: Introducción al Machine Learning**.

La secuencia desarrollada ha sido:

**Semana 1:** ¿Qué es Machine Learning?

**Semana 2:** ¿De qué maneras puede aprender una máquina?

**Semana 3:** ¿Cómo se construye y entrena una solución supervisada?

**Semana 4:** ¿Cómo aprende una red neuronal?

La siguiente etapa del proyecto cambia el foco desde el modelo hacia su materia prima:

**¿Qué datos necesitamos y cómo debemos prepararlos para que un modelo pueda aprender correctamente?**

La **Semana 5** iniciará la Unidad 2: **Recolección y preparación de datos**, abordando tipos de datos, características, etiquetas y requerimientos de un dataset según el problema.

