# Semana 2 — Tipos de aprendizaje automático

## 1. Propósito de la sesión

Comprender las principales formas mediante las cuales un sistema de Machine Learning puede aprender a partir de datos o de su interacción con un entorno, diferenciando el **aprendizaje supervisado, el aprendizaje no supervisado y el aprendizaje por refuerzo**.

Al finalizar la sesión, el estudiante debería ser capaz de:

* Explicar las características fundamentales del aprendizaje supervisado, no supervisado y por refuerzo.
* Diferenciar estos paradigmas según los datos disponibles y el objetivo del problema.
* Identificar el papel que cumplen las etiquetas en el aprendizaje supervisado.
* Reconocer problemas de clasificación y regresión como aplicaciones habituales del aprendizaje supervisado.
* Reconocer el agrupamiento como una aplicación característica del aprendizaje no supervisado.
* Comprender los conceptos básicos de agente, entorno, acción y recompensa en aprendizaje por refuerzo.
* Asociar diferentes problemas reales con el paradigma de aprendizaje automático más apropiado.
* Identificar la clasificación de imágenes del proyecto integrador como un problema de aprendizaje supervisado.

---

# 2. ¿De qué maneras puede aprender una máquina?

Durante la Semana 1 establecimos que Machine Learning permite construir modelos capaces de aprender patrones a partir de datos.

Sin embargo, **no todos los modelos aprenden de la misma manera**.

La estrategia utilizada dependerá fundamentalmente de:

* qué datos tenemos disponibles;
* si conocemos o no las respuestas correctas;
* qué queremos obtener del modelo;
* si existe interacción con un entorno;
* qué tipo de problema queremos resolver.

El descriptor de la asignatura distingue tres grandes tipos de aprendizaje:

**Aprendizaje supervisado**
**Aprendizaje no supervisado**
**Aprendizaje por refuerzo**

Cada uno responde a una lógica diferente.

Una primera aproximación puede representarse de la siguiente manera:

**Tenemos datos + conocemos la respuesta → Aprendizaje supervisado**

**Tenemos datos + no conocemos la respuesta → Aprendizaje no supervisado**

**Tenemos un agente + entorno + recompensas → Aprendizaje por refuerzo**

---

# 3. Aprendizaje supervisado

El **aprendizaje supervisado** utiliza ejemplos en los cuales conocemos previamente el resultado correcto que esperamos que el modelo aprenda.

El conjunto de entrenamiento contiene, por tanto:

**Datos de entrada + etiquetas**

Podemos representar el proceso como:

**Entrada X + Resultado conocido Y → Entrenamiento → Modelo**

Posteriormente:

**Nueva entrada X → Modelo → Predicción Ŷ**

El objetivo consiste en que el modelo aprenda una relación entre las variables de entrada y el resultado esperado.

Por ejemplo, si queremos construir un sistema que identifique fotografías de perros y gatos, podemos disponer de:

| Imagen         | Etiqueta |
| -------------- | -------- |
| imagen_001.jpg | perro    |
| imagen_002.jpg | gato     |
| imagen_003.jpg | gato     |
| imagen_004.jpg | perro    |

Durante el entrenamiento, el modelo conoce tanto la imagen como su clasificación correcta.

Por esta razón se utiliza el término **supervisado**: existe una referencia conocida que permite determinar si la predicción realizada durante el aprendizaje se aproxima o no al resultado esperado.

---

# 4. Variables de entrada y variable objetivo

En aprendizaje supervisado es importante distinguir entre los datos utilizados para realizar la predicción y aquello que queremos predecir.

### Variables de entrada

También pueden denominarse **características** o *features*.

Representan la información utilizada por el modelo.

Por ejemplo, para predecir el precio de una vivienda podríamos utilizar:

* superficie;
* número de dormitorios;
* número de baños;
* ubicación;
* antigüedad.

### Variable objetivo

También denominada *target*, etiqueta o variable de salida.

Representa aquello que queremos que el modelo aprenda a predecir.

En el ejemplo anterior:

**Características de la vivienda → Modelo → Precio**

En clasificación de imágenes:

**Imagen → Modelo → Clase**

Para nuestro proyecto integrador:

**Imagen → CNN → categoría de la imagen**

La calidad y correcta definición de estas entradas y resultados tendrá una influencia directa sobre la solución construida.

---

# 5. Clasificación

Uno de los principales problemas abordados mediante aprendizaje supervisado es la **clasificación**.

El objetivo consiste en asignar una observación a una categoría previamente definida.

Por ejemplo:

**Correo electrónico → spam / no spam**

**Transacción → fraude / no fraude**

**Imagen → perro / gato**

**Fotografía de residuo → plástico / vidrio / cartón / metal**

Las categorías posibles reciben habitualmente el nombre de **clases**.

### Clasificación binaria

Existen solamente dos clases posibles.

Ejemplos:

* sí / no;
* positivo / negativo;
* fraude / no fraude;
* perro / gato.

### Clasificación multiclase

Existen más de dos clases posibles.

Ejemplo:

**Imagen → plástico / vidrio / cartón / metal**

Nuestro proyecto integrador puede corresponder a clasificación binaria o multiclase dependiendo del problema seleccionado por cada dupla.

---

# 6. Regresión

Otro problema habitual de aprendizaje supervisado es la **regresión**.

A diferencia de la clasificación, la salida no corresponde a una categoría, sino a un **valor numérico continuo**.

Por ejemplo:

**Características de una vivienda → precio estimado**

**Información histórica → ventas esperadas**

**Variables meteorológicas → temperatura estimada**

**Características de un vehículo → consumo esperado**

La diferencia fundamental puede resumirse así:

**Clasificación → predice una categoría**

**Regresión → predice un valor numérico**

Ambas pertenecen normalmente al aprendizaje supervisado porque durante el entrenamiento disponemos de ejemplos cuyo resultado correcto conocemos.

---

# 7. Ejemplo de aprendizaje supervisado

Supongamos que una empresa desea desarrollar un sistema capaz de clasificar fotografías de productos tecnológicos en cuatro categorías:

* notebooks;
* smartphones;
* tablets;
* monitores.

La empresa dispone de 8.000 fotografías previamente clasificadas.

El dataset podría contener:

| Imagen           | Clase      |
| ---------------- | ---------- |
| producto_001.jpg | notebook   |
| producto_002.jpg | smartphone |
| producto_003.jpg | monitor    |
| producto_004.jpg | tablet     |

El proceso general sería:

**Imágenes etiquetadas**
↓
**Entrenamiento**
↓
**Modelo**
↓
**Nueva fotografía**
↓
**Predicción: smartphone**

Este problema corresponde a **aprendizaje supervisado de clasificación multiclase**.

Esta misma estructura conceptual será utilizada en el proyecto integrador.

---

# 8. Aprendizaje no supervisado

En el **aprendizaje no supervisado** trabajamos con datos que no poseen necesariamente una respuesta o etiqueta conocida.

Disponemos principalmente de:

**Datos de entrada**

pero no de:

**Resultado esperado conocido**

Por tanto:

**Datos sin etiquetas → Algoritmo → Estructuras o patrones**

El objetivo ya no consiste necesariamente en aprender a reproducir una respuesta conocida, sino en **descubrir estructuras, relaciones o agrupaciones presentes en los datos**.

El algoritmo debe encontrar patrones utilizando las características de las observaciones.

---

# 9. Agrupamiento o clustering

Una de las aplicaciones más representativas del aprendizaje no supervisado es el **clustering** o agrupamiento.

Su objetivo consiste en formar grupos de observaciones que presenten determinadas similitudes.

Supongamos que una empresa dispone de información sobre miles de clientes:

* frecuencia de compra;
* gasto promedio;
* cantidad de productos adquiridos;
* antigüedad como cliente;
* número de visitas.

Sin embargo, la empresa **no dispone de segmentos previamente definidos**.

Un algoritmo de clustering podría analizar estos datos y descubrir grupos con comportamientos similares.

Por ejemplo:

**Clientes**
↓
**Algoritmo de clustering**
↓
**Grupo 1 — compradores frecuentes**
**Grupo 2 — compradores ocasionales**
**Grupo 3 — clientes de alto valor**

Lo importante es que estas categorías **no fueron proporcionadas previamente como etiquetas**.

El algoritmo identifica estructuras existentes en los datos.

---

# 10. Diferencia entre clasificación y clustering

Clasificación y clustering pueden parecer similares porque ambos pueden terminar separando observaciones en grupos. Sin embargo, conceptualmente son diferentes.

### Clasificación

Las clases están previamente definidas.

Durante el entrenamiento sabemos a qué clase pertenece cada ejemplo.

Por ejemplo:

**Imagen 1 → perro**
**Imagen 2 → gato**

El modelo aprende posteriormente a asignar nuevas observaciones a esas clases.

### Clustering

Los grupos no necesariamente están definidos previamente.

El algoritmo intenta descubrir agrupaciones a partir de similitudes presentes en los datos.

Por ejemplo:

**Clientes sin segmentos**
↓
**Algoritmo**
↓
**Grupo A / Grupo B / Grupo C**

Por tanto:

**Clasificación → conocemos las categorías.**

**Clustering → buscamos descubrir agrupaciones.**

---

# 11. Otros usos del aprendizaje no supervisado

Aunque el clustering constituye uno de sus ejemplos más conocidos, el aprendizaje no supervisado puede utilizarse también para otros objetivos.

Entre ellos:

### Reducción de dimensionalidad

Busca representar información compleja utilizando una cantidad menor de variables o dimensiones, intentando conservar las características más relevantes de los datos.

### Identificación de estructuras

Permite descubrir relaciones o patrones que no eran evidentes inicialmente.

### Exploración de datos

Puede utilizarse para comprender mejor grandes conjuntos de información antes de construir otros modelos.

### Detección de comportamientos poco frecuentes

Dependiendo de la técnica utilizada, puede contribuir a identificar observaciones diferentes del comportamiento habitual.

Estos problemas muestran que Machine Learning no siempre busca predecir una respuesta previamente conocida.

---

# 12. Aprendizaje por refuerzo

El **aprendizaje por refuerzo** presenta una lógica diferente.

En lugar de aprender directamente desde un conjunto de ejemplos etiquetados, existe un **agente que interactúa con un entorno**.

El agente ejecuta acciones y recibe información sobre las consecuencias de esas acciones.

Los elementos fundamentales son:

* **Agente:** sistema que toma decisiones.
* **Entorno:** espacio o situación donde opera el agente.
* **Estado:** situación en la que se encuentra el entorno.
* **Acción:** decisión que puede ejecutar el agente.
* **Recompensa:** señal que indica qué tan favorable fue el resultado de una acción.

Podemos representar la interacción como:

**Estado → Agente → Acción → Entorno → Recompensa → Nuevo estado**

Este proceso se repite múltiples veces.

El objetivo general consiste en aprender una estrategia que permita obtener una **recompensa acumulada favorable**.

---

# 13. Ejemplo de aprendizaje por refuerzo

Supongamos que queremos enseñar a un agente virtual a desplazarse por un laberinto.

El agente puede realizar cuatro acciones:

* avanzar;
* retroceder;
* desplazarse a la izquierda;
* desplazarse a la derecha.

Podemos establecer recompensas:

**Llegar a la meta → +100**

**Chocar contra un obstáculo → -10**

**Cada movimiento → -1**

Inicialmente, el agente puede actuar de manera poco eficiente.

Mediante sucesivas interacciones con el entorno comienza a identificar qué decisiones producen mejores resultados.

Conceptualmente:

**Observa estado**
↓
**Selecciona acción**
↓
**Entorno responde**
↓
**Recibe recompensa**
↓
**Actualiza su estrategia**
↓
**Vuelve a actuar**

El aprendizaje ocurre mediante la experiencia acumulada de interacción.

---

# 14. Aplicaciones del aprendizaje por refuerzo

El aprendizaje por refuerzo puede aplicarse especialmente a problemas donde existe una secuencia de decisiones.

Algunos ejemplos son:

* agentes que juegan videojuegos;
* sistemas de control;
* robótica;
* navegación autónoma;
* asignación dinámica de recursos;
* optimización de determinadas decisiones secuenciales.

Una característica importante es que una acción puede afectar los estados y decisiones posteriores.

Por ello, el objetivo no consiste simplemente en obtener una buena respuesta inmediata, sino en aprender una estrategia que produzca buenos resultados a lo largo del tiempo.

---

# 15. Comparación de los tres paradigmas

Las diferencias principales pueden resumirse de la siguiente manera:

| Característica           | Supervisado                    | No supervisado                       | Por refuerzo                                              |
| ------------------------ | ------------------------------ | ------------------------------------ | --------------------------------------------------------- |
| **Datos etiquetados**    | Sí                             | No necesariamente                    | No funciona principalmente mediante un dataset etiquetado |
| **Referencia conocida**  | Existe un resultado esperado   | No existe necesariamente             | Existe una señal de recompensa                            |
| **Objetivo principal**   | Predecir resultados            | Descubrir patrones o estructuras     | Aprender una estrategia de decisión                       |
| **Ejemplos típicos**     | Clasificación, regresión       | Clustering                           | Control y decisiones secuenciales                         |
| **Forma de aprendizaje** | A partir de ejemplos conocidos | A partir de estructuras en los datos | Mediante interacción con un entorno                       |
| **Ejemplo**              | Clasificar una imagen          | Segmentar clientes                   | Agente que aprende a recorrer un entorno                  |

Una pregunta útil para identificar el paradigma apropiado es:

**¿Qué información tenemos disponible y qué queremos que el sistema aprenda?**

---

# 16. ¿Cómo seleccionar el tipo de aprendizaje?

Antes de seleccionar un algoritmo específico debemos comprender la naturaleza del problema.

Podemos utilizar algunas preguntas orientadoras.

### Pregunta 1: ¿Tenemos ejemplos con resultados conocidos?

**Sí → probablemente aprendizaje supervisado.**

Ejemplo:

Tenemos fotografías y sabemos qué objeto aparece en cada una.

### Pregunta 2: ¿Tenemos datos, pero no categorías o resultados conocidos?

**Sí → puede corresponder a aprendizaje no supervisado.**

Ejemplo:

Tenemos información sobre clientes y queremos descubrir segmentos.

### Pregunta 3: ¿El sistema debe aprender tomando decisiones e interactuando con un entorno?

**Sí → puede corresponder a aprendizaje por refuerzo.**

Ejemplo:

Un agente debe aprender qué acciones ejecutar para alcanzar un objetivo.

Sin embargo, estas preguntas constituyen una primera aproximación. Existen problemas más complejos y enfoques que pueden combinar diferentes estrategias.

---

# 17. El proyecto integrador: ¿qué tipo de aprendizaje utilizaremos?

El proyecto integrador de Machine Learning 2026 consiste en desarrollar una solución de **clasificación de imágenes mediante Deep Learning**.

Para entrenar nuestro modelo necesitaremos imágenes acompañadas de la categoría correcta.

Por ejemplo:

| Imagen          | Clase    |
| --------------- | -------- |
| residuo_001.jpg | plástico |
| residuo_002.jpg | vidrio   |
| residuo_003.jpg | cartón   |
| residuo_004.jpg | metal    |

Por tanto, nuestro problema presenta:

**Datos de entrada → imágenes**

**Resultados conocidos → clases**

**Objetivo → clasificar imágenes nuevas**

En consecuencia, utilizaremos:

**APRENDIZAJE SUPERVISADO**

y específicamente:

**APRENDIZAJE SUPERVISADO → CLASIFICACIÓN → CLASIFICACIÓN DE IMÁGENES**

Posteriormente utilizaremos una **Red Neuronal Convolucional (CNN)** como modelo de Deep Learning para realizar esta tarea.

Esto permite ubicar desde ahora nuestro proyecto dentro del mapa general de Machine Learning:

**Inteligencia Artificial**
↓
**Machine Learning**
↓
**Aprendizaje supervisado**
↓
**Clasificación**
↓
**Deep Learning**
↓
**CNN**
↓
**Clasificación de imágenes**

---

# 18. Casos para discusión en clase

Para cada situación, determine qué paradigma de aprendizaje parece más apropiado y justifique su decisión.

### Caso 1

Una institución financiera dispone de miles de transacciones históricas identificadas como **fraudulentas** o **legítimas** y desea clasificar nuevas transacciones.

**Pregunta:** ¿Supervisado, no supervisado o por refuerzo?

### Caso 2

Una empresa dispone de información sobre 100.000 clientes, pero no posee categorías previamente definidas. Quiere descubrir diferentes perfiles de consumidores.

**Pregunta:** ¿Qué paradigma resulta más apropiado?

### Caso 3

Un robot debe aprender a desplazarse dentro de un espacio evitando obstáculos y recibe una recompensa cuando alcanza correctamente su destino.

**Pregunta:** ¿Qué elementos corresponden al agente, entorno, acción y recompensa?

### Caso 4

Disponemos de fotografías correspondientes a cinco tipos diferentes de flores. Cada fotografía indica la especie a la que pertenece.

**Pregunta:** ¿Qué paradigma utilizaríamos y qué representan las etiquetas?

### Caso 5

Una inmobiliaria dispone de características y precios históricos de viviendas y quiere estimar el precio de propiedades nuevas.

**Pregunta:** ¿Qué paradigma utilizaríamos? ¿Clasificación o regresión?

---

# 19. Síntesis de la Semana 2

Al finalizar esta sesión deben quedar instaladas seis ideas fundamentales:

1. **Existen diferentes paradigmas de aprendizaje automático y su elección depende del problema y de la información disponible.**
2. **El aprendizaje supervisado utiliza ejemplos con resultados conocidos y permite abordar problemas como clasificación y regresión.**
3. **El aprendizaje no supervisado trabaja sin una respuesta objetivo previamente definida y busca descubrir patrones o estructuras presentes en los datos.**
4. **El aprendizaje por refuerzo se basa en la interacción entre un agente y un entorno mediante acciones y recompensas.**
5. **Clasificación y clustering no son equivalentes: en clasificación conocemos previamente las clases; en clustering buscamos descubrir agrupaciones.**
6. **Nuestro proyecto integrador corresponde a un problema de aprendizaje supervisado de clasificación de imágenes.**

### Hacia la Semana 3

Hasta ahora hemos respondido dos preguntas:

**Semana 1:** ¿Qué es Machine Learning y qué tipos de problemas puede resolver?

**Semana 2:** ¿De qué maneras puede aprender una máquina?

La Semana 3 avanzará hacia una nueva pregunta:

**¿Cómo se construye y entrena una solución de aprendizaje supervisado?**

Para ello estudiaremos con mayor profundidad **datos de entrada, características, etiquetas, entrenamiento y predicción**, además del flujo general que permite pasar desde un conjunto de datos hasta un modelo capaz de realizar predicciones.

