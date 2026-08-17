# Semana 1 — Introducción al Machine Learning

## 1. Propósito de la sesión

Introducir al estudiante en los fundamentos del Machine Learning, comprendiendo su relación con la Inteligencia Artificial y el Deep Learning, sus principales características y los tipos de problemas que pueden ser abordados mediante modelos capaces de aprender patrones a partir de datos.

Al finalizar la sesión, el estudiante debería ser capaz de:

* Explicar qué se entiende por Inteligencia Artificial, Machine Learning y Deep Learning.
* Diferenciar estos tres conceptos y reconocer la relación existente entre ellos.
* Comprender el principio general mediante el cual un modelo aprende a partir de datos.
* Identificar problemas que pueden abordarse mediante Machine Learning.
* Reconocer la clasificación de imágenes como un problema susceptible de ser resuelto mediante Machine Learning y Deep Learning.
* Comprender, a nivel general, las principales etapas de una solución basada en Machine Learning.

---

# 2. Inteligencia Artificial

La **Inteligencia Artificial (IA)** corresponde al campo de la informática dedicado al desarrollo de sistemas capaces de realizar tareas que normalmente requieren capacidades asociadas a la inteligencia humana.

Estas tareas pueden involucrar, entre otras:

* reconocimiento de imágenes;
* comprensión y generación de lenguaje;
* reconocimiento de voz;
* toma de decisiones;
* resolución de problemas;
* planificación;
* predicción;
* identificación de patrones.

La IA constituye un campo amplio. Por esta razón, no toda solución de Inteligencia Artificial utiliza necesariamente Machine Learning.

Por ejemplo, un sistema puede tomar decisiones utilizando un conjunto de reglas previamente programadas:

**SI temperatura > 30 °C → activar sistema de refrigeración.**

En este caso, el comportamiento fue explícitamente definido por una persona. El sistema ejecuta una regla, pero no aprende dicha regla a partir de datos.

Esta distinción permite introducir una característica fundamental del Machine Learning: **el aprendizaje a partir de ejemplos y datos**.

---

# 3. ¿Qué es Machine Learning?

**Machine Learning (ML)** o aprendizaje automático es un área de la Inteligencia Artificial orientada al desarrollo de algoritmos y modelos capaces de identificar patrones a partir de datos y utilizar esos patrones para realizar predicciones, clasificaciones o tomar determinadas decisiones.

En la programación tradicional, generalmente se construyen explícitamente las reglas que debe seguir el sistema:

**Datos + Reglas → Resultado**

En Machine Learning cambia parcialmente esta lógica. Disponemos de datos y ejemplos de los resultados esperados y utilizamos un algoritmo para encontrar patrones que permitan construir un modelo:

**Datos + Resultados conocidos → Entrenamiento → Modelo**

Posteriormente:

**Datos nuevos + Modelo → Predicción**

Esta diferencia resulta fundamental.

Supongamos que queremos construir un sistema que determine si una fotografía contiene un **perro o un gato**.

Construir manualmente reglas como:

* si tiene orejas de determinada forma, entonces es un gato;
* si el hocico tiene determinada longitud, entonces es un perro;
* si el pelo presenta determinada textura, entonces pertenece a determinada clase;

resultaría extremadamente complejo y poco robusto.

Una estrategia de Machine Learning consiste, en cambio, en proporcionar numerosos ejemplos previamente clasificados:

**imagen → perro**
**imagen → gato**
**imagen → perro**
**imagen → gato**

El modelo utiliza estos ejemplos durante su entrenamiento para identificar patrones que permitan posteriormente clasificar imágenes que nunca había observado.

---

# 4. El concepto de modelo

Un concepto central en Machine Learning es el **modelo**.

Un modelo puede entenderse como una representación matemática que ha aprendido determinadas relaciones o patrones presentes en los datos utilizados durante el entrenamiento.

Podemos representar conceptualmente el proceso como:

**Datos → Algoritmo de aprendizaje → Modelo**

Una vez entrenado:

**Nuevo dato → Modelo → Predicción**

El algoritmo y el modelo no son exactamente lo mismo.

El **algoritmo de aprendizaje** establece el procedimiento mediante el cual se aprende a partir de los datos.

El **modelo entrenado** es el resultado de ese proceso y contiene los parámetros aprendidos.

Esta distinción será especialmente importante posteriormente cuando se entrenen redes neuronales y se almacene el modelo resultante para incorporarlo a una aplicación.

---

# 5. Machine Learning no significa memorizar

El objetivo de un modelo de Machine Learning no consiste simplemente en recordar los ejemplos utilizados durante su entrenamiento.

El objetivo es **aprender patrones suficientemente generales para responder correctamente ante datos nuevos**.

Supongamos que entrenamos un clasificador utilizando 5.000 fotografías de perros y gatos.

El verdadero desafío no consiste en reconocer esas mismas 5.000 imágenes.

El modelo debe ser capaz de recibir posteriormente una fotografía completamente nueva y determinar correctamente a qué categoría pertenece.

Esta capacidad de funcionar adecuadamente ante datos no utilizados durante el entrenamiento se relaciona con un concepto fundamental que será estudiado posteriormente: la **generalización**.

---

# 6. Inteligencia Artificial, Machine Learning y Deep Learning

Los conceptos IA, Machine Learning y Deep Learning suelen utilizarse indistintamente, pero representan niveles diferentes.

Podemos entender su relación mediante una estructura jerárquica:

**Inteligencia Artificial**
↳ **Machine Learning**
 ↳ **Deep Learning**

### Inteligencia Artificial

Es el campo más amplio. Busca desarrollar sistemas capaces de ejecutar tareas asociadas con comportamientos inteligentes.

### Machine Learning

Es un área dentro de la Inteligencia Artificial que utiliza datos para que los sistemas aprendan patrones y puedan generar predicciones o decisiones.

### Deep Learning

Es un área dentro de Machine Learning basada fundamentalmente en **redes neuronales con múltiples capas**.

Por tanto:

**Todo Deep Learning pertenece al Machine Learning, pero no todo Machine Learning es Deep Learning.**

Del mismo modo:

**Machine Learning pertenece al ámbito de la Inteligencia Artificial, pero no toda Inteligencia Artificial utiliza Machine Learning.**

---

# 7. ¿Por qué surge el Deep Learning?

Los algoritmos tradicionales de Machine Learning permiten resolver una gran cantidad de problemas. Sin embargo, determinados tipos de datos presentan una enorme complejidad.

Entre ellos:

* imágenes;
* audio;
* video;
* lenguaje natural.

Una imagen digital puede contener cientos de miles o millones de valores numéricos correspondientes a sus píxeles.

Determinar manualmente cuáles características permiten distinguir un perro de un gato, una célula normal de una anormal o diferentes tipos de productos puede resultar extremadamente complejo.

El **Deep Learning** permite que redes neuronales con múltiples capas aprendan progresivamente representaciones útiles de estos datos.

En imágenes, por ejemplo, diferentes niveles de una red pueden aprender representaciones asociadas con:

**bordes → formas → estructuras → objetos**

Esta capacidad explica la importancia que han adquirido las redes neuronales profundas en problemas relacionados con visión computacional.

---

# 8. Datos: la materia prima del Machine Learning

Machine Learning depende directamente de los datos.

Un dataset puede contener diferentes tipos de información:

* números;
* categorías;
* texto;
* audio;
* imágenes;
* video.

Para nuestro proyecto integrador, el dato fundamental será la **imagen digital**.

Cada imagen deberá estar asociada con información que permita conocer qué representa.

Por ejemplo:

| Imagen         | Clase    |
| -------------- | -------- |
| imagen_001.jpg | plástico |
| imagen_002.jpg | cartón   |
| imagen_003.jpg | vidrio   |
| imagen_004.jpg | plástico |

La columna **clase** representa aquello que posteriormente queremos que el modelo sea capaz de predecir.

Durante las próximas semanas estudiaremos cómo obtener, organizar, etiquetar y preparar correctamente estos datos.

---

# 9. Características y etiquetas

En Machine Learning aparecen dos conceptos fundamentales.

### Características (*features*)

Son las variables o información que utiliza el modelo para aprender.

En datos tabulares podrían corresponder, por ejemplo, a:

* edad;
* ingresos;
* antigüedad;
* número de compras.

En una imagen, la información disponible proviene fundamentalmente de los valores de sus píxeles y de las representaciones que posteriormente puede aprender una red neuronal.

### Etiqueta (*label*)

Es el resultado conocido que queremos que el modelo aprenda a predecir.

Ejemplo:

**Imagen de entrada → Modelo → "Gato"**

En este caso:

* la imagen constituye la entrada;
* **gato** constituye la etiqueta o clase esperada.

La existencia de ejemplos previamente etiquetados será fundamental para el tipo de aprendizaje que utilizaremos en el proyecto integrador.

---

# 10. ¿Qué problemas puede resolver Machine Learning?

Machine Learning puede utilizarse en diferentes tipos de problemas.

### Clasificación

El objetivo consiste en asignar una categoría a una observación.

Ejemplos:

* spam / no spam;
* fraude / no fraude;
* perro / gato;
* producto A / producto B / producto C;
* residuo plástico / vidrio / cartón.

El proyecto integrador de la asignatura pertenece principalmente a esta categoría.

### Regresión

Busca predecir un valor numérico continuo.

Ejemplos:

* precio de una vivienda;
* demanda esperada;
* temperatura;
* ventas futuras.

### Agrupamiento o *clustering*

Busca descubrir grupos de observaciones similares sin disponer necesariamente de categorías previamente definidas.

Ejemplos:

* segmentación de clientes;
* agrupación de documentos;
* identificación de patrones de comportamiento.

### Detección de anomalías

Busca identificar observaciones cuyo comportamiento resulta diferente del patrón habitual.

Ejemplos:

* transacciones potencialmente fraudulentas;
* fallas de maquinaria;
* comportamiento anormal de una red informática.

---

# 11. Machine Learning aplicado a imágenes

El análisis automático de imágenes forma parte del área conocida como **visión computacional**.

Algunos problemas habituales son:

### Clasificación de imágenes

Determinar qué representa una imagen completa.

**Imagen → "automóvil"**

### Detección de objetos

Identificar qué objetos aparecen y dónde están ubicados.

**Imagen → persona + automóvil + bicicleta**

### Segmentación

Determinar qué píxeles pertenecen a determinados objetos o regiones.

### Reconocimiento

Identificar determinados patrones visuales dentro de las imágenes.

Nuestro proyecto integrador se concentrará específicamente en **clasificación de imágenes**.

---

# 12. Flujo general de una solución de Machine Learning

Aunque posteriormente estudiaremos cada etapa en profundidad, una solución típica puede representarse mediante el siguiente ciclo:

**1. Definir el problema**

¿Qué queremos predecir o clasificar?

↓

**2. Obtener los datos**

¿Qué información necesitamos para resolver el problema?

↓

**3. Preparar los datos**

Limpiar, transformar, organizar y etiquetar.

↓

**4. Dividir los datos**

Crear conjuntos para entrenamiento, validación y prueba.

↓

**5. Seleccionar o diseñar el modelo**

Determinar qué arquitectura o algoritmo utilizar.

↓

**6. Entrenar**

El modelo ajusta sus parámetros utilizando los datos disponibles.

↓

**7. Evaluar**

Determinar qué tan correctamente funciona ante datos que no fueron utilizados para aprender.

↓

**8. Mejorar**

Modificar datos, parámetros o arquitectura.

↓

**9. Implementar**

Incorporar el modelo entrenado dentro de una aplicación o servicio.

Este flujo anticipa prácticamente toda la estructura de la asignatura.

---

# 13. Python y Machine Learning

Python se ha convertido en uno de los lenguajes más utilizados para Machine Learning debido, entre otros factores, a su ecosistema de bibliotecas.

* **NumPy:** operaciones numéricas y matrices.
* **Pandas:** manipulación y análisis de datos.
* **Matplotlib:** visualización.
* **Scikit-learn:** algoritmos y herramientas de Machine Learning.
* **TensorFlow/Keras:** construcción y entrenamiento de redes neuronales.

---

# 14. El proyecto integrador

El proyecto recorrerá cuatro grandes etapas:

**Problema → Datos → CNN → Implementación**

### Primera etapa

Definir el problema y desarrollar una primera aproximación mediante una red neuronal.

### Segunda etapa

Construir, preparar, etiquetar y validar el conjunto de imágenes.

### Tercera etapa

Diseñar, entrenar y evaluar una **Red Neuronal Convolucional (CNN)**.

### Cuarta etapa

Optimizar el modelo e incorporarlo dentro de una solución funcional.

El funcionamiento esperado será:

**Usuario carga una imagen → aplicación procesa la imagen → CNN realiza inferencia → aplicación muestra clase predicha + nivel de confianza.**

---

# 15. Ejemplo conductor: clasificación de residuos

Consideremos una organización que desea automatizar parcialmente la clasificación de residuos.

El problema consiste en reconocer cuatro categorías:

* plástico;
* vidrio;
* cartón;
* metal.

Disponemos de fotografías previamente clasificadas.

El flujo podría ser:

**Fotografías etiquetadas**
↓
**Preparación del dataset**
↓
**Entrenamiento del modelo**
↓
**Evaluación**
↓
**Modelo entrenado**
↓
**Nueva fotografía**
↓
**Predicción: "Plástico — 94%"**

Este ejemplo permite observar que el modelo no recibe una regla explícita que describa exactamente cómo luce cada material. Aprende patrones a partir de los ejemplos proporcionados.

Esta misma lógica podrá aplicarse a distintos problemas seleccionados por las duplas.

---

# 16. Preguntas para discusión en clase

Para conectar los conceptos con problemas reales considere:

**Caso 1:** Una empresa quiere predecir el precio de venta de viviendas.
¿Qué debería producir el modelo: una categoría o un valor numérico?

**Caso 2:** Un hospital dispone de imágenes clasificadas en diferentes categorías y desea clasificar automáticamente nuevas imágenes.
¿Qué tipo general de problema representa?

**Caso 3:** Una empresa dispone de información sobre miles de clientes, pero no tiene segmentos previamente definidos y quiere descubrir grupos con comportamientos similares.
¿Estamos frente al mismo tipo de problema que la clasificación de imágenes?

**Caso 4:** Queremos construir un clasificador de cinco especies de plantas.
¿Qué datos deberíamos recopilar y qué información tendría que acompañar cada imagen?

---

# 17. Síntesis de la Semana 1

Al finalizar esta primera sesión deben quedar instaladas seis ideas fundamentales:

1. **La Inteligencia Artificial es el campo general; Machine Learning es una de sus áreas y Deep Learning es una especialización dentro de Machine Learning.**
2. **Machine Learning permite construir modelos que aprenden patrones a partir de datos.**
3. **Entrenar y utilizar un modelo son procesos diferentes:** primero se aprende a partir de ejemplos y posteriormente se realiza inferencia sobre datos nuevos.
4. **El objetivo no es memorizar los datos de entrenamiento, sino aprender patrones que permitan trabajar con información nueva.**
5. **Clasificación, regresión, agrupamiento y detección de anomalías representan diferentes problemas abordables mediante Machine Learning.**
6. **Durante el semestre aplicaremos estos conceptos desarrollando progresivamente un clasificador de imágenes cuyo núcleo final será una CNN.**

### Hacia la Semana 2

La primera semana responde principalmente a la pregunta:

**¿Qué es Machine Learning y qué tipos de problemas puede resolver?**

La siguiente deberá responder:

**¿De qué maneras puede aprender una máquina?**

Esto permitirá introducir formalmente tres paradigmas: **aprendizaje supervisado, aprendizaje no supervisado y aprendizaje por refuerzo**. 

