# Semana 6 — Preparación y etiquetado de datos

## 1. Propósito de la sesión

Comprender y aplicar las principales tareas necesarias para transformar un conjunto de imágenes disponibles en un **dataset preparado para Machine Learning**, considerando limpieza, organización, etiquetado, transformación y división inicial de los datos.

Al finalizar la sesión, el estudiante debería ser capaz de:

* Identificar las principales etapas de preparación de un dataset.
* Detectar y eliminar imágenes inválidas, corruptas o irrelevantes.
* Organizar imágenes de manera consistente según sus clases.
* Comprender la importancia de criterios de etiquetado claros.
* Aplicar transformaciones básicas sobre imágenes.
* Comprender el propósito del redimensionamiento y la normalización.
* Diferenciar datos originales de datos transformados.
* Comprender por qué es necesario separar los datos antes del entrenamiento.
* Identificar conjuntos de entrenamiento, validación y prueba.
* Relacionar estas tareas con la construcción del dataset definitivo del proyecto integrador.

---

# 2. Desde datos disponibles hacia datos utilizables

Durante la Semana 5 establecimos que disponer de muchas imágenes no significa necesariamente disponer de un buen dataset.

Un conjunto de imágenes puede contener:

* archivos corruptos;
* duplicados;
* imágenes mal etiquetadas;
* resoluciones diferentes;
* formatos distintos;
* clases desbalanceadas;
* imágenes irrelevantes.

Por tanto, entre:

**Recolectar datos**

y:

**Entrenar un modelo**

existe una etapa crítica:

**Preparar los datos**

El flujo general puede representarse así:

**Datos recopilados**
↓
**Inspección**
↓
**Limpieza**
↓
**Organización**
↓
**Etiquetado**
↓
**Transformación**
↓
**División**
↓
**Dataset preparado**

---

# 3. ¿Qué significa preparar datos?

La **preparación de datos** corresponde al conjunto de tareas destinadas a convertir información disponible en una representación consistente y adecuada para ser utilizada por un modelo.

En clasificación de imágenes puede incluir:

* verificar archivos;
* eliminar imágenes corruptas;
* revisar etiquetas;
* organizar clases;
* renombrar archivos;
* convertir formatos;
* redimensionar imágenes;
* normalizar valores;
* separar entrenamiento, validación y prueba.

Estas tareas persiguen un objetivo común:

**reducir problemas en los datos antes de que afecten el entrenamiento.**

---

# 4. Inspección inicial

Antes de modificar el dataset debemos comprender su estado actual.

Algunas preguntas iniciales son:

**¿Cuántas imágenes existen?**

**¿Cuántas clases tenemos?**

**¿Cuántas imágenes posee cada clase?**

**¿Qué formatos aparecen?**

**¿Qué dimensiones tienen las imágenes?**

**¿Existen archivos corruptos?**

**¿Las etiquetas parecen correctas?**

Por ejemplo:

```text
Dataset original

Total: 4.480 imágenes

plástico: 1.310
vidrio: 1.020
cartón: 1.090
metal: 1.060
```

Esta inspección constituye una línea base antes de comenzar la limpieza.

---

# 5. Limpieza del dataset

La limpieza consiste en identificar y corregir o eliminar observaciones problemáticas.

En imágenes podemos encontrar diferentes situaciones.

### Archivos corruptos

La imagen no puede abrirse correctamente.

### Imágenes irrelevantes

El archivo no representa ninguna de las clases del problema.

### Imágenes excesivamente borrosas

No contienen información suficiente para identificar el objeto.

### Duplicados

La misma imagen aparece varias veces.

### Etiquetas incorrectas

La imagen pertenece a una clase diferente de aquella donde fue almacenada.

### Archivos no deseados

Por ejemplo:

```text
Thumbs.db
.DS_Store
archivo.txt
```

Estos elementos no deberían formar parte del dataset utilizado por el modelo.

---

# 6. Detección de imágenes corruptas

Python puede ayudarnos a verificar que las imágenes sean válidas.

Por ejemplo:

```python
from PIL import Image
import os

ruta = "dataset"

for carpeta, subcarpetas, archivos in os.walk(ruta):
    for archivo in archivos:
        ruta_archivo = os.path.join(carpeta, archivo)

        try:
            with Image.open(ruta_archivo) as img:
                img.verify()

        except Exception:
            print("Archivo problemático:", ruta_archivo)
```

Este procedimiento permite identificar archivos que no pueden ser interpretados correctamente como imágenes.

Después de detectarlos, será necesario decidir si corresponde:

* eliminarlos;
* reemplazarlos;
* recuperarlos desde la fuente original.

---

# 7. Duplicados

Los duplicados pueden aparecer porque:

* una imagen fue descargada varias veces;
* un archivo fue copiado accidentalmente;
* diferentes nombres corresponden al mismo contenido.

Por ejemplo:

```text
img001.jpg
img001_copia.jpg
foto_plastico_15.jpg
```

podrían representar exactamente la misma fotografía.

Los duplicados pueden producir una falsa sensación de cantidad y diversidad.

Además, si una copia queda en entrenamiento y otra en prueba, la evaluación podría resultar artificialmente alta.

Por esta razón:

**limpiar duplicados debe ocurrir antes de dividir el dataset.**

---

# 8. Organización del dataset

Una estructura ordenada facilita tanto la administración del proyecto como la utilización de herramientas de Machine Learning.

Una organización frecuente para clasificación es:

```text
dataset/
│
├── plastico/
│   ├── img001.jpg
│   ├── img002.jpg
│   └── ...
│
├── vidrio/
│   ├── img001.jpg
│   ├── img002.jpg
│   └── ...
│
├── carton/
│   ├── img001.jpg
│   ├── img002.jpg
│   └── ...
│
└── metal/
    ├── img001.jpg
    ├── img002.jpg
    └── ...
```

Cada carpeta representa una clase.

Esta estructura permite que muchas bibliotecas identifiquen automáticamente las etiquetas a partir del nombre de las carpetas.

---

# 9. Convenciones de nombres

La consistencia también debe mantenerse en los nombres utilizados.

Por ejemplo, evitar simultáneamente:

```text
plastico
Plastico
PLÁSTICO
plasticos
```

porque podrían interpretarse como categorías distintas.

Una convención adecuada podría utilizar:

* minúsculas;
* sin espacios;
* sin caracteres especiales;
* nombres breves y consistentes.

Por ejemplo:

```text
plastico
vidrio
carton
metal
```

La misma regla debería mantenerse durante todo el proyecto.

---

# 10. ¿Qué significa etiquetar?

El **etiquetado** consiste en asociar cada imagen con la categoría correcta.

En clasificación supervisada, esta información es indispensable porque durante el entrenamiento necesitamos comparar:

**Predicción del modelo**

con:

**Etiqueta real**

Por ejemplo:

| Imagen     | Etiqueta |
| ---------- | -------- |
| img001.jpg | plástico |
| img002.jpg | vidrio   |
| img003.jpg | cartón   |

Un error de etiquetado representa directamente una señal de aprendizaje incorrecta.

Por tanto:

**la calidad de las etiquetas es parte de la calidad del dataset.**

---

# 11. Definir criterios antes de etiquetar

Cuando las clases son simples, el etiquetado puede parecer evidente.

Sin embargo, algunos casos pueden generar ambigüedad.

Supongamos que clasificamos residuos y aparece:

**una caja de cartón con componentes plásticos.**

¿Debe etiquetarse como:

* cartón;
* plástico;
* mixto?

La respuesta depende del objetivo del proyecto.

Por ello, antes de etiquetar es recomendable definir **criterios claros**.

Por ejemplo:

> La clase se asignará según el material predominante visible en la imagen.

Este tipo de criterio ayuda a mantener consistencia entre diferentes personas que participan del proceso.

---

# 12. Etiquetado y ambigüedad

No todas las imágenes deberían necesariamente incorporarse al dataset.

Si una observación no puede clasificarse con seguridad, existen opciones como:

* eliminarla;
* revisarla nuevamente;
* crear una nueva categoría si tiene sentido;
* definir una regla explícita para casos similares.

Incorporar una imagen ambigua con una etiqueta arbitraria puede perjudicar el aprendizaje.

En términos simples:

**si una persona experta no puede determinar claramente la clase, probablemente tampoco sea un buen ejemplo de entrenamiento.**

---

# 13. Etiquetado manual

Para proyectos pequeños o medianos, el etiquetado puede realizarse manualmente.

Por ejemplo:

1. observar la imagen;
2. determinar su categoría;
3. moverla a la carpeta correspondiente.

Este método puede ser suficiente para un proyecto académico.

Sin embargo, requiere:

* tiempo;
* criterios consistentes;
* revisión posterior.

Cuando participan varias personas, es recomendable acordar previamente qué representa cada clase.

---

# 14. Etiquetado mediante herramientas

En proyectos más grandes pueden utilizarse herramientas especializadas de anotación.

Estas permiten:

* cargar imágenes;
* asignar categorías;
* revisar etiquetas;
* exportar anotaciones;
* coordinar varios anotadores.

Para nuestro proyecto de **clasificación de imágenes**, no necesitamos necesariamente herramientas complejas de anotación espacial, porque no estamos delimitando objetos dentro de la imagen.

Nuestro requerimiento principal es:

**una imagen → una clase**

Esto simplifica considerablemente el proceso.

---

# 15. Formatos de imagen

Un dataset puede incluir imágenes en formatos diferentes:

* JPG;
* JPEG;
* PNG;
* WEBP;
* BMP.

Aunque muchas bibliotecas soportan múltiples formatos, es conveniente conocer qué archivos contiene el dataset.

Puede ser recomendable convertirlos hacia un conjunto reducido de formatos, por ejemplo:

**JPG y PNG**

Esto facilita:

* carga;
* validación;
* consistencia;
* mantenimiento.

La conversión debe realizarse evitando pérdidas innecesarias de información.

---

# 16. Dimensiones diferentes

Las imágenes recopiladas pueden presentar dimensiones muy diferentes.

Por ejemplo:

```text
640 × 480
800 × 600
1920 × 1080
4000 × 3000
```

Sin embargo, los modelos normalmente requieren entradas de dimensiones consistentes.

Por ejemplo:

```text
128 × 128
224 × 224
256 × 256
```

Esto implica aplicar una transformación de **redimensionamiento**.

---

# 17. Redimensionamiento

El redimensionamiento permite que todas las imágenes posean dimensiones compatibles con la entrada del modelo.

Por ejemplo:

**Imagen original: 1920 × 1080**

↓

**Resize**

↓

**Imagen preparada: 224 × 224**

En Python:

```python
from PIL import Image

imagen = Image.open("imagen.jpg")
imagen = imagen.resize((224, 224))
```

Sin embargo, debemos ser cuidadosos.

Cambiar arbitrariamente ancho y alto puede deformar la imagen si altera demasiado su relación de aspecto.

Existen diferentes estrategias para abordar este problema, entre ellas:

* redimensionar directamente;
* recortar;
* agregar relleno;
* mantener proporciones.

---

# 18. Relación de aspecto

La **relación de aspecto** representa la proporción entre ancho y alto.

Por ejemplo:

**1920 × 1080 → 16:9**

Si transformamos directamente esta imagen a:

**224 × 224**

la convertimos en una imagen cuadrada.

Esto puede provocar deformación.

Dependiendo del problema podemos utilizar operaciones como:

**resize + crop**

o:

**resize + padding**

El objetivo es obtener dimensiones consistentes intentando preservar la información relevante.

---

# 19. Canales de color

Las imágenes también pueden tener diferentes representaciones.

### Escala de grises

Habitualmente posee un canal.

Conceptualmente:

**alto × ancho × 1**

### RGB

Posee tres canales:

**Rojo**

**Verde**

**Azul**

Conceptualmente:

**alto × ancho × 3**

Por ejemplo:

```text
224 × 224 × 3
```

La mayoría de los proyectos de clasificación con fotografías utilizarán imágenes RGB.

Es importante que el proceso de preparación mantenga una representación coherente.

---

# 20. Representación numérica de una imagen

Los píxeles se representan numéricamente.

En imágenes RGB de 8 bits, cada componente suele tomar valores entre:

**0 y 255**

Por ejemplo:

```text
R = 255
G = 0
B = 0
```

representa rojo intenso.

Una imagen completa puede verse como una gran estructura de números.

El modelo no procesa directamente conceptos como:

**“botella”**

o:

**“cartón”**

sino estos valores numéricos y las representaciones aprendidas a partir de ellos.

---

# 21. Normalización

Una transformación habitual consiste en llevar los valores de los píxeles desde:

**0 – 255**

hacia un rango más pequeño, por ejemplo:

**0 – 1**

Esto puede realizarse mediante:

**valor normalizado = valor original / 255**

Ejemplo:

| Original | Normalizado |
| -------: | ----------: |
|        0 |           0 |
|       64 |       0,251 |
|      128 |       0,502 |
|      255 |           1 |

En Python:

```python
imagen = imagen / 255.0
```

La normalización puede facilitar el entrenamiento de redes neuronales al trabajar con escalas numéricas más controladas.

---

# 22. Transformar no significa alterar la etiqueta

Cuando redimensionamos o normalizamos una imagen estamos cambiando su **representación**, pero no su significado.

Por ejemplo:

**imagen original → plástico**

↓

**resize**

↓

**imagen 224 × 224 → plástico**

↓

**normalización**

↓

**tensor numérico → plástico**

La etiqueta permanece igual.

Por ello debemos mantener correctamente la asociación:

**entrada transformada ↔ etiqueta original**

---

# 23. Separación de los datos

Después de limpiar y preparar el dataset necesitamos separar los ejemplos según la función que cumplirán.

La estructura más común contempla:

* **entrenamiento**;
* **validación**;
* **prueba**.

Conceptualmente:

**Dataset completo**
↓
**Train + Validation + Test**

Cada conjunto tiene un propósito distinto.

---

# 24. Conjunto de entrenamiento

El **training set** contiene los ejemplos utilizados directamente para ajustar los parámetros del modelo.

Durante el entrenamiento:

**X_train + y_train**

son utilizados para:

* realizar predicciones;
* calcular la pérdida;
* ejecutar *backpropagation*;
* actualizar pesos y sesgos.

Por tanto:

**el modelo aprende directamente de estos datos.**

Habitualmente constituye la mayor parte del dataset.

---

# 25. Conjunto de validación

El **validation set** permite observar el comportamiento del modelo durante su desarrollo sin utilizar esos ejemplos directamente para actualizar sus parámetros.

Puede utilizarse para:

* detectar problemas de generalización;
* comparar configuraciones;
* decidir cuándo detener el entrenamiento;
* seleccionar hiperparámetros;
* comparar modelos.

Conceptualmente:

**Entrenamos con train**

pero:

**tomamos decisiones utilizando validation**

Esta distinción será especialmente importante durante la Semana 7.

---

# 26. Conjunto de prueba

El **test set** se reserva para realizar una evaluación final del modelo.

Idealmente no debería utilizarse repetidamente para tomar decisiones durante el desarrollo.

Conceptualmente:

**Train → aprender**

**Validation → ajustar decisiones**

**Test → evaluar finalmente**

El conjunto de prueba intenta responder:

**¿Cómo funciona el modelo ante datos que realmente no participaron en su construcción?**

---

# 27. Ejemplo de división

Supongamos un dataset con:

**10.000 imágenes**

Podríamos utilizar, por ejemplo:

**70% entrenamiento**

**15% validación**

**15% prueba**

Lo que corresponde a:

| Conjunto      | Imágenes |
| ------------- | -------: |
| Entrenamiento |    7.000 |
| Validación    |    1.500 |
| Prueba        |    1.500 |

Estas proporciones no constituyen una regla universal.

Pueden variar según:

* tamaño del dataset;
* complejidad del problema;
* disponibilidad de datos.

Lo importante es mantener conjuntos suficientemente representativos.

---

# 28. División estratificada

En clasificación debemos procurar que cada conjunto mantenga una representación razonable de las diferentes clases.

Supongamos:

```text
plástico: 1.000
vidrio: 1.000
cartón: 1.000
metal: 1.000
```

Una buena división debería evitar situaciones como:

**entrenamiento → casi todo plástico y vidrio**

**prueba → casi todo cartón y metal**

Una división **estratificada** busca conservar aproximadamente las proporciones de cada clase entre los diferentes conjuntos.

Esto permite una evaluación más consistente.

---

# 29. Evitar fuga de datos

Un concepto importante durante la preparación es la **fuga de datos** o *data leakage*.

Ocurre cuando información que no debería estar disponible durante el entrenamiento termina influyendo en el modelo.

Un ejemplo sencillo:

**imagen original → entrenamiento**

**misma imagen duplicada → prueba**

El modelo podría obtener un resultado excelente porque ya ha visto prácticamente ese ejemplo.

Otro caso:

varias fotografías casi idénticas obtenidas de una misma secuencia terminan distribuidas entre entrenamiento y prueba.

Esto reduce la independencia entre los conjuntos.

---

# 30. El orden importa

Una secuencia adecuada es:

**1. Recopilar**

↓

**2. Limpiar**

↓

**3. Revisar etiquetas**

↓

**4. Eliminar duplicados**

↓

**5. Organizar**

↓

**6. Dividir**

↓

**7. Aplicar transformaciones según corresponda**

Este orden evita algunos problemas.

Por ejemplo, si dividimos antes de eliminar duplicados, podríamos terminar con copias de una misma imagen en diferentes conjuntos.

---

# 31. Estructura física después de dividir

Una posible organización final es:

```text
dataset/
│
├── train/
│   ├── plastico/
│   ├── vidrio/
│   ├── carton/
│   └── metal/
│
├── validation/
│   ├── plastico/
│   ├── vidrio/
│   ├── carton/
│   └── metal/
│
└── test/
    ├── plastico/
    ├── vidrio/
    ├── carton/
    └── metal/
```

Esta organización resulta clara y puede ser utilizada directamente por numerosas herramientas de Deep Learning.

---

# 32. Carga de imágenes con Keras

Keras permite crear datasets directamente desde una estructura de carpetas.

Por ejemplo:

```python
from tensorflow.keras.utils import image_dataset_from_directory

train_ds = image_dataset_from_directory(
    "dataset/train",
    image_size=(224, 224),
    batch_size=32
)
```

La biblioteca puede:

* detectar las carpetas;
* identificar las clases;
* cargar imágenes;
* redimensionarlas;
* generar lotes.

Esto muestra por qué una buena organización previa simplifica considerablemente el desarrollo posterior.

---

# 33. Normalización mediante Keras

La normalización también puede incorporarse directamente dentro del modelo o pipeline.

Por ejemplo:

```python
from tensorflow.keras import layers

normalizacion = layers.Rescaling(1./255)
```

Si un píxel posee originalmente:

**255**

pasará a:

**1**

Si posee:

**128**

pasará aproximadamente a:

**0,50**

El objetivo no es alterar el contenido de la imagen, sino cambiar su escala numérica.

---

# 34. Dataset original y dataset procesado

Es recomendable diferenciar:

**Datos originales**

de:

**Datos procesados**

Por ejemplo:

```text
proyecto/
│
├── data_raw/
│
└── data_processed/
```

Esto permite:

* conservar los archivos originales;
* repetir el procesamiento;
* corregir errores;
* documentar transformaciones;
* comparar versiones.

Una mala práctica sería modificar permanentemente los únicos archivos originales sin mantener respaldo.

---

# 35. Trazabilidad

La **trazabilidad** significa poder explicar qué ocurrió con los datos desde su origen hasta su utilización.

Por ejemplo:

**Fuente original**
↓
**4.520 imágenes descargadas**
↓
**80 eliminadas por corrupción**
↓
**140 duplicados eliminados**
↓
**4.300 imágenes válidas**
↓
**redimensionamiento a 224 × 224**
↓
**división train/validation/test**

Esta información resulta especialmente importante cuando debemos justificar cómo construimos nuestro dataset.

---

# 36. Documentar el dataset

Una documentación básica debería indicar:

* origen de las imágenes;
* fecha de obtención;
* clases;
* cantidad por clase;
* criterios de inclusión;
* criterios de exclusión;
* problemas encontrados;
* limpieza realizada;
* dimensiones finales;
* formato;
* distribución train/validation/test.

Por ejemplo:

```text
Dataset final: 4.200 imágenes

Clases:
- plástico: 1.050
- vidrio: 1.050
- cartón: 1.050
- metal: 1.050

Resolución: 224 × 224 RGB

División:
- Train: 70%
- Validation: 15%
- Test: 15%
```

---

# 37. Aplicación al proyecto integrador

La segunda etapa del proyecto consiste precisamente en construir el **dataset definitivo**.

Cada dupla tendrá que demostrar que su conjunto de datos no es simplemente una colección de archivos.

Deberá existir una lógica documentada de:

**Problema**
↓
**Clases**
↓
**Obtención de imágenes**
↓
**Revisión**
↓
**Limpieza**
↓
**Etiquetado**
↓
**Transformación**
↓
**División**


---

# 38. Ejemplo conductor: preparación del dataset de residuos

Supongamos que recopilamos inicialmente:

```text
4.650 imágenes
```

Durante la inspección encontramos:

```text
70 archivos corruptos
130 duplicados
50 imágenes irrelevantes
```

Después de la limpieza:

```text
4.400 imágenes válidas
```

Distribución:

```text
plástico: 1.100
vidrio: 1.100
cartón: 1.100
metal: 1.100
```

Posteriormente:

**Redimensionamiento → 224 × 224 RGB**

y división:

**70% train → 3.080**

**15% validation → 660**

**15% test → 660**

El resultado ya no es simplemente una colección de fotografías.

Es un **dataset estructurado para un proceso de Machine Learning**.

---

# 39. Preguntas para discusión en clase

### Caso 1

Una dupla posee imágenes de cinco clases, pero utiliza los nombres:

```text
perro
Perro
PERROS
dog
```

**Pregunta:** ¿Qué problema presenta esta organización?

### Caso 2

Una imagen aparece tanto en entrenamiento como en prueba.

**Pregunta:** ¿Por qué puede afectar la evaluación del modelo?

### Caso 3

Un dataset contiene imágenes de 4000 × 3000, 800 × 600 y 224 × 224.

**Pregunta:** ¿Qué transformación probablemente necesitaremos antes de utilizarlas en una red?

### Caso 4

Una fotografía de plástico fue etiquetada accidentalmente como vidrio.

**Pregunta:** ¿Qué impacto puede tener este error durante el aprendizaje?

### Caso 5

Una dupla modifica todas las imágenes originales sin conservar copia.

**Pregunta:** ¿Qué problema puede generar esto para la trazabilidad del proyecto?

### Caso 6

Un dataset contiene cuatro clases equilibradas, pero después de dividirlo una clase prácticamente no aparece en el conjunto de prueba.

**Pregunta:** ¿Qué aspecto debería revisarse en la estrategia de división?

---

# 40. Síntesis de la Semana 6

Al finalizar esta sesión deben quedar instaladas ocho ideas fundamentales:

1. **Preparar datos significa transformar información disponible en un dataset consistente y utilizable por un modelo.**
2. **La limpieza debe identificar archivos corruptos, duplicados, ejemplos irrelevantes y errores de etiquetado.**
3. **El etiquetado requiere clases y criterios claramente definidos.**
4. **Las imágenes normalmente deben adoptar dimensiones, formatos y representaciones consistentes antes del entrenamiento.**
5. **El redimensionamiento y la normalización modifican la representación de la imagen, pero no su etiqueta.**
6. **Los datos deben separarse en entrenamiento, validación y prueba porque cada conjunto cumple una función diferente.**
7. **Debemos evitar fugas de datos y mantener una distribución razonable de las clases entre los conjuntos.**
8. **El dataset final debe ser reproducible y documentado, porque constituirá el insumo directo para el entrenamiento posterior de la CNN.**

### Hacia la Semana 7

Hasta ahora hemos respondido:

**Semana 5:** ¿Qué datos necesitamos y cómo evaluamos si son adecuados?

**Semana 6:** ¿Cómo limpiamos, organizamos, etiquetamos y dividimos esos datos?

La siguiente pregunta será:

**¿Cómo sabemos si nuestro modelo realmente aprendió y no simplemente memorizó los datos de entrenamiento?**

La **Semana 7** abordará **generalización y overfitting**, profundizando en la relación entre entrenamiento, validación y prueba y en los problemas que aparecen cuando un modelo se ajusta excesivamente a sus datos de entrenamiento.

