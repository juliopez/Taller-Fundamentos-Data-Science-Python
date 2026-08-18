# Semana 5 — Datos para Machine Learning

## 1. Propósito de la sesión

Comprender qué características debe poseer un conjunto de datos para ser utilizado en un proyecto de Machine Learning, identificando los tipos de datos involucrados, las variables relevantes, las etiquetas y los principales requerimientos que debe cumplir un dataset antes de iniciar el entrenamiento de un modelo.

Al finalizar la sesión, el estudiante debería ser capaz de:

* Explicar por qué los datos constituyen un componente crítico en Machine Learning.
* Identificar diferentes tipos de datos utilizados en proyectos de aprendizaje automático.
* Diferenciar datos estructurados y no estructurados.
* Identificar características y etiquetas dentro de un dataset.
* Determinar qué datos son necesarios según el problema que se desea resolver.
* Reconocer la importancia de la calidad, cantidad, diversidad y representatividad de los datos.
* Comprender el concepto de distribución de datos.
* Identificar problemas básicos como datos faltantes, duplicados, errores y desbalance entre clases.
* Comprender el ciclo general de los datos dentro de un proyecto de Machine Learning.
* Relacionar estos conceptos con la construcción del dataset del proyecto integrador.

---

# 2. Del modelo hacia los datos

Durante la Unidad 1 estudiamos principalmente cómo funciona un modelo de Machine Learning.

La secuencia fue:

**Datos → Modelo → Predicción**

También establecimos que durante el entrenamiento:

**Datos etiquetados → Red neuronal → Predicción → Error → Ajuste**

Hasta ahora nos concentramos principalmente en el **modelo**.

A partir de esta unidad cambiaremos temporalmente el foco hacia los **datos**.

La pregunta principal será:

**¿Qué información necesita un modelo para aprender correctamente?**

Un algoritmo sofisticado no puede compensar completamente un conjunto de datos incorrecto, insuficiente o poco representativo.

Por esta razón, una idea fundamental en Machine Learning es:

**La calidad del modelo depende fuertemente de la calidad de los datos utilizados para entrenarlo.**

---

# 3. ¿Qué es un dataset?

Un **dataset** es un conjunto organizado de observaciones o ejemplos que contiene la información utilizada para analizar un problema o entrenar un modelo.

En datos tabulares puede representarse mediante filas y columnas.

Por ejemplo:

| Edad |   Ingreso | Compras | Cliente frecuente |
| ---: | --------: | ------: | ----------------- |
|   25 |   750.000 |       4 | No                |
|   37 | 1.200.000 |      15 | Sí                |
|   48 | 1.850.000 |      21 | Sí                |

Cada fila representa normalmente una **observación**.

Cada columna representa una **variable**.

En este caso:

**Edad + Ingreso + Compras → características**

**Cliente frecuente → etiqueta**

Sin embargo, un dataset no necesariamente adopta una estructura tabular.

También puede contener:

* imágenes;
* texto;
* audio;
* video;
* documentos;
* series temporales.

---

# 4. Datos estructurados y no estructurados

Los datos pueden presentar diferentes niveles de organización.

### Datos estructurados

Poseen una estructura claramente definida.

Habitualmente se almacenan en:

* tablas;
* bases de datos relacionales;
* archivos CSV;
* hojas de cálculo.

Ejemplo:

| Producto | Precio | Stock | Categoría   |
| -------- | -----: | ----: | ----------- |
| Notebook | 650000 |    12 | Computación |
| Mouse    |  15000 |    80 | Accesorios  |

Cada atributo tiene una posición y significado definidos.

### Datos no estructurados

No siguen necesariamente una estructura tabular tradicional.

Ejemplos:

* imágenes;
* documentos;
* audio;
* video;
* correos electrónicos.

Una fotografía de un automóvil, por ejemplo, no contiene columnas denominadas:

**ruedas = 4**
**color = rojo**
**tipo = automóvil**

La información se encuentra contenida en los valores de sus píxeles.

Nuestro proyecto integrador trabajará principalmente con **datos no estructurados: imágenes**.

---

# 5. Tipos de datos en Machine Learning

Dependiendo del problema podemos utilizar diferentes tipos de información.

### Datos numéricos

Representan cantidades.

Ejemplos:

* edad;
* precio;
* temperatura;
* ingresos;
* distancia.

### Datos categóricos

Representan categorías.

Ejemplos:

* región;
* tipo de cliente;
* categoría de producto;
* estado civil.

### Texto

Ejemplos:

* comentarios;
* documentos;
* publicaciones;
* correos electrónicos.

### Audio

Ejemplos:

* voz;
* sonidos ambientales;
* música.

### Imágenes

Ejemplos:

* fotografías;
* radiografías;
* imágenes satelitales;
* documentos escaneados.

### Video

Corresponde a secuencias de imágenes que pueden además incluir audio.

La naturaleza del dato condicionará posteriormente las técnicas de procesamiento y los modelos que podemos utilizar.

---

# 6. Datos y problema de negocio

Antes de recopilar información debemos definir correctamente el problema.

No tiene sentido comenzar preguntando:

**¿Qué dataset podemos conseguir?**

sin saber primero:

**¿Qué queremos resolver?**

El orden adecuado debería ser:

**Problema**
↓
**Objetivo del modelo**
↓
**Datos necesarios**

Por ejemplo:

### Problema

Clasificar automáticamente diferentes tipos de residuos.

### Objetivo

Determinar la categoría de un residuo a partir de una fotografía.

### Datos necesarios

Fotografías representativas de cada categoría.

### Etiquetas

* plástico;
* vidrio;
* cartón;
* metal.

Por tanto, los datos deben seleccionarse en función de la **pregunta que queremos responder**.

---

# 7. Características y etiquetas dentro del dataset

Durante la Unidad 1 introdujimos los conceptos de **features** y **labels**.

En esta unidad comenzamos a observarlos desde la perspectiva de construcción del dataset.

### Características

Representan la información utilizada por el modelo para aprender.

En un problema tabular:

**X = características**

Por ejemplo:

**edad + ingresos + compras**

### Etiqueta

Representa aquello que queremos predecir.

**y = etiqueta**

Por ejemplo:

**cliente frecuente**

En clasificación de imágenes:

**X = imagen**

**y = clase de la imagen**

Ejemplo:

**imagen_001.jpg → plástico**

---

# 8. ¿Qué significa etiquetar datos?

El **etiquetado** consiste en asignar a cada ejemplo la categoría o resultado correcto que posteriormente utilizaremos durante el entrenamiento supervisado.

Supongamos que disponemos de 4.000 fotografías de residuos.

Para utilizarlas en clasificación supervisada debemos conocer qué representa cada una.

Por ejemplo:

| Archivo      | Etiqueta |
| ------------ | -------- |
| img_0001.jpg | plástico |
| img_0002.jpg | vidrio   |
| img_0003.jpg | metal    |
| img_0004.jpg | cartón   |

Esta información constituye la referencia que utilizará el modelo durante el aprendizaje.

Un etiquetado incorrecto implica que el modelo puede aprender información equivocada.

Por ejemplo:

**Fotografía de vidrio → etiqueta “plástico”**

constituye un error de calidad del dataset.

---

# 9. Organización de un dataset de imágenes

Una forma frecuente de organizar imágenes para clasificación consiste en utilizar una carpeta por clase.

Por ejemplo:

```text
dataset/
│
├── plastico/
│   ├── img001.jpg
│   ├── img002.jpg
│   └── img003.jpg
│
├── vidrio/
│   ├── img004.jpg
│   ├── img005.jpg
│   └── img006.jpg
│
├── carton/
│   ├── img007.jpg
│   ├── img008.jpg
│   └── img009.jpg
│
└── metal/
    ├── img010.jpg
    ├── img011.jpg
    └── img012.jpg
```

En esta estructura, la carpeta puede actuar como referencia para la clase correspondiente.

Posteriormente herramientas como TensorFlow o Keras pueden utilizar esta organización para cargar las imágenes y generar automáticamente sus etiquetas.

---

# 10. Cantidad de datos

Una pregunta habitual es:

**¿Cuántos datos necesito?**

No existe una respuesta única.

La cantidad necesaria depende de factores como:

* dificultad del problema;
* número de clases;
* diversidad interna de cada clase;
* arquitectura utilizada;
* resolución de las imágenes;
* similitud entre categorías;
* calidad de los ejemplos.

En general:

**más datos relevantes y variados suelen proporcionar mejores oportunidades de aprendizaje**.

Sin embargo:

**más datos no significa automáticamente mejores datos.**

Un dataset con 100.000 imágenes duplicadas o mal etiquetadas puede ser menos útil que uno más pequeño pero correctamente construido.

---

# 11. Calidad de los datos

Un dataset de calidad debería contener información:

* correcta;
* relevante;
* consistente;
* representativa;
* suficientemente diversa.

Algunos problemas frecuentes son:

### Datos incorrectos

Ejemplos mal etiquetados.

### Datos corruptos

Archivos que no pueden abrirse correctamente.

### Duplicados

La misma observación aparece numerosas veces.

### Datos irrelevantes

Ejemplos que no representan realmente el problema.

### Inconsistencias

Diferentes criterios de etiquetado para casos similares.

Estos problemas pueden afectar directamente el comportamiento del modelo.

---

# 12. Representatividad

Los datos utilizados para entrenar un modelo deben ser **representativos de las condiciones reales en las que posteriormente será utilizado**.

Supongamos que queremos clasificar fotografías de frutas tomadas por teléfonos móviles.

Si entrenamos el modelo solamente con fotografías:

* tomadas en estudio;
* con fondo blanco;
* iluminación perfecta;
* objetos centrados;

pero posteriormente los usuarios toman fotografías:

* con diferentes fondos;
* en exteriores;
* con poca luz;
* desde distintos ángulos;

el modelo podría presentar dificultades.

Por tanto, el dataset debería representar adecuadamente la **variabilidad esperada en producción**.

---

# 13. Diversidad dentro de una clase

Una clase no debería estar representada por ejemplos excesivamente similares.

Consideremos la categoría:

**perro**

Si todas las fotografías utilizadas durante el entrenamiento corresponden a perros:

* de la misma raza;
* fotografiados frontalmente;
* sobre fondo blanco;

el modelo podría aprender características demasiado específicas.

Una mejor colección debería incluir:

* diferentes razas;
* distintos tamaños;
* múltiples colores;
* varios ángulos;
* diferentes fondos;
* distintas condiciones de iluminación.

La diversidad ayuda al modelo a aprender patrones más generales.

---

# 14. Distribución de los datos

La **distribución de los datos** describe cómo se encuentran representadas las diferentes características o categorías dentro del conjunto disponible.

En clasificación, un aspecto inmediato consiste en observar cuántos ejemplos tenemos por clase.

Por ejemplo:

| Clase    | Imágenes |
| -------- | -------: |
| plástico |    1.000 |
| vidrio   |    1.050 |
| cartón   |      980 |
| metal    |    1.020 |

Este dataset presenta una distribución relativamente equilibrada.

Comparemos con:

| Clase    | Imágenes |
| -------- | -------: |
| plástico |    3.500 |
| vidrio   |      250 |
| cartón   |      150 |
| metal    |      100 |

Aquí existe un fuerte **desbalance entre clases**.

---

# 15. ¿Qué es el desbalance de clases?

Existe **desbalance de clases** cuando algunas categorías poseen una cantidad de ejemplos considerablemente mayor que otras.

Esto puede producir problemas durante el entrenamiento.

Supongamos:

**90% de las imágenes → plástico**

**10% → vidrio**

Un modelo que siempre responda:

**plástico**

obtendría:

**90% de accuracy**

sin haber aprendido realmente a distinguir adecuadamente ambas categorías.

Este ejemplo muestra por qué debemos observar cuidadosamente cómo están distribuidos los datos.

---

# 16. Datos duplicados

Otro problema habitual corresponde a los **duplicados**.

Supongamos que tenemos:

**5.000 archivos**

pero:

**1.500 corresponden a copias de las mismas imágenes.**

Aparentemente disponemos de 5.000 ejemplos, pero la diversidad real del dataset es considerablemente menor.

Los duplicados pueden además generar problemas cuando posteriormente dividimos los datos.

Por ejemplo, podría ocurrir:

**imagen A → entrenamiento**

y

**copia de imagen A → prueba**

En ese caso, la evaluación podría sobreestimar el verdadero desempeño del modelo.

---

# 17. Datos faltantes

En datasets tabulares resulta frecuente encontrar valores ausentes.

Por ejemplo:

| Edad | Ingreso | Región     |
| ---: | ------: | ---------- |
|   25 |  850000 | RM         |
|   37 |       — | Valparaíso |
|    — | 1200000 | Biobío     |

Estos valores deben ser identificados y tratados adecuadamente.

En imágenes, el equivalente puede aparecer como:

* archivo inexistente;
* archivo dañado;
* imagen ilegible;
* etiqueta ausente;
* imagen sin categoría asociada.

Por tanto, antes del entrenamiento debemos verificar que cada observación sea válida.

---

# 18. Datos incorrectos e inconsistentes

Un dataset también puede contener ejemplos válidos técnicamente pero incorrectos desde el punto de vista semántico.

Por ejemplo:

**carpeta/plastico/img001.jpg**

pero la fotografía corresponde a:

**una lata metálica**

Otro caso podría ser:

**“cartón”**

**“carton”**

**“Cartón”**

tratados accidentalmente como categorías distintas.

La consistencia en nombres, categorías y criterios de clasificación resulta esencial.

---

# 19. Sesgos en los datos

Los datos recopilados pueden contener **sesgos**, es decir, representaciones sistemáticamente desequilibradas de la realidad que queremos modelar.

Supongamos que un sistema debe clasificar vehículos en diferentes condiciones ambientales.

Si prácticamente todas las imágenes fueron tomadas:

* durante el día;
* con buen clima;
* en zonas urbanas;

podríamos estar subrepresentando:

* conducción nocturna;
* lluvia;
* caminos rurales.

El modelo podría funcionar adecuadamente dentro del contexto representado y fallar cuando las condiciones cambien.

Por tanto, antes de entrenar debemos preguntarnos:

**¿Qué situaciones están representadas en el dataset y cuáles no?**

---

# 20. Resolución y formato de imágenes

En clasificación de imágenes también debemos considerar características técnicas.

Entre ellas:

* formato;
* resolución;
* dimensiones;
* canales de color;
* relación de aspecto.

Podemos encontrar imágenes de:

**300 × 300**

**1024 × 768**

**4000 × 3000**

y formatos:

* JPG;
* PNG;
* WEBP.

Los modelos normalmente requieren una representación consistente.

Por esta razón, durante el preprocesamiento podremos necesitar operaciones como:

* redimensionamiento;
* normalización;
* conversión de formatos.

Estas transformaciones serán desarrolladas con mayor profundidad durante la próxima sesión.

---

# 21. Fuentes de datos

Los datos utilizados en un proyecto pueden obtenerse desde diferentes fuentes.

### Datos propios

Recopilados específicamente para el proyecto.

Ejemplo:

Fotografías tomadas por los estudiantes.

### Datos institucionales

Disponibles dentro de una organización.

### Repositorios públicos

Existen datasets disponibles para investigación y aprendizaje.

### APIs

Algunos servicios permiten obtener información mediante interfaces programáticas.

### Datos sintéticos

En determinados contextos es posible generar información artificial para complementar conjuntos existentes.

Independientemente de la fuente, siempre debemos evaluar su pertinencia y calidad.

---

# 22. Dataset propio versus dataset existente

Para un proyecto de clasificación de imágenes pueden existir dos estrategias principales.

### Utilizar un dataset existente

Ventajas:

* rapidez;
* mayor cantidad de ejemplos;
* etiquetas disponibles.

Desventajas:

* puede no representar exactamente el problema;
* puede contener sesgos;
* puede presentar categorías diferentes a las requeridas.

### Construir un dataset propio

Ventajas:

* control sobre las clases;
* mayor alineación con el problema;
* posibilidad de controlar condiciones de recolección.

Desventajas:

* requiere más tiempo;
* necesita etiquetado;
* puede contener pocos ejemplos.

También es posible utilizar un enfoque mixto:

**dataset existente + imágenes propias**

siempre que exista consistencia entre ambos.

---

# 23. Preguntas antes de aceptar un dataset

Antes de decidir que un dataset es adecuado deberíamos plantearnos preguntas como:

**¿Representa realmente el problema?**

**¿Contiene las clases que necesitamos?**

**¿Las etiquetas son confiables?**

**¿Existe suficiente cantidad de ejemplos?**

**¿Las clases están razonablemente representadas?**

**¿Existen duplicados?**

**¿Las imágenes presentan suficiente diversidad?**

**¿Las condiciones de las imágenes se parecen a las que enfrentará el modelo en producción?**

**¿Tenemos permiso para utilizar esos datos?**

Estas preguntas permiten evaluar el dataset antes de invertir recursos en entrenar un modelo.

---

# 24. Ciclo de vida de los datos

Los datos atraviesan diferentes etapas dentro de un proyecto de Machine Learning.

Podemos representar un ciclo básico:

**1. Definir necesidades**

¿Qué información necesitamos?

↓

**2. Recolectar**

Obtener las observaciones.

↓

**3. Inspeccionar**

Comprender qué contiene el dataset.

↓

**4. Limpiar**

Eliminar o corregir problemas.

↓

**5. Etiquetar**

Asignar resultados conocidos cuando corresponda.

↓

**6. Transformar**

Preparar los datos para el modelo.

↓

**7. Dividir**

Generar conjuntos de entrenamiento, validación y prueba.

↓

**8. Entrenar**

Utilizar los datos para construir el modelo.

↓

**9. Evaluar**

Determinar su capacidad de generalización.

↓

**10. Mantener**

Incorporar nuevos datos o corregir problemas cuando sea necesario.

Los datos no constituyen, por tanto, una actividad aislada previa al entrenamiento. Forman parte de todo el ciclo de vida del proyecto.

---

# 25. Inspección inicial del dataset

Antes de entrenar es recomendable realizar una **exploración inicial**.

En clasificación de imágenes podríamos revisar:

* cantidad total de archivos;
* cantidad de clases;
* imágenes por clase;
* formatos existentes;
* dimensiones de las imágenes;
* archivos corruptos;
* duplicados;
* ejemplos visuales de cada categoría.

Por ejemplo:

```text
Dataset: residuos

Total imágenes: 4.250

Clases:
- plástico: 1.120
- vidrio: 1.030
- cartón: 1.080
- metal: 1.020
```

Esta inspección permite identificar problemas tempranamente.

---

# 26. Primera inspección con Python

Python puede utilizarse para revisar la estructura de un dataset.

Por ejemplo:

```python
import os

ruta = "dataset"

for clase in os.listdir(ruta):
    carpeta = os.path.join(ruta, clase)

    if os.path.isdir(carpeta):
        cantidad = len(os.listdir(carpeta))
        print(clase, cantidad)
```

Una posible salida sería:

```text
plastico 1120
vidrio 1030
carton 1080
metal 1020
```

Este procedimiento sencillo permite comenzar a responder una pregunta importante:

**¿Cómo están distribuidas las imágenes entre las distintas clases?**

---

# 27. Visualización de ejemplos

También resulta conveniente observar directamente algunas imágenes.

Por ejemplo, utilizando Python:

```python
import matplotlib.pyplot as plt
from PIL import Image

imagen = Image.open("dataset/plastico/img001.jpg")

plt.imshow(imagen)
plt.axis("off")
plt.show()
```

La inspección visual puede revelar problemas que no aparecen en un simple conteo:

* fotografía incorrectamente etiquetada;
* imagen borrosa;
* archivo con marcas;
* fondo artificial;
* objeto demasiado pequeño;
* imagen completamente diferente de las demás.

Machine Learning requiere combinar **análisis automático y revisión crítica de los datos**.

---

# 28. Datos adecuados para nuestro proyecto integrador

Nuestro proyecto necesitará un dataset que cumpla al menos con condiciones básicas.

### Problema claramente definido

Debe conocerse qué queremos clasificar.

### Clases identificables

Las categorías deben estar correctamente definidas.

### Imágenes etiquetadas

Cada imagen debe asociarse con una única clase correcta.

### Cantidad suficiente

Debe existir un número razonable de ejemplos para cada categoría.

### Diversidad

Las imágenes deberían reflejar distintas condiciones.

### Calidad

Los archivos deben ser válidos y las etiquetas correctas.

### Consistencia

Las clases y criterios deben utilizarse uniformemente.

---

# 29. Ejemplo conductor: construcción del dataset de residuos

Retomemos el proyecto de clasificación de residuos.

El objetivo es clasificar:

* plástico;
* vidrio;
* cartón;
* metal.

Una primera propuesta podría considerar:

**1.000 imágenes por categoría**

Total:

**4.000 imágenes**

Sin embargo, antes de aceptarlas debemos verificar:

### Plástico

¿Incluye botellas, envases y diferentes colores?

### Vidrio

¿Incluye botellas, frascos y diferentes formas?

### Cartón

¿Incluye cajas, envases y diferentes tamaños?

### Metal

¿Incluye latas y otros objetos metálicos?

Además debemos evitar que una categoría esté asociada accidentalmente con un fondo específico.

Por ejemplo:

**todas las fotografías de plástico sobre fondo blanco**

y

**todas las fotografías de vidrio sobre fondo negro**

porque el modelo podría terminar aprendiendo:

**fondo → categoría**

en lugar de reconocer correctamente el material.

---

# 30. Datos y calidad del aprendizaje

Podemos resumir la relación de la siguiente manera:

**Problema mal definido**
→ datos inadecuados.

**Datos inadecuados**
→ entrenamiento deficiente.

**Entrenamiento deficiente**
→ modelo poco confiable.

Por el contrario:

**Problema claro**
↓
**Datos pertinentes**
↓
**Datos de calidad**
↓
**Entrenamiento adecuado**
↓
**Mayor posibilidad de generalización**

Por esta razón, preparar correctamente los datos no constituye una tarea secundaria.

Es una de las partes centrales de cualquier proyecto de Machine Learning.

---

# 31. Preguntas para discusión en clase

### Caso 1

Una empresa quiere clasificar cinco tipos de productos, pero dispone de 5.000 fotografías de una categoría y solamente 100 de cada una de las restantes.

**Pregunta:** ¿Qué problema presenta el dataset?

### Caso 2

Un estudiante descarga 10.000 imágenes, pero descubre que aproximadamente 3.000 son duplicadas.

**Pregunta:** ¿Por qué la cantidad total de archivos puede resultar engañosa?

### Caso 3

Un modelo será utilizado con fotografías tomadas mediante teléfonos móviles, pero fue entrenado exclusivamente con imágenes de estudio sobre fondo blanco.

**Pregunta:** ¿Qué problema puede aparecer?

### Caso 4

Una imagen de una botella de vidrio está almacenada dentro de la carpeta correspondiente a plástico.

**Pregunta:** ¿Qué tipo de problema de calidad representa?

### Caso 5

Una dupla quiere desarrollar un clasificador de cuatro especies de plantas.

**Pregunta:** ¿Qué aspectos debería considerar antes de comenzar a recolectar imágenes?

### Caso 6

Todas las imágenes de una clase se tomaron durante el día y las de otra clase durante la noche.

**Pregunta:** ¿Qué característica podría aprender accidentalmente el modelo?

---

# 32. Síntesis de la Semana 5

Al finalizar esta sesión deben quedar instaladas ocho ideas fundamentales:

1. **Los datos constituyen la materia prima del aprendizaje automático y su calidad condiciona directamente el modelo que podemos construir.**
2. **Un dataset puede contener datos estructurados o no estructurados; nuestro proyecto integrador trabaja principalmente con imágenes.**
3. **Los datos deben seleccionarse a partir del problema que queremos resolver y no únicamente en función de su disponibilidad.**
4. **En aprendizaje supervisado necesitamos entradas y etiquetas confiables.**
5. **Cantidad, calidad, diversidad y representatividad son dimensiones diferentes y todas deben considerarse al evaluar un dataset.**
6. **El desbalance, duplicados, errores de etiquetado e inconsistencias pueden perjudicar el entrenamiento y la evaluación.**
7. **Los datos atraviesan un ciclo que incluye recolección, inspección, limpieza, etiquetado, transformación, división, entrenamiento y mantenimiento.**

### Hacia la Semana 6

La Semana 5 respondió principalmente:

**¿Qué datos necesitamos y cómo sabemos si son adecuados?**

La siguiente sesión avanzará hacia una tarea práctica:

**¿Cómo transformamos un conjunto de imágenes disponibles en un dataset preparado para Machine Learning?**

La **Semana 6** abordará **preparación y etiquetado de datos**, incluyendo limpieza, organización, transformación, etiquetado y división inicial de conjuntos de imágenes.

