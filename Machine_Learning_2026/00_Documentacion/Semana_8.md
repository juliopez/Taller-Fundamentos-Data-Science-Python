# Semana 8 — Validación de modelos y aumento de datos

## 1. Propósito de la sesión

Comprender y aplicar estrategias que permitan evaluar de manera más robusta un modelo de Machine Learning y mejorar la diversidad efectiva del conjunto de entrenamiento mediante **validación cruzada** y **data augmentation**. 

Al finalizar la sesión, el estudiante debería ser capaz de:

* Explicar por qué una única partición de datos puede no ser suficiente para estimar el desempeño de un modelo.
* Comprender el propósito de la validación cruzada.
* Explicar el funcionamiento general de *k-fold cross-validation*.
* Reconocer cuándo la validación cruzada es útil y cuándo puede resultar costosa.
* Comprender la importancia de preservar la distribución de clases durante la validación.
* Diferenciar validación cruzada de conjunto de prueba.
* Explicar qué es *data augmentation*.
* Identificar transformaciones válidas e inválidas en imágenes.
* Comprender cómo el aumento de datos puede ayudar a reducir overfitting.
* Diseñar una estrategia de validación y aumento de datos para el proyecto integrador.
* Consolidar el dataset definitivo que será utilizado posteriormente para entrenar la CNN.

---

# 2. ¿Por qué necesitamos validar?

Durante la Semana 7 establecimos que un modelo no debe evaluarse únicamente utilizando los datos con los que fue entrenado.

La pregunta ahora es:

**¿Cómo podemos estimar de manera confiable si nuestro modelo funcionará bien con datos nuevos?**

Una primera estrategia consiste en dividir:

**Train / Validation / Test**

Sin embargo, una única división puede depender demasiado de cuáles observaciones terminaron en cada conjunto.

Por ejemplo, dos divisiones diferentes del mismo dataset podrían producir resultados distintos.

Esto introduce la necesidad de utilizar estrategias de validación más robustas.

---

# 3. Limitaciones de una única partición

Supongamos que tenemos:

**1.000 imágenes**

y realizamos una división:

**70% train**

**15% validation**

**15% test**

Es posible que, por azar, el conjunto de validación contenga imágenes especialmente fáciles o difíciles.

Entonces podríamos obtener:

**Validation accuracy = 92%**

Con otra división:

**Validation accuracy = 83%**

El modelo no cambió.

Lo que cambió fue la composición de los datos utilizados para validarlo.

Esto muestra que una única partición puede entregar una estimación sensible a la muestra seleccionada.

---

# 4. ¿Qué es la validación cruzada?

La **validación cruzada** es una estrategia que permite evaluar un modelo utilizando diferentes subconjuntos de los datos.

La idea general consiste en:

**dividir los datos varias veces**

y:

**entrenar y evaluar el modelo sobre distintas combinaciones**

Esto permite obtener una estimación más robusta de su comportamiento.

Una de las estrategias más conocidas es:

**k-fold cross-validation**

o:

**validación cruzada de k particiones**.

---

# 5. K-fold cross-validation

En *k-fold cross-validation*, el dataset se divide en:

**k subconjuntos aproximadamente del mismo tamaño**

denominados:

**folds**

Por ejemplo, si:

**k = 5**

tenemos:

**Fold 1**

**Fold 2**

**Fold 3**

**Fold 4**

**Fold 5**

El proceso utiliza cuatro folds para entrenamiento y uno para validación.

Después cambia el fold utilizado para validar.

Esto se repite hasta que todos los folds hayan sido utilizados como conjunto de validación.

---

# 6. Ejemplo con 5 folds

Supongamos:

**Dataset = 5.000 observaciones**

Dividimos en:

**5 folds de 1.000 observaciones**

### Iteración 1

Train:

**Fold 2 + 3 + 4 + 5**

Validation:

**Fold 1**

### Iteración 2

Train:

**Fold 1 + 3 + 4 + 5**

Validation:

**Fold 2**

### Iteración 3

Validation:

**Fold 3**

### Iteración 4

Validation:

**Fold 4**

### Iteración 5

Validation:

**Fold 5**

Finalmente disponemos de cinco resultados de validación.

---

# 7. Resultado de la validación cruzada

Supongamos que obtenemos:

| Fold | Accuracy |
| ---: | -------: |
|    1 |      87% |
|    2 |      90% |
|    3 |      88% |
|    4 |      91% |
|    5 |      89% |

Podemos calcular:

**Accuracy promedio = 89%**

Este resultado puede entregar una estimación más estable que utilizar únicamente una partición.

También podemos observar cuánto varían los resultados entre folds.

Por ejemplo:

**87% – 91%**

representa una variación relativamente pequeña.

En cambio:

**60% – 95%**

podría indicar que el comportamiento depende mucho de qué datos participan en el entrenamiento y validación.

---

# 8. ¿Qué nos aporta la validación cruzada?

La validación cruzada permite:

* utilizar mejor datasets pequeños;
* reducir dependencia de una única partición;
* comparar modelos;
* comparar hiperparámetros;
* estimar estabilidad del modelo;
* detectar sensibilidad a determinados subconjuntos.

Sin embargo, tiene un costo importante:

**el modelo debe entrenarse varias veces.**

Si utilizamos:

**k = 5**

debemos entrenar aproximadamente cinco modelos durante el proceso de validación.

---

# 9. Costo computacional

En algoritmos relativamente rápidos, la validación cruzada puede ser muy útil.

Sin embargo, en Deep Learning el entrenamiento puede requerir:

* muchos minutos;
* horas;
* GPU;
* considerable memoria.

Supongamos que entrenar una CNN tarda:

**40 minutos**

Con:

**5-fold cross-validation**

podríamos necesitar aproximadamente:

**5 × 40 minutos = 200 minutos**

para evaluar una sola configuración.

Por esta razón, en Deep Learning muchas veces se utilizan estrategias diferentes o una validación cruzada más limitada.

---

# 10. Validación cruzada y Deep Learning

El descriptor incluye la validación cruzada como contenido obligatorio, por lo que debemos comprender su lógica. 

Sin embargo, esto no significa que necesariamente debamos entrenar cinco o diez CNN completas para cada experimento del proyecto integrador.

En nuestro caso podremos utilizar principalmente:

**Train / Validation / Test**

como estructura operativa del proyecto.

La validación cruzada será comprendida como una estrategia adicional especialmente útil cuando:

* el dataset es pequeño;
* necesitamos comparar modelos;
* el costo de entrenamiento es razonable;
* necesitamos una estimación más robusta.

---

# 11. Stratified K-Fold

En problemas de clasificación debemos procurar que cada fold mantenga una distribución similar de las clases.

Supongamos un dataset:

```text
clase A: 500
clase B: 500
clase C: 500
clase D: 500
```

Una división aleatoria poco cuidadosa podría generar un fold con muy pocos ejemplos de una clase.

**Stratified K-Fold** busca conservar aproximadamente la proporción de clases en cada partición.

Esto resulta especialmente importante cuando existen clases menos frecuentes.

---

# 12. Validación cruzada versus conjunto de prueba

Es fundamental diferenciar ambos conceptos.

### Validación cruzada

Se utiliza durante el proceso de desarrollo.

Puede servir para:

* comparar modelos;
* elegir hiperparámetros;
* estimar estabilidad.

### Test set

Se utiliza al final.

Debe permanecer separado de las decisiones de desarrollo.

Por tanto:

**Cross-validation → desarrollo y selección**

**Test → evaluación final**

El conjunto de prueba continúa cumpliendo una función independiente.

---

# 13. ¿Podemos usar test dentro de los folds?

Para mantener una evaluación final independiente, una estrategia posible es:

**Dataset completo**

↓

**Train + Test**

Luego:

**Train**

↓

**Cross-validation**

y finalmente:

**modelo seleccionado**

↓

**Test**

De esta manera, el conjunto de prueba no participa en el proceso de selección.

Esto protege la validez de la evaluación final.

---

# 14. Validación con Scikit-learn

En problemas tradicionales de Machine Learning, Python permite realizar validación cruzada de manera sencilla.

Por ejemplo:

```python
from sklearn.model_selection import cross_val_score

scores = cross_val_score(
    modelo,
    X,
    y,
    cv=5
)

print(scores)
print(scores.mean())
```

Conceptualmente:

**cv=5**

indica:

**5-fold cross-validation**

El resultado contiene una métrica para cada partición.

---

# 15. Stratified K-Fold con Python

También puede definirse explícitamente una estrategia estratificada:

```python
from sklearn.model_selection import StratifiedKFold

cv = StratifiedKFold(
    n_splits=5,
    shuffle=True,
    random_state=42
)
```

Aquí:

**n_splits=5**

define cinco folds.

**shuffle=True**

mezcla los datos antes de dividirlos.

**random_state=42**

permite reproducir la división.

La reproducibilidad es importante porque facilita comparar experimentos.

---

# 16. Reproducibilidad

Un experimento de Machine Learning debería poder repetirse bajo condiciones similares.

Si realizamos una división aleatoria diferente cada vez, comparar resultados puede resultar difícil.

Por esta razón, muchas herramientas permiten fijar una **semilla aleatoria**.

Por ejemplo:

```python
random_state=42
```

Esto no significa que el número 42 tenga un significado especial para el modelo.

Simplemente permite repetir la misma secuencia pseudoaleatoria.

La idea central es:

**mismas condiciones → resultados comparables**

---

# 17. Evaluación del dataset antes del modelo

Validar no significa solamente calcular métricas sobre un modelo.

También debemos evaluar el dataset.

Antes de entrenar deberíamos revisar:

* cantidad de imágenes por clase;
* proporciones train/validation/test;
* duplicados;
* etiquetas;
* calidad visual;
* diversidad;
* dimensiones;
* formatos;
* representación de condiciones reales.

Por tanto:

**validación de datos**

y:

**validación de modelos**

son procesos relacionados, pero distintos.

---

# 18. ¿Qué es data augmentation?

El **data augmentation** o aumento de datos consiste en generar versiones modificadas de ejemplos existentes mediante transformaciones que preserven su categoría.

Por ejemplo:

**Imagen original → gato**

Podemos producir:

**Imagen rotada levemente → gato**

**Imagen desplazada → gato**

**Imagen volteada horizontalmente → gato**

Estas nuevas variantes permiten presentar al modelo ejemplos diferentes durante el entrenamiento.

El objetivo no es crear información completamente nueva, sino aumentar la **diversidad efectiva** del dataset.

---

# 19. ¿Por qué utilizar data augmentation?

En clasificación de imágenes es frecuente trabajar con datasets limitados.

Esto puede aumentar el riesgo de *overfitting*.

Si mostramos siempre exactamente las mismas imágenes, el modelo puede ajustarse demasiado a ellas.

Data augmentation introduce variaciones.

Por ejemplo:

**Imagen 1**

puede aparecer durante diferentes épocas como:

* versión original;
* rotación pequeña;
* cambio de zoom;
* desplazamiento;
* volteo.

Esto obliga al modelo a aprender patrones más robustos.

---

# 20. Data augmentation y generalización

La relación puede representarse así:

**Poca diversidad**

↓

**Modelo aprende detalles específicos**

↓

**Mayor riesgo de overfitting**

Con aumento de datos:

**Mayor diversidad efectiva**

↓

**Modelo enfrenta variaciones**

↓

**Mayor oportunidad de aprender patrones generales**

↓

**Mejor generalización potencial**

Sin embargo:

**data augmentation no garantiza automáticamente un buen modelo.**

Debe utilizarse de manera coherente con el problema.

---

# 21. Transformaciones habituales

Algunas transformaciones utilizadas en imágenes incluyen:

* rotación;
* volteo horizontal;
* desplazamiento;
* zoom;
* recorte;
* cambio moderado de brillo;
* contraste;
* traslación.

Por ejemplo:

**Imagen original**

↓

**Rotación +8°**

↓

misma clase.

La condición fundamental es:

**la transformación no debe cambiar el significado de la imagen.**

---

# 22. Transformaciones válidas e inválidas

No toda transformación es adecuada para todos los problemas.

Supongamos que clasificamos:

**perros / gatos**

Un volteo horizontal normalmente mantiene la clase.

Pero si clasificamos:

**flecha izquierda / flecha derecha**

un volteo horizontal puede cambiar completamente la etiqueta.

Ejemplo:

**←**

después del volteo:

**→**

Ya no pertenece a la misma clase.

Por tanto:

**una transformación válida depende del significado del problema.**

---

# 23. Rotación

Pequeñas rotaciones pueden resultar útiles cuando la orientación exacta del objeto no determina la clase.

Por ejemplo:

**manzana a 0°**

**manzana a 8°**

**manzana a -10°**

siguen siendo:

**manzana**

Sin embargo, rotaciones extremas podrían producir imágenes poco realistas.

El aumento de datos debería intentar representar **variaciones plausibles del mundo real**.

---

# 24. Flip horizontal y vertical

### Horizontal flip

Puede ser apropiado en numerosos objetos naturales.

Ejemplo:

**gato mirando a la izquierda**

↓

**gato mirando a la derecha**

La clase sigue siendo:

**gato**

### Vertical flip

Puede ser menos realista para determinados problemas.

Por ejemplo:

**automóvil invertido verticalmente**

podría no representar una situación habitual.

Por tanto, la transformación debe seleccionarse según el contexto.

---

# 25. Zoom y desplazamiento

El objeto puede aparecer a diferentes distancias o posiciones dentro de la fotografía.

Podemos utilizar:

### Zoom

Simula variaciones de escala.

### Translation

Desplaza ligeramente la imagen.

Estas técnicas ayudan a reducir dependencia excesiva de:

* tamaño exacto;
* posición exacta;
* encuadre específico.

Esto puede resultar especialmente útil si las fotografías finales serán tomadas por usuarios.

---

# 26. Brillo y contraste

Las condiciones de iluminación pueden cambiar.

Por ejemplo:

**misma botella**

puede fotografiarse:

* bajo luz intensa;
* en interior;
* en sombra.

Transformaciones moderadas de brillo y contraste pueden ayudar a representar esta variabilidad.

Sin embargo, cambios excesivos pueden destruir características importantes.

Nuevamente:

**el objetivo es producir ejemplos plausibles.**

---

# 27. Data augmentation no significa duplicar

Existe una diferencia importante entre:

**copiar una imagen**

y:

**aumentar una imagen**

Duplicado:

**imagen A → copia idéntica A**

No agrega diversidad.

Data augmentation:

**imagen A → A rotada**

**imagen A → A desplazada**

**imagen A → A con cambio moderado de brillo**

Estas variantes aportan diferencias que pueden ayudar al entrenamiento.

---

# 28. Augmentation solo en entrenamiento

Una regla muy importante:

**Data augmentation se aplica normalmente al conjunto de entrenamiento.**

No deberíamos aplicar transformaciones aleatorias al conjunto de prueba para hacerlo artificialmente diferente en cada evaluación.

Conceptualmente:

**Train → augmentation**

**Validation → transformación consistente**

**Test → transformación consistente**

Los conjuntos de validación y prueba deben representar de manera estable los datos sobre los que medimos desempeño.

---

# 29. Flujo correcto

Una estrategia puede ser:

**Dataset limpio**

↓

**Train / Validation / Test**

↓

### Train

Resize
Normalización
Data augmentation

### Validation

Resize
Normalización

### Test

Resize
Normalización

De esta manera, las transformaciones destinadas a aumentar diversidad afectan solamente el proceso de aprendizaje.

---

# 30. Data augmentation con Keras

Keras permite incorporar transformaciones directamente dentro del pipeline.

Por ejemplo:

```python
from tensorflow.keras import layers

data_augmentation = keras.Sequential([
    layers.RandomFlip("horizontal"),
    layers.RandomRotation(0.1),
    layers.RandomZoom(0.1)
])
```

Cada vez que una imagen atraviesa estas capas durante el entrenamiento puede recibir una transformación diferente.

Esto significa que no es necesario guardar físicamente miles de archivos adicionales.

---

# 31. Aumento dinámico de datos

Podemos distinguir dos formas generales.

### Offline augmentation

Generamos nuevas imágenes y las guardamos físicamente.

Por ejemplo:

```text
img001.jpg
img001_rotada.jpg
img001_zoom.jpg
```

### Online augmentation

Las transformaciones se generan dinámicamente durante el entrenamiento.

Ventajas:

* no aumenta significativamente el almacenamiento;
* produce variaciones diferentes;
* simplifica la gestión de archivos.

Frameworks modernos permiten trabajar fácilmente con este enfoque.

---

# 32. Data augmentation y etiquetas

Las transformaciones deben conservar correctamente la etiqueta.

Por ejemplo:

**Imagen original → plástico**

↓

**Rotación**

↓

**plástico**

El cambio afecta:

**X**

pero no:

**y**

Por tanto:

**transformación válida = modifica entrada sin cambiar la clase real**

Esta regla es esencial.

---

# 33. ¿Cuánto augmentation aplicar?

No existe una configuración universal.

Si las transformaciones son demasiado pequeñas, quizá aporten poca diversidad.

Si son demasiado fuertes, pueden producir ejemplos irreales.

Por ejemplo:

**rotación de 5°**

puede ser razonable.

Pero:

**rotación aleatoria de 170°**

puede no representar adecuadamente las imágenes que encontraremos en producción.

La configuración debería responder a:

**¿Qué variaciones son plausibles en el contexto real?**

---

# 34. Visualizar antes de utilizar

Antes de entrenar con augmentation resulta recomendable observar ejemplos transformados.

Por ejemplo:

```python
import matplotlib.pyplot as plt

for images, labels in train_ds.take(1):
    augmented = data_augmentation(images)

    plt.imshow(augmented[0].numpy().astype("uint8"))
    plt.axis("off")
    plt.show()
```

La inspección visual permite determinar si las transformaciones producen imágenes razonables.

No deberíamos aplicar aumentos solamente porque una biblioteca los ofrece.

---

# 35. Augmentation y clases específicas

En algunos proyectos puede existir un desbalance de clases.

Podría surgir la idea de aplicar más augmentation a las clases minoritarias.

Esta estrategia puede ayudar en determinados casos, pero debe utilizarse con cuidado.

Generar muchas variaciones de pocas imágenes originales no equivale a disponer de gran cantidad de observaciones realmente independientes.

Por ejemplo:

**20 imágenes originales**

transformadas en:

**2.000 variantes**

siguen teniendo únicamente:

**20 fuentes reales de información**

Por tanto, augmentation complementa la recopilación de datos, pero no siempre la reemplaza.

---

# 36. Evaluar antes y después del augmentation

Una forma adecuada de estudiar su efecto consiste en comparar experimentos.

### Modelo A

Sin aumento de datos.

### Modelo B

Con aumento de datos.

Después comparar:

* train accuracy;
* validation accuracy;
* train loss;
* validation loss;
* diferencia entre train y validation.

Podría ocurrir:

### Sin augmentation

```text
Train: 98%
Validation: 76%
```

### Con augmentation

```text
Train: 91%
Validation: 87%
```

Aunque disminuyó el desempeño sobre entrenamiento, mejoró la generalización.

---

# 37. Validación de datos en el proyecto integrador

**¿Cuántas imágenes posee?**

**¿Cuántas clases?**

**¿Cuántas imágenes por clase?**

**¿De dónde provienen?**

**¿Cómo fueron etiquetadas?**

**¿Qué problemas fueron eliminados?**

**¿Cómo se dividieron train, validation y test?**

**¿Se preservó la distribución de clases?**

**¿Qué transformaciones se aplicarán?**

**¿Qué estrategia de data augmentation utilizarán?**

Estas respuestas forman parte de la justificación técnica del dataset.

---

# 38. Producto esperado al cerrar la UA2

Al terminar esta unidad, cada dupla debería poseer:

**1. Problema de clasificación claramente definido**

↓

**2. Clases identificadas**

↓

**3. Colección de imágenes válida**

↓

**4. Etiquetado consistente**

↓

**5. Dataset limpio**

↓

**6. Datos organizados**

↓

**7. Train / Validation / Test**

↓

**8. Estrategia de aumento de datos**

↓

**9. Dataset definitivo**

Este será el insumo directo para la Unidad 3:

**Deep Learning y entrenamiento de la CNN.**

---

# 39. Ejemplo conductor: dataset definitivo de residuos

Supongamos que después de la limpieza tenemos:

```text
4.400 imágenes
```

Distribuidas en:

```text
plástico: 1.100
vidrio: 1.100
cartón: 1.100
metal: 1.100
```

División:

```text
Train: 70% = 3.080
Validation: 15% = 660
Test: 15% = 660
```

Configuración:

```text
Resolución: 224 × 224
Color: RGB
Normalización: 0–1
```

Data augmentation para entrenamiento:

```text
Horizontal flip
Rotación moderada
Zoom moderado
Pequeñas variaciones de contraste
```

Sin augmentation:

```text
Validation y Test
```

El dataset queda entonces preparado para el entrenamiento posterior de la CNN.

---

# 40. Preguntas para discusión en clase

### Caso 1

Un modelo obtiene 91% utilizando una determinada división y 76% utilizando otra.

**Pregunta:** ¿Qué podría indicarnos esta variación y qué estrategia permitiría evaluar el modelo de manera más robusta?

### Caso 2

Tenemos cuatro clases y utilizamos 5-fold cross-validation.

**Pregunta:** ¿Por qué sería conveniente mantener proporciones similares de las clases en cada fold?

### Caso 3

Una dupla utiliza el test set dentro de la validación cruzada y luego reporta nuevamente su resultado como evaluación final.

**Pregunta:** ¿Qué problema presenta esta metodología?

### Caso 4

Para clasificar flechas izquierda/derecha se utiliza `RandomFlip("horizontal")`.

**Pregunta:** ¿Es una transformación adecuada? ¿Por qué?

### Caso 5

Un estudiante genera 5.000 copias exactas de 500 imágenes y afirma haber realizado data augmentation.

**Pregunta:** ¿Es correcto?

### Caso 6

La CNN muestra menor accuracy de entrenamiento después de incorporar augmentation, pero aumenta considerablemente el desempeño en validación.

**Pregunta:** ¿Cómo interpretaríamos este resultado?

### Caso 7

Una transformación convierte una imagen perfectamente reconocible en otra deformada que no podría aparecer en el contexto real.

**Pregunta:** ¿Debería mantenerse en el pipeline de augmentation?

---

# 41. Síntesis de la Semana 8

Al finalizar esta sesión deben quedar instaladas ocho ideas fundamentales:

1. **Una única partición de datos puede producir una estimación sensible a la muestra utilizada para validar.**
2. **La validación cruzada utiliza diferentes particiones para estimar de manera más robusta el comportamiento de un modelo.**
3. **K-fold divide los datos en k subconjuntos y utiliza cada uno de ellos como validación en diferentes iteraciones.**
4. **El conjunto de prueba debe mantenerse independiente de los procesos de selección y ajuste del modelo.**
5. **Data augmentation genera variaciones plausibles de los ejemplos existentes manteniendo su etiqueta.**
6. **El aumento de datos busca incrementar la diversidad efectiva y puede contribuir a reducir overfitting y mejorar generalización.**
7. **Las transformaciones deben seleccionarse según el problema: una operación válida para un dataset puede cambiar completamente la clase en otro.**
8. **Al cerrar la Unidad 2 debemos disponer de un dataset limpio, etiquetado, dividido, documentado y preparado para entrenar la CNN.**

### Hacia la Semana 9

Con esta sesión cerramos la **Unidad 2: Recolección y preparación de datos**.

La progresión ha sido:

**Semana 5:** ¿Qué datos necesitamos?

**Semana 6:** ¿Cómo los limpiamos, etiquetamos, transformamos y dividimos?

**Semana 7:** ¿Cómo aseguramos que el modelo generalice y no simplemente memorice?

**Semana 8:** ¿Cómo validamos de manera más robusta y aumentamos la diversidad de los datos?

Hasta este momento tenemos:

**Problema definido + Dataset preparado**

La siguiente etapa comienza con una nueva pregunta:

**¿Cómo utilizamos estos datos para construir una solución de Deep Learning con mayor capacidad de representación?**

La **Semana 9** iniciará la Unidad 3: **Deep Learning**, abordando redes neuronales profundas, arquitectura, capas, función de pérdida, optimización y proceso de entrenamiento.

