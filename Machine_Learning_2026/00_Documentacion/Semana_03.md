# Semana 3 — Aprendizaje supervisado y entrenamiento

## 1. Propósito de la sesión

Comprender cómo se construye una solución basada en **aprendizaje supervisado**, identificando los elementos que intervienen desde la representación de los datos hasta el entrenamiento y posterior utilización del modelo para realizar predicciones sobre datos nuevos.

Al finalizar la sesión, el estudiante debería ser capaz de:

* Identificar los componentes fundamentales de una solución de aprendizaje supervisado.
* Diferenciar datos de entrada, características y etiquetas.
* Comprender la relación entre variables de entrada y variable objetivo.
* Explicar conceptualmente qué significa entrenar un modelo.
* Diferenciar entrenamiento y predicción o inferencia.
* Comprender el papel de los parámetros de un modelo.
* Reconocer la necesidad de evaluar el desempeño del modelo.
* Describir el flujo general de construcción de una solución de aprendizaje supervisado.
* Relacionar este proceso con el proyecto integrador de clasificación de imágenes.

---

# 2. Recordando el aprendizaje supervisado

Durante la Semana 2 establecimos que el **aprendizaje supervisado** utiliza ejemplos para los cuales conocemos previamente el resultado correcto.

Disponemos de:

**Datos de entrada + Resultado conocido**

El objetivo consiste en utilizar estos ejemplos para construir un modelo capaz de realizar predicciones sobre datos nuevos.

Conceptualmente:

**Datos etiquetados**
↓
**Entrenamiento**
↓
**Modelo entrenado**
↓
**Datos nuevos**
↓
**Predicción**

Por ejemplo, para construir un clasificador de imágenes:

| Imagen         | Clase |
| -------------- | ----- |
| imagen_001.jpg | perro |
| imagen_002.jpg | gato  |
| imagen_003.jpg | perro |
| imagen_004.jpg | gato  |

El modelo utiliza durante su entrenamiento tanto las imágenes como sus respectivas clases.

Posteriormente debería ser capaz de recibir una imagen nueva y determinar a qué clase pertenece.

---

# 3. Los componentes de una solución supervisada

Una solución de aprendizaje supervisado requiere varios componentes.

### Datos de entrada

Representan la información que utilizaremos para aprender.

### Etiquetas o resultados conocidos

Indican cuál es la respuesta correcta asociada a cada ejemplo.

### Modelo

Representa la estructura matemática que aprenderá relaciones entre las entradas y las salidas.

### Proceso de entrenamiento

Permite ajustar progresivamente el modelo utilizando los ejemplos disponibles.

### Evaluación

Permite determinar qué tan adecuadamente funciona el modelo.

### Predicción

Corresponde a la utilización del modelo entrenado sobre datos nuevos.

Podemos representar el proceso general como:

**Datos + Etiquetas → Entrenamiento → Modelo → Evaluación → Predicción**

Cada uno de estos elementos será desarrollado progresivamente durante la asignatura.

---

# 4. Datos de entrada

Los **datos de entrada** corresponden a la información que el modelo recibe para aprender o realizar una predicción.

La naturaleza de los datos dependerá del problema.

Por ejemplo:

### Predicción del precio de una vivienda

Los datos podrían incluir:

* superficie;
* número de habitaciones;
* ubicación;
* antigüedad;
* número de baños.

### Clasificación de clientes

Los datos podrían incluir:

* edad;
* frecuencia de compra;
* gasto promedio;
* antigüedad;
* productos adquiridos.

### Clasificación de imágenes

El dato de entrada corresponde a una **imagen digital**.

Aunque visualmente observamos objetos, personas o paisajes, computacionalmente una imagen está representada mediante valores numéricos.

Por tanto, los modelos de Machine Learning finalmente trabajan con **representaciones numéricas de la información**.

---

# 5. Características o features

Las **características**, conocidas habitualmente como *features*, corresponden a la información que el modelo utiliza para identificar relaciones y patrones.

Consideremos un problema sencillo de predicción de precios:

| Superficie | Dormitorios | Antigüedad |      Precio |
| ---------: | ----------: | ---------: | ----------: |
|         60 |           2 |         15 |  75.000.000 |
|         90 |           3 |          8 | 110.000.000 |
|        120 |           4 |          3 | 160.000.000 |

En este ejemplo:

**Superficie + Dormitorios + Antigüedad → características**

mientras:

**Precio → variable objetivo**

Podemos representar las características mediante:

**X = variables utilizadas para realizar la predicción**

y la variable objetivo mediante:

**y = resultado que queremos predecir**

Por tanto:

**X → Modelo → ŷ**

donde **ŷ** representa la predicción generada por el modelo.

---

# 6. Características en una imagen

En una imagen, el concepto de característica adquiere una particularidad importante.

Una imagen digital está compuesta por **píxeles**.

Cada píxel contiene información numérica relacionada con su intensidad o color.

Una imagen en escala de grises puede representarse conceptualmente como una matriz:

|    |    |     |     |
| -: | -: | --: | --: |
| 12 | 18 |  25 |  31 |
| 15 | 22 |  80 |  92 |
| 20 | 45 | 120 | 135 |
| 10 | 30 |  60 |  75 |

Para el computador, la imagen no comienza siendo:

**“un gato”**

sino una estructura de valores numéricos.

En imágenes a color, habitualmente existen diferentes canales que representan componentes de color, por ejemplo:

**Rojo (R)**
**Verde (G)**
**Azul (B)**

Posteriormente veremos que las redes neuronales convolucionales pueden aprender progresivamente representaciones visuales más complejas a partir de esta información.

---

# 7. Etiquetas y variable objetivo

La **etiqueta** corresponde al resultado conocido que queremos que el modelo aprenda a predecir.

Por ejemplo:

| Imagen       | Etiqueta |
| ------------ | -------- |
| flor_001.jpg | rosa     |
| flor_002.jpg | tulipán  |
| flor_003.jpg | girasol  |
| flor_004.jpg | rosa     |

En este caso:

**X = imágenes**

**y = especies de flores**

Durante el entrenamiento, el modelo puede comparar sus predicciones con estas respuestas conocidas.

Las etiquetas deben representar correctamente aquello que queremos que el sistema aprenda.

Si las etiquetas contienen errores, el modelo recibirá información incorrecta durante su entrenamiento.

Por esta razón, la calidad de los datos y del proceso de etiquetado será un aspecto central de la Unidad 2.

---

# 8. Dataset de entrenamiento

El conjunto de ejemplos utilizado para que el modelo aprenda recibe el nombre de **conjunto o dataset de entrenamiento**.

Cada ejemplo contiene normalmente:

**Entrada X + resultado y**

Por ejemplo:

**imagen de perro → perro**

**imagen de gato → gato**

**imagen de perro → perro**

Durante el entrenamiento, estos ejemplos son utilizados repetidamente para ajustar el comportamiento del modelo.

La cantidad de datos necesaria depende de numerosos factores, entre ellos:

* complejidad del problema;
* diversidad de las observaciones;
* cantidad de clases;
* arquitectura utilizada;
* calidad de los datos.

Por tanto, disponer simplemente de una gran cantidad de ejemplos no garantiza automáticamente un buen modelo.

---

# 9. ¿Qué significa entrenar un modelo?

**Entrenar un modelo** significa ajustar sus parámetros utilizando los datos disponibles para mejorar progresivamente su capacidad de realizar la tarea solicitada.

El proceso puede entenderse conceptualmente mediante cuatro pasos:

**1. El modelo recibe una entrada**

↓

**2. Genera una predicción**

↓

**3. La predicción se compara con el resultado correcto**

↓

**4. El modelo ajusta sus parámetros para intentar reducir el error**

Este proceso se repite múltiples veces utilizando numerosos ejemplos.

De forma simplificada:

**Entrada → Predicción → Comparación → Ajuste**

El entrenamiento constituye, por tanto, un proceso iterativo.

---

# 10. Parámetros del modelo

Los **parámetros** son valores internos que el modelo aprende durante el entrenamiento.

Estos parámetros determinan cómo transforma las entradas en predicciones.

En diferentes algoritmos pueden adoptar distintas formas.

En las redes neuronales encontraremos principalmente:

* **pesos**;
* **sesgos o bias**.

Inicialmente, estos parámetros todavía no representan adecuadamente el problema.

Durante el entrenamiento se modifican progresivamente buscando mejorar las predicciones.

Podemos representar conceptualmente:

**Parámetros iniciales**
↓
**Entrenamiento con datos**
↓
**Ajuste de parámetros**
↓
**Modelo entrenado**

Esta idea será fundamental durante la Semana 4, cuando estudiaremos con mayor profundidad el funcionamiento de las redes neuronales.

---

# 11. Predicción y error

Supongamos que estamos construyendo un clasificador de imágenes con dos categorías:

**perro / gato**

El modelo recibe una imagen cuya etiqueta correcta es:

**gato**

pero genera como predicción:

**perro**

Existe entonces una diferencia entre:

**Resultado esperado → gato**

y

**Predicción → perro**

Durante el entrenamiento necesitamos una forma de representar qué tan incorrecta fue la predicción.

Esta idea se expresa mediante una **función de pérdida** (*loss function*).

La función de pérdida permite cuantificar el error cometido por el modelo durante el proceso de aprendizaje.

Conceptualmente:

**Predicción + Resultado correcto → Función de pérdida → Error**

El entrenamiento intentará progresivamente **reducir esa pérdida**.

---

# 12. Aprender significa reducir el error

El proceso de entrenamiento puede entenderse como una búsqueda de parámetros que permitan reducir el error del modelo.

Supongamos que observamos conceptualmente:

**Inicio del entrenamiento → error alto**

Después de varios ajustes:

**Entrenamiento intermedio → error menor**

Finalmente:

**Modelo entrenado → error suficientemente bajo**

Sin embargo, obtener un error pequeño sobre los mismos datos utilizados durante el entrenamiento **no garantiza que el modelo funcione correctamente con datos nuevos**.

Aquí reaparece una idea introducida durante la Semana 1:

**generalización**.

Un buen modelo debe aprender patrones útiles, no simplemente memorizar los ejemplos utilizados para entrenarlo.

Esta problemática será desarrollada con mayor profundidad durante la Unidad 2.

---

# 13. Entrenamiento versus inferencia

Es importante diferenciar dos momentos fundamentales en el ciclo de Machine Learning.

### Entrenamiento

Durante el entrenamiento:

* utilizamos datos cuyo resultado conocemos;
* el modelo realiza predicciones;
* calculamos el error;
* ajustamos sus parámetros;
* repetimos el proceso.

Por tanto:

**Datos etiquetados → Modelo → Predicción → Error → Ajuste**

### Inferencia

Una vez entrenado el modelo, podemos utilizarlo para procesar datos nuevos.

En este momento normalmente ya no modificamos sus parámetros.

Por ejemplo:

**Nueva imagen → Modelo entrenado → Predicción**

Este proceso recibe habitualmente el nombre de **inferencia**.

En nuestro producto final, cuando un usuario cargue una fotografía y la aplicación entregue una clasificación, estaremos realizando **inferencia**.

---

# 14. Entrenar no es lo mismo que utilizar

La diferencia anterior tiene importantes consecuencias prácticas.

Durante el **entrenamiento** normalmente necesitamos:

* numerosos datos;
* mayor capacidad computacional;
* múltiples iteraciones;
* cálculo de errores;
* modificación de parámetros.

Durante la **inferencia** necesitamos principalmente:

* el modelo ya entrenado;
* una nueva entrada;
* capacidad suficiente para ejecutar el modelo;
* producir rápidamente una predicción.

Por ello, el modelo puede entrenarse en un entorno y posteriormente ejecutarse en otro.

Por ejemplo:

**Entrenamiento en cloud o GPU**
↓
**Modelo entrenado**
↓
**Implementación en servidor, computador o dispositivo**

Esta distinción será especialmente importante durante la Unidad 4, cuando trabajemos la implementación y optimización de modelos.

---

# 15. ¿Cómo sabemos si un modelo funciona?

Después de entrenar un modelo debemos determinar qué tan correctamente realiza su tarea.

No basta con observar algunos ejemplos y concluir que funciona.

Necesitamos **métricas de evaluación**.

En un problema de clasificación, una primera métrica intuitiva es la **exactitud o accuracy**.

Puede expresarse conceptualmente como:

**Accuracy = predicciones correctas / total de predicciones**

Por ejemplo:

Si un modelo clasifica 100 imágenes y acierta 85:

**Accuracy = 85 / 100 = 0,85 = 85%**

Esto significa que clasificó correctamente el 85% de los ejemplos evaluados.

Posteriormente conoceremos otras herramientas de evaluación, pero en esta etapa interesa comprender una idea:

**Todo modelo debe ser evaluado utilizando criterios objetivos.**

---

# 16. Separar aprendizaje y evaluación

Existe un problema si evaluamos el modelo utilizando exactamente los mismos ejemplos con los que fue entrenado.

Supongamos que un estudiante prepara una prueba con 100 preguntas.

Luego recibe anticipadamente esas mismas 100 preguntas junto con sus respuestas y las memoriza.

Si posteriormente evaluamos al estudiante utilizando exactamente las mismas preguntas, podría obtener un resultado excelente sin haber aprendido realmente a resolver problemas nuevos.

Algo similar puede ocurrir en Machine Learning.

Por ello, normalmente los datos se dividen en diferentes conjuntos.

Una primera aproximación es:

**Datos disponibles**
↓
**Datos de entrenamiento + Datos de prueba**

El modelo aprende utilizando los primeros y posteriormente evaluamos su comportamiento utilizando ejemplos que no formaron parte del entrenamiento.

Más adelante incorporaremos también el **conjunto de validación** y estudiaremos formalmente la división:

**entrenamiento / validación / prueba**.

---

# 17. Flujo general de una solución de aprendizaje supervisado

Podemos ahora desarrollar con mayor detalle el flujo presentado durante la Semana 1.

### 1. Definir el problema

¿Qué queremos predecir?

Ejemplo:

**Clasificar imágenes de residuos.**

### 2. Definir las clases o resultado esperado

Por ejemplo:

* plástico;
* vidrio;
* cartón;
* metal.

### 3. Obtener datos

Recopilar imágenes representativas de las diferentes categorías.

### 4. Etiquetar

Cada imagen debe estar asociada con su clase correcta.

### 5. Preparar los datos

Organizar y transformar los datos para que puedan ser utilizados por el modelo.

### 6. Separar los datos

Reservar información para entrenamiento y evaluación.

### 7. Seleccionar el modelo

Determinar qué algoritmo o arquitectura utilizaremos.

### 8. Entrenar

El modelo ajusta sus parámetros utilizando los datos de entrenamiento.

### 9. Evaluar

Medir su comportamiento utilizando datos no empleados directamente para aprender.

### 10. Mejorar

Modificar datos, configuración o modelo cuando los resultados no sean satisfactorios.

### 11. Guardar el modelo

Conservar los parámetros aprendidos.

### 12. Realizar inferencia

Utilizar el modelo con nuevos datos.

---

# 18. Primera aproximación con Python

En Python, el flujo general de una solución supervisada suele reflejar las mismas etapas conceptuales.

De manera simplificada:

```python
# 1. Cargar datos
X, y = cargar_datos()

# 2. Separar datos
X_train, X_test, y_train, y_test = dividir_datos(X, y)

# 3. Crear el modelo
modelo = crear_modelo()

# 4. Entrenar
modelo.fit(X_train, y_train)

# 5. Evaluar
resultado = modelo.evaluate(X_test, y_test)

# 6. Predecir
prediccion = modelo.predict(nuevo_dato)
```

La sintaxis específica dependerá de la biblioteca y del modelo utilizado.

Lo importante en esta etapa es reconocer que el código reproduce el mismo proceso conceptual:

**cargar → dividir → crear → entrenar → evaluar → predecir**

Posteriormente implementaremos estas etapas utilizando herramientas específicas para redes neuronales.

---

# 19. Aplicación al proyecto integrador

Nuestro proyecto integrador seguirá exactamente esta lógica.

Supongamos que una dupla decide construir un clasificador de cuatro tipos de residuos.

### Entrada

Fotografías de residuos.

### Etiquetas

* plástico;
* vidrio;
* cartón;
* metal.

### Modelo inicial

Durante las primeras semanas se construirá una primera aproximación mediante una red neuronal básica.

### Modelo definitivo

Posteriormente se diseñará una **Red Neuronal Convolucional (CNN)**.

### Entrenamiento

Las imágenes etiquetadas serán utilizadas para ajustar los parámetros del modelo.

### Evaluación

Se utilizarán imágenes que permitan determinar su capacidad de clasificación.

### Inferencia

Finalmente:

**Nueva fotografía**
↓
**Modelo entrenado**
↓
**Predicción**
↓
**“Vidrio — 91%”**

Por tanto, las etapas estudiadas durante esta semana constituyen el **flujo técnico que acompañará todo el proyecto integrador**.

---

# 20. Caso conductor: del problema al modelo

Consideremos una empresa agrícola que necesita identificar automáticamente cuatro categorías de frutas:

* manzana;
* naranja;
* plátano;
* pera.

La empresa dispone inicialmente de 4.000 fotografías etiquetadas.

El problema puede estructurarse como:

**Problema:** clasificar automáticamente fotografías de frutas.

**Entrada:** imagen.

**Variable objetivo:** tipo de fruta.

**Tipo de aprendizaje:** supervisado.

**Tipo de problema:** clasificación multiclase.

**Datos disponibles:** imágenes etiquetadas.

**Entrenamiento:** utilizar las imágenes conocidas para ajustar el modelo.

**Evaluación:** comprobar el comportamiento sobre imágenes no utilizadas para entrenar.

**Inferencia:** entregar una nueva fotografía al modelo.

**Resultado esperado:** categoría predicha.

Así, una necesidad inicialmente expresada en lenguaje cotidiano puede transformarse progresivamente en un **problema de Machine Learning claramente definido**.

---

# 21. Preguntas para discusión en clase

### Caso 1

Disponemos de información histórica sobre viviendas y sus respectivos precios.

**Pregunta:** ¿Qué correspondería a X y qué correspondería a y?

### Caso 2

Tenemos 10.000 fotografías clasificadas como perros, gatos y aves.

**Pregunta:** ¿Cuáles son los datos de entrada y cuáles son las etiquetas?

### Caso 3

Un modelo obtiene un 99% de exactitud utilizando las mismas imágenes con las que fue entrenado.

**Pregunta:** ¿Podemos concluir inmediatamente que funcionará correctamente con fotografías nuevas? ¿Por qué?

### Caso 4

Un modelo ya fue entrenado y posteriormente se incorpora dentro de una aplicación móvil. El usuario toma una fotografía y recibe una clasificación.

**Pregunta:** ¿La aplicación está entrenando el modelo o realizando inferencia?

### Caso 5

Una dupla quiere desarrollar un clasificador de especies de árboles, pero dispone de imágenes sin ninguna identificación de especie.

**Pregunta:** ¿Qué problema presenta esta situación para desarrollar una solución supervisada?

---

# 22. Síntesis de la Semana 3

Al finalizar esta sesión deben quedar instaladas siete ideas fundamentales:

1. **Una solución supervisada utiliza datos de entrada y resultados conocidos para entrenar un modelo.**
2. **Las características representan la información utilizada para predecir y la etiqueta representa aquello que queremos predecir.**
3. **Entrenar consiste en ajustar progresivamente los parámetros del modelo a partir de los datos disponibles.**
4. **El modelo genera predicciones que pueden compararse con los resultados conocidos para determinar su error.**
5. **Entrenamiento e inferencia son procesos diferentes: durante el primero se aprenden parámetros; durante el segundo se utiliza el modelo ya entrenado.**
6. **Un modelo debe ser evaluado con datos que permitan determinar su capacidad para trabajar con observaciones nuevas.**
7. **El flujo problema → datos → entrenamiento → evaluación → modelo → inferencia constituye la base técnica del proyecto integrador.**

### Hacia la Semana 4

Hasta ahora hemos avanzado desde lo general hacia el proceso de construcción:

**Semana 1:** ¿Qué es Machine Learning?

**Semana 2:** ¿De qué maneras puede aprender una máquina?

**Semana 3:** ¿Cómo se construye y entrena una solución supervisada?

La Semana 4 responderá:

**¿Cómo aprende una red neuronal?**

Para ello abordaremos **neurona artificial, capas, pesos, funciones de activación, entrenamiento y backpropagation**, preparando además la primera aproximación práctica del proyecto integrador.

