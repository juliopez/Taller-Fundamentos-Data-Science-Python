# Semana 12 — Autoencoders y Redes Generativas Adversarias (GAN)

## 1. Propósito de la sesión

Comprender dos arquitecturas relevantes de Deep Learning orientadas a objetivos diferentes de la clasificación tradicional: los **Autoencoders**, utilizados para aprender representaciones comprimidas de los datos, y las **Redes Generativas Adversarias (GAN)**, utilizadas para generar nuevos ejemplos a partir de patrones aprendidos.

Al finalizar la sesión, el estudiante debería ser capaz de:

* Explicar qué es un autoencoder y cuál es su objetivo principal.
* Identificar las funciones del encoder, espacio latente y decoder.
* Comprender el concepto de representación comprimida.
* Reconocer aplicaciones de autoencoders en reducción de dimensionalidad, reconstrucción y detección de anomalías.
* Explicar qué es una GAN.
* Identificar los componentes generador y discriminador.
* Comprender la lógica de entrenamiento adversarial.
* Reconocer aplicaciones de GAN en generación y aumento sintético de datos.
* Diferenciar autoencoders, GAN, CNN y LSTM según su objetivo.
* Analizar cuándo estas arquitecturas podrían complementar un proyecto de clasificación de imágenes.

---

# 2. No todas las redes neuronales buscan clasificar

Hasta ahora hemos trabajado principalmente con modelos cuyo objetivo es producir una predicción.

Por ejemplo:

**Imagen → CNN → Clase**

o:

**Secuencia → LSTM → Predicción**

Sin embargo, Deep Learning también permite construir modelos cuyo objetivo puede ser:

* aprender una representación interna;
* reconstruir una entrada;
* reducir dimensionalidad;
* detectar comportamientos anómalos;
* generar datos nuevos.

Dos arquitecturas importantes para comprender estas capacidades son:

**Autoencoder**

y:

**GAN**

Aunque ambas trabajan con representaciones aprendidas, sus objetivos son muy diferentes.

---

# 3. ¿Qué es un Autoencoder?

Un **Autoencoder** es una red neuronal entrenada para reconstruir su propia entrada.

Conceptualmente:

**Entrada**

↓

**Compresión**

↓

**Representación interna**

↓

**Reconstrucción**

↓

**Salida aproximada a la entrada**

Por ejemplo:

**Imagen original**

↓

**Autoencoder**

↓

**Imagen reconstruida**

El objetivo es que la salida sea lo más parecida posible a la entrada.

---

# 4. Una tarea aparentemente trivial

A primera vista podría parecer extraño entrenar una red para producir:

**entrada → misma entrada**

Por ejemplo:

**imagen de gato → imagen de gato**

Sin embargo, introducimos una restricción importante.

La red debe pasar la información a través de una **representación más compacta**.

Conceptualmente:

**Imagen de alta dimensión**

↓

**Representación comprimida**

↓

**Reconstrucción**

Si la red consigue reconstruir adecuadamente la imagen, significa que la representación interna conserva información relevante.

---

# 5. Arquitectura de un Autoencoder

Un autoencoder posee tres componentes principales:

**Encoder**

↓

**Espacio latente**

↓

**Decoder**

Podemos representarlo así:

**Entrada x**

↓

**Encoder**

↓

**z**

↓

**Decoder**

↓

**Reconstrucción x̂**

donde:

* **x** = entrada original;
* **z** = representación latente;
* **x̂** = reconstrucción.

El entrenamiento busca que:

**x̂ ≈ x**

---

# 6. Encoder

El **encoder** transforma la entrada original en una representación de menor dimensión.

Por ejemplo:

**Imagen 128 × 128 × 3**

↓

**Encoder**

↓

**Vector de 128 valores**

Conceptualmente, el encoder intenta responder:

**¿Qué información necesito conservar para poder reconstruir posteriormente esta entrada?**

Por tanto:

**alta dimensión → representación compacta**

---

# 7. Espacio latente

La representación comprimida recibe habitualmente el nombre de:

**espacio latente**

o:

**latent space**

Este espacio contiene características aprendidas por el modelo.

Por ejemplo:

```text
Imagen
↓
Encoder
↓
[0.12, -0.84, 1.20, 0.31, ...]
```

Estos valores no corresponden necesariamente a conceptos definidos manualmente.

La red aprende una representación útil para reconstruir los datos.

---

# 8. Decoder

El **decoder** realiza el proceso inverso.

Recibe:

**representación latente**

y busca reconstruir:

**entrada original**

Conceptualmente:

**z**

↓

**Decoder**

↓

**x̂**

Por tanto:

**Encoder → comprime**

**Decoder → reconstruye**

El autoencoder completo aprende ambos procesos simultáneamente.

---

# 9. Función de pérdida de un Autoencoder

Durante el entrenamiento conocemos:

**entrada original**

y:

**salida reconstruida**

Podemos comparar ambas.

Conceptualmente:

**Imagen original**

*

**Imagen reconstruida**

↓

**Loss de reconstrucción**

↓

**Backpropagation**

Una función posible es medir cuánto difieren los píxeles reconstruidos respecto de los originales.

El entrenamiento busca:

**minimizar el error de reconstrucción.**

---

# 10. Ejemplo conceptual

Supongamos una imagen pequeña:

```text
Entrada:
[0.1, 0.4, 0.8, 0.2]
```

El encoder genera:

```text
Latente:
[0.52, -0.16]
```

El decoder reconstruye:

```text
Salida:
[0.12, 0.38, 0.76, 0.23]
```

La salida no es exactamente igual, pero es similar.

El error entre ambos vectores permite ajustar los parámetros.

---

# 11. Compresión con pérdida

El espacio latente posee normalmente menos información explícita que la entrada.

Por ello, la reconstrucción puede no ser perfecta.

Esto obliga al modelo a priorizar patrones relevantes.

Podemos representar:

**Datos originales**

↓

**Eliminar redundancia**

↓

**Mantener información importante**

↓

**Representación compacta**

Este principio hace que los autoencoders resulten útiles para aprender características.

---

# 12. Autoencoder denso

Un autoencoder sencillo podría construirse con capas densas.

Por ejemplo:

```python
from tensorflow import keras
from tensorflow.keras import layers

autoencoder = keras.Sequential([
    layers.Input(shape=(784,)),
    layers.Dense(128, activation="relu"),
    layers.Dense(32, activation="relu"),
    layers.Dense(128, activation="relu"),
    layers.Dense(784, activation="sigmoid")
])
```

Conceptualmente:

```text
784
↓
128
↓
32    ← espacio latente
↓
128
↓
784
```

La parte descendente corresponde al encoder y la ascendente al decoder.

---

# 13. Autoencoder convolucional

Cuando trabajamos con imágenes podemos utilizar capas convolucionales.

Conceptualmente:

**Imagen**

↓

**Conv + Pooling**

↓

**Conv + Pooling**

↓

**Representación latente**

↓

**Reconstrucción progresiva**

↓

**Imagen reconstruida**

Esto permite aprovechar la estructura espacial de la imagen.

Por tanto:

**CNN y Autoencoder no son conceptos excluyentes.**

Podemos construir:

**Autoencoders convolucionales.**

---

# 14. Aplicación: reducción de dimensionalidad

Una aplicación de los autoencoders es aprender representaciones de menor dimensión.

Supongamos:

**Imagen = 150.000 valores**

El encoder podría transformarla en:

**128 valores**

Estos 128 valores pueden utilizarse como una representación compacta.

Esto puede servir para:

* visualización;
* clustering;
* búsqueda de similitud;
* entrada de otros modelos.

La lógica es similar a técnicas tradicionales de reducción de dimensionalidad, pero utilizando una red neuronal capaz de aprender transformaciones no lineales.

---

# 15. Aplicación: eliminación de ruido

Podemos entrenar un autoencoder para recibir una entrada alterada y reconstruir una versión limpia.

Por ejemplo:

**Imagen con ruido**

↓

**Autoencoder**

↓

**Imagen limpia**

Durante entrenamiento:

**Entrada → imagen con ruido**

**Objetivo → imagen original**

Este tipo de arquitectura recibe el nombre de:

**Denoising Autoencoder**

o autoencoder de eliminación de ruido.

---

# 16. Aplicación: detección de anomalías

Los autoencoders también pueden utilizarse para detectar observaciones anómalas.

Supongamos que entrenamos exclusivamente con imágenes consideradas normales.

El modelo aprende a reconstruir adecuadamente ese tipo de ejemplos.

Entonces:

**Imagen normal → bajo error de reconstrucción**

Pero ante:

**Imagen muy diferente → alto error de reconstrucción**

podemos utilizar ese error como señal de anomalía.

Conceptualmente:

**Error pequeño → comportamiento conocido**

**Error grande → posible anomalía**

---

# 17. Ejemplo de anomalías

Supongamos una fábrica que produce piezas metálicas.

Entrenamos un autoencoder utilizando:

**10.000 imágenes de piezas correctas**

Posteriormente:

**pieza correcta**

↓

**reconstrucción buena**

↓

**error bajo**

Una pieza con defecto:

**pieza defectuosa**

↓

**reconstrucción deficiente**

↓

**error alto**

Esto permite identificar ejemplos que se alejan del patrón aprendido.

---

# 18. Limitaciones de los Autoencoders

Un autoencoder no produce automáticamente una representación perfecta.

Puede presentar problemas como:

* memorizar los datos;
* aprender características poco útiles;
* reconstrucciones borrosas;
* sensibilidad a la arquitectura;
* necesidad de regularización.

Si la red posee demasiada capacidad, podría aprender simplemente:

**entrada → copia**

sin desarrollar una representación realmente útil.

Por ello, la arquitectura y el tamaño del espacio latente son decisiones importantes.

---

# 19. ¿Qué es una GAN?

Una **GAN** o **Generative Adversarial Network** es una arquitectura generativa compuesta por dos redes neuronales que compiten durante el entrenamiento.

Estas dos redes son:

**Generador**

y:

**Discriminador**

El objetivo general es que el generador aprenda a producir ejemplos artificiales que se parezcan cada vez más a los datos reales.

Conceptualmente:

**Ruido aleatorio → Generador → Imagen sintética**

---

# 20. El Generador

El **generador** recibe normalmente una entrada aleatoria.

Por ejemplo:

```text
z = vector de ruido
```

y produce:

**imagen sintética**

Conceptualmente:

**Ruido**

↓

**Generador**

↓

**Imagen falsa**

Al comienzo del entrenamiento, estas imágenes pueden ser completamente irreconocibles.

Con el tiempo, el generador aprende a producir ejemplos cada vez más parecidos a los reales.

---

# 21. El Discriminador

El **discriminador** recibe una imagen y debe decidir:

**¿Es real o fue generada?**

Por ejemplo:

**Imagen real → 1**

**Imagen sintética → 0**

Conceptualmente:

**Imagen**

↓

**Discriminador**

↓

**Real / Falsa**

El discriminador funciona inicialmente como un clasificador binario.

---

# 22. Competencia adversarial

La característica principal de una GAN es que ambas redes poseen objetivos opuestos.

### Discriminador

Quiere distinguir correctamente:

**real vs falsa**

### Generador

Quiere engañar al discriminador.

Por tanto:

**Generador produce una falsificación**

↓

**Discriminador intenta detectarla**

↓

**Generador aprende de sus errores**

↓

**produce una falsificación mejor**

Este proceso recibe el nombre de:

**entrenamiento adversarial.**

---

# 23. Analogía conceptual

Podemos pensar en:

**Generador = falsificador**

**Discriminador = investigador**

Al comienzo:

**falsificaciones muy malas**

↓

**investigador las detecta fácilmente**

El falsificador mejora.

Después:

**falsificaciones mejores**

↓

**investigador debe mejorar**

Ambas redes progresan simultáneamente.

Idealmente, al final:

**las imágenes generadas son suficientemente realistas como para resultar difíciles de distinguir de las reales.**

---

# 24. Flujo de entrenamiento de una GAN

Una representación simplificada es:

**Datos reales**

→ Discriminador

y simultáneamente:

**Ruido aleatorio**

↓

**Generador**

↓

**Datos sintéticos**

↓

**Discriminador**

El discriminador aprende:

**real / falso**

El generador aprende:

**producir ejemplos que el discriminador clasifique como reales**

Este proceso se repite muchas veces.

---

# 25. Entrenamiento del Discriminador

Durante una etapa podemos entrenar el discriminador con:

### Ejemplos reales

Etiqueta:

**1**

### Ejemplos generados

Etiqueta:

**0**

Conceptualmente:

```text
Imagen real → Discriminador → debería responder REAL
Imagen falsa → Discriminador → debería responder FALSA
```

El discriminador aprende como un clasificador.

---

# 26. Entrenamiento del Generador

Posteriormente queremos mejorar el generador.

Generamos:

**imagen falsa**

y la entregamos al discriminador.

Pero ahora buscamos que el discriminador responda:

**REAL**

Por tanto, el error obtenido se propaga hacia el generador.

Conceptualmente:

**Ruido**

↓

**Generador**

↓

**Imagen falsa**

↓

**Discriminador**

↓

**“falsa”**

↓

**Error**

↓

**Actualizar Generador**

---

# 27. El equilibrio ideal

En un escenario ideal, el generador produce ejemplos tan realistas que el discriminador tiene dificultades para distinguirlos.

Conceptualmente:

**P(real) ≈ 0,5**

para datos generados y reales.

Esto significaría que el discriminador ya no puede identificar fácilmente cuáles ejemplos fueron generados.

En la práctica, entrenar GAN puede resultar inestable y alcanzar este equilibrio no es trivial.

---

# 28. Aplicaciones de GAN

Las GAN han sido utilizadas para:

* generación de rostros artificiales;
* creación de imágenes;
* superresolución;
* restauración de imágenes;
* transformación de estilos;
* síntesis de datos;
* generación de escenarios;
* modificación de atributos visuales.

También pueden utilizarse para complementar determinados datasets.

Sin embargo, los datos generados deben evaluarse cuidadosamente antes de utilizarlos.

---

# 29. GAN y aumento de datos

Supongamos que tenemos pocas imágenes de una categoría.

Podríamos entrenar una GAN para generar ejemplos adicionales.

Conceptualmente:

**Datos reales limitados**

↓

**GAN**

↓

**Imágenes sintéticas**

↓

**Dataset ampliado**

Sin embargo:

**generar imágenes sintéticas no equivale automáticamente a obtener nuevos datos reales.**

La utilidad dependerá de:

* calidad de las imágenes;
* diversidad;
* realismo;
* ausencia de artefactos;
* similitud con la distribución real.

---

# 30. Data Augmentation versus GAN

Durante la Semana 8 estudiamos:

**Data augmentation**

Por ejemplo:

**imagen real → rotación / zoom / flip**

GAN utiliza una lógica diferente.

### Data augmentation

Transforma imágenes reales existentes.

### GAN

Genera nuevos ejemplos a partir de una representación aprendida de la distribución.

Conceptualmente:

**Augmentation**

```text
Imagen existente
↓
Transformación
↓
Variante
```

**GAN**

```text
Ruido
↓
Generador
↓
Nueva imagen sintética
```

---

# 31. Ventajas potenciales de las GAN

Entre sus posibilidades encontramos:

* generar nuevas muestras;
* ampliar datasets escasos;
* producir variaciones complejas;
* explorar distribuciones aprendidas.

Sin embargo, también presentan dificultades importantes.

Entre ellas:

* entrenamiento inestable;
* elevado costo;
* dificultad de evaluación;
* generación de artefactos;
* reproducción insuficiente de diversidad.

Por tanto:

**GAN no debe entenderse como una solución automática para falta de datos.**

---

# 32. Mode Collapse

Un problema conocido en GAN es:

**mode collapse**

Ocurre cuando el generador produce una variedad limitada de ejemplos.

Supongamos que el dataset contiene:

* diferentes tipos;
* colores;
* posiciones;
* tamaños.

Pero el generador aprende solamente a producir:

**un pequeño subconjunto de imágenes similares.**

Visualmente pueden ser buenas, pero no representan toda la diversidad del dataset.

Esto limita la utilidad del modelo generativo.

---

# 33. Calidad versus diversidad

Un generador debería producir:

**imágenes realistas**

y también:

**imágenes diversas.**

Podemos tener:

### Caso A

Imágenes de excelente calidad pero prácticamente idénticas.

Problema:

**baja diversidad**

### Caso B

Gran variedad pero imágenes poco realistas.

Problema:

**baja calidad**

Una buena GAN debería buscar un equilibrio entre ambas propiedades.

---

# 34. GAN y sesgos del dataset

Una GAN aprende a partir de los datos disponibles.

Si el dataset contiene sesgos:

**el generador puede reproducirlos.**

Por ejemplo, si determinadas condiciones visuales están subrepresentadas, la GAN también puede generar pocas muestras de esas condiciones.

Por tanto:

**los datos sintéticos heredan las limitaciones de los datos utilizados para entrenarlos.**

Esto conecta directamente con los conceptos de calidad y representatividad estudiados en UA2.

---

# 35. Arquitectura básica de un Generador

Un generador de imágenes puede comenzar desde un vector:

```text
z = 100 valores aleatorios
```

y transformarlo progresivamente:

```text
100
↓
Dense
↓
Representación espacial pequeña
↓
Upsampling
↓
Imagen
```

Conceptualmente:

**representación compacta → imagen de alta dimensión**

Esto recuerda al decoder de un autoencoder.

Sin embargo, el generador no recibe una imagen real para reconstruir.

Recibe:

**ruido aleatorio**

---

# 36. Arquitectura básica de un Discriminador

El discriminador puede parecerse a una CNN de clasificación.

Conceptualmente:

**Imagen**

↓

**Convoluciones**

↓

**Pooling / reducción**

↓

**Dense**

↓

**Sigmoid**

↓

**Real / Falsa**

Por tanto, conocimientos adquiridos durante la Semana 10 son directamente útiles para comprender el discriminador.

---

# 37. Autoencoder y GAN: una diferencia fundamental

Aunque ambas arquitecturas pueden generar una imagen en su salida, sus objetivos son diferentes.

### Autoencoder

Recibe:

**una entrada real**

y busca:

**reconstruir esa misma entrada**

### GAN

El generador recibe:

**ruido**

y busca:

**crear un ejemplo nuevo que parezca real**

Por tanto:

**Autoencoder → reconstrucción**

**GAN → generación**

---

# 38. Comparación general de arquitecturas

| Arquitectura    | Entrada típica                             | Objetivo principal                    | Ejemplo                   |
| --------------- | ------------------------------------------ | ------------------------------------- | ------------------------- |
| **CNN**         | Imagen                                     | Clasificación / análisis espacial     | Clasificar residuos       |
| **LSTM**        | Secuencia                                  | Modelar dependencias temporales       | Analizar texto            |
| **Autoencoder** | Datos                                      | Reconstruir / aprender representación | Detectar anomalías        |
| **GAN**         | Ruido + datos reales durante entrenamiento | Generar muestras                      | Crear imágenes sintéticas |

Esto permite ampliar nuestro mapa de Deep Learning.

---

# 39. ¿Autoencoder es aprendizaje supervisado?

En un autoencoder convencional no necesitamos etiquetas de clase.

La propia entrada actúa como objetivo.

Por ejemplo:

**X = imagen**

**y = misma imagen**

Por ello suele relacionarse con formas de:

**aprendizaje no supervisado**

o:

**auto-supervisado**

dependiendo de la formulación.

Lo importante para esta sesión es comprender que:

**no requiere necesariamente etiquetas como perro/gato/plástico/vidrio.**

---

# 40. ¿GAN es aprendizaje supervisado?

Una GAN clásica tampoco necesita necesariamente etiquetas de clase.

El generador aprende a partir de la distribución de los datos reales y la señal proporcionada por el discriminador.

Sin embargo, existen variantes como:

**Conditional GAN**

donde podemos condicionar la generación utilizando clases.

Por ejemplo:

**Generar → “gato”**

o:

**Generar → “perro”**

Esto permite controlar parcialmente qué tipo de ejemplo queremos obtener.

---

# 41. Conditional GAN

Conceptualmente:

**Ruido + Clase**

↓

**Generador**

↓

**Imagen de esa categoría**

Por ejemplo:

```text
Ruido + "cartón"
↓
Generador
↓
Imagen sintética de cartón
```

Esto podría resultar útil cuando queremos generar ejemplos de categorías específicas.

Sin embargo, su implementación es más compleja que la de una GAN básica.

---

# 42. Autoencoders y clasificación

Aunque un autoencoder no es principalmente un clasificador, su encoder puede utilizarse para aprender características.

Por ejemplo:

**Imagen**

↓

**Encoder**

↓

**Representación latente**

↓

**Clasificador**

Esto significa que las representaciones aprendidas pueden utilizarse como entrada para otros modelos.

Por tanto, una arquitectura diseñada originalmente para reconstrucción puede tener aplicaciones complementarias.

---

# 43. GAN y clasificación de imágenes

Una GAN tampoco reemplaza directamente nuestra CNN.

Nuestro producto final continúa siendo:

**Imagen → CNN → Clase**

Pero una GAN podría desempeñar un papel complementario:

**Generar imágenes adicionales**

↓

**Mejorar dataset**

↓

**Entrenar CNN**

Conceptualmente:

**GAN → datos**

**CNN → clasificación**

Son funciones distintas dentro del pipeline.

---

# 44. ¿Utilizaremos GAN en el proyecto integrador?

No será obligatorio incorporar una GAN al proyecto.

El descriptor exige conocerla como arquitectura Deep Learning, pero nuestro proyecto tiene como núcleo la **clasificación de imágenes mediante CNN**. 

Incorporar una GAN completa aumentaría considerablemente:

* complejidad;
* tiempo de entrenamiento;
* recursos;
* dificultad de evaluación.

Por tanto, en el proyecto integrador las GAN deben comprenderse principalmente como una **arquitectura complementaria posible**, no como un requisito de la solución final.

---

# 45. ¿Utilizaremos Autoencoders en el proyecto?

Tampoco serán un componente obligatorio del clasificador.

Sin embargo, podrían utilizarse en situaciones específicas, por ejemplo:

* detección de imágenes anómalas;
* reducción de dimensionalidad;
* aprendizaje de representaciones;
* exploración del dataset.

---

# 46. Seleccionar arquitectura según objetivo

Consideremos cuatro preguntas.

### ¿Quiero clasificar una imagen?

**CNN**

### ¿Quiero analizar una secuencia?

**LSTM**

### ¿Quiero reconstruir o comprimir datos?

**Autoencoder**

### ¿Quiero generar nuevos ejemplos?

**GAN**

Esta relación no constituye una regla absoluta, pero ofrece una primera orientación útil.

La arquitectura debe seleccionarse según:

**dato + objetivo + restricciones**

---

# 47. Ejemplo conductor: residuos

Supongamos nuestro clasificador:

**plástico / vidrio / cartón / metal**

### CNN

Objetivo:

**clasificar una fotografía**

### Autoencoder

Posible aplicación:

**identificar fotografías muy diferentes al conjunto habitual**

### GAN

Posible aplicación:

**generar nuevas imágenes sintéticas de determinadas clases**

Por tanto, el mismo dominio puede utilizar arquitecturas diferentes para objetivos distintos.

---

# 48. Calidad de datos sintéticos

Antes de incorporar datos generados a un dataset deberíamos evaluar:

* realismo;
* diversidad;
* consistencia con la clase;
* artefactos;
* similitud excesiva con datos originales;
* posible impacto sobre el clasificador.

No basta con afirmar:

> “La imagen fue generada por una GAN, por tanto sirve para entrenar.”

Debe existir una revisión crítica.

Los datos sintéticos también son **datos que deben validarse**.

---

# 49. Generación y ética

Los modelos generativos introducen además consideraciones importantes.

Una GAN puede producir imágenes muy realistas.

Esto implica riesgos asociados con:

* falsificación;
* manipulación visual;
* desinformación;
* uso de datos protegidos;
* reproducción de sesgos.

Por tanto, comprender una arquitectura generativa implica también reconocer que:

**su capacidad técnica debe utilizarse de manera responsable.**

---

# 50. Entrenamiento y recursos

El entrenamiento de GAN puede ser considerablemente más exigente que entrenar un clasificador sencillo.

Debemos entrenar:

**Generador**

y:

**Discriminador**

manteniendo cierto equilibrio.

Si uno se vuelve demasiado fuerte:

* el otro puede dejar de aprender;
* el entrenamiento puede volverse inestable.

Esto explica por qué las GAN constituyen una arquitectura técnicamente desafiante.

---

# 51. Autoencoder con Keras: estructura conceptual

Podemos definir encoder y decoder:

```python
encoder = keras.Sequential([
    layers.Input(shape=(784,)),
    layers.Dense(128, activation="relu"),
    layers.Dense(32, activation="relu")
])

decoder = keras.Sequential([
    layers.Input(shape=(32,)),
    layers.Dense(128, activation="relu"),
    layers.Dense(784, activation="sigmoid")
])
```

Conceptualmente:

**784 → 128 → 32**

y:

**32 → 128 → 784**

El espacio:

**32**

representa la codificación latente.

---

# 52. Entrenar un Autoencoder

El modelo completo recibe como entrada y objetivo los mismos datos.

Conceptualmente:

```python
autoencoder.fit(
    X_train,
    X_train
)
```

Esto expresa:

**Entrada = X_train**

**Objetivo = X_train**

La red aprende a reconstruir los ejemplos.

Este comportamiento es muy diferente del clasificador:

```python
modelo.fit(
    X_train,
    y_train
)
```

donde:

**y_train**

contiene las clases.

---

# 53. Pseudocódigo conceptual de GAN

El entrenamiento puede resumirse así:

```text
REPETIR:

1. Tomar imágenes reales.

2. Generar imágenes falsas.

3. Entrenar discriminador:
   real → 1
   falsa → 0

4. Generar nuevas imágenes falsas.

5. Entrenar generador para que:
   discriminador → 1
```

Este ciclo muestra la naturaleza adversarial del proceso.

---

# 54. ¿Cuál de las dos redes gana?

La finalidad no es que exista un ganador definitivo.

Si el discriminador se vuelve perfecto:

**detecta siempre las falsificaciones**

el generador puede tener dificultades para aprender.

Si el generador engaña inmediatamente al discriminador:

el discriminador no ofrece una señal suficientemente útil.

El entrenamiento requiere una competencia dinámica.

El objetivo es que:

**ambas redes mejoren progresivamente.**

---

# 55. Evaluar modelos generativos

Evaluar un clasificador es relativamente directo:

* accuracy;
* precision;
* recall;
* F1;
* matriz de confusión.

Evaluar un generador es más complejo.

Debemos considerar:

* realismo;
* diversidad;
* cobertura de la distribución;
* calidad perceptual.

Existen métricas específicas, pero ninguna reemplaza completamente el análisis del resultado.

Esto hace que los modelos generativos sean más difíciles de evaluar que un clasificador convencional.

---

# 56. Preguntas para discusión en clase

### Caso 1

Una empresa quiere reducir una imagen de alta dimensión a una representación compacta que permita posteriormente reconstruirla.

**Pregunta:** ¿Qué arquitectura parece apropiada?

### Caso 2

Entrenamos un modelo solamente con imágenes normales y queremos identificar ejemplos que producen un error de reconstrucción anormalmente alto.

**Pregunta:** ¿Qué aplicación de autoencoders estamos utilizando?

### Caso 3

Una red recibe un vector aleatorio y genera una imagen.

**Pregunta:** ¿Corresponde probablemente al encoder, decoder, generador o discriminador?

### Caso 4

Una red recibe imágenes reales y sintéticas e intenta determinar cuáles son verdaderas.

**Pregunta:** ¿Qué componente de una GAN representa?

### Caso 5

Una dupla duplica imágenes rotándolas algunos grados.

Otra utiliza una GAN para producir muestras sintéticas.

**Pregunta:** ¿Por qué ambos procesos no corresponden exactamente al mismo tipo de aumento de datos?

### Caso 6

Una GAN genera 2.000 fotografías, pero todas son muy similares.

**Pregunta:** ¿Qué problema podría estar ocurriendo?

### Caso 7

Queremos desarrollar exclusivamente el clasificador de imágenes requerido por el proyecto integrador.

**Pregunta:** ¿Es necesario reemplazar nuestra CNN por una GAN?

---

# 57. Síntesis de la Semana 12

Al finalizar esta sesión deben quedar instaladas diez ideas fundamentales:

1. **Deep Learning no se limita a clasificación; también puede aprender representaciones y generar datos.**
2. **Un Autoencoder intenta reconstruir su propia entrada pasando por una representación latente.**
3. **El encoder comprime la información y el decoder intenta reconstruirla.**
4. **Los Autoencoders pueden utilizarse para reducción de dimensionalidad, eliminación de ruido y detección de anomalías.**
5. **Una GAN está formada principalmente por un generador y un discriminador.**
6. **El generador produce ejemplos sintéticos y el discriminador intenta distinguirlos de los reales.**
7. **El aprendizaje adversarial permite que ambas redes mejoren mediante objetivos contrapuestos.**
8. **Data augmentation transforma observaciones existentes, mientras que una GAN puede generar nuevas muestras sintéticas.**
9. **CNN, LSTM, Autoencoder y GAN responden a objetivos y estructuras diferentes; ninguna arquitectura es universalmente superior.**
10. **El proyecto integrador continuará utilizando CNN como núcleo, mientras Autoencoders y GAN amplían el panorama de soluciones disponibles en Deep Learning.**

### Hacia la Semana 13

Durante la Unidad 3 hemos recorrido:

**Semana 9:** fundamentos de Deep Learning.

**Semana 10:** CNN para clasificación de imágenes.

**Semana 11:** LSTM para datos secuenciales.

**Semana 12:** Autoencoders para aprender representaciones y GAN para generar datos.

Nos queda ahora una pregunta especialmente relevante para el proyecto:

**¿Es necesario entrenar siempre una CNN completamente desde cero?**

La **Semana 13** abordará **Transfer Learning y Fine-Tuning**, utilizando modelos previamente entrenados como punto de partida para resolver nuevos problemas de clasificación de imágenes.

