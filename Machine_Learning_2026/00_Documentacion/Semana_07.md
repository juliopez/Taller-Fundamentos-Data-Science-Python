# Semana 7 — Generalización y Overfitting

## 1. Propósito de la sesión

Comprender la capacidad de un modelo de Machine Learning para **generalizar correctamente frente a datos nuevos**, identificando el fenómeno de *overfitting*, sus principales causas, señales y consecuencias, así como la función de los conjuntos de entrenamiento, validación y prueba dentro del proceso de evaluación.

Al finalizar la sesión, el estudiante debería ser capaz de:

* Explicar qué significa que un modelo generalice.
* Diferenciar aprendizaje real de memorización.
* Identificar el fenómeno de *overfitting*.
* Reconocer señales de sobreajuste a partir del comportamiento de entrenamiento y validación.
* Comprender la función diferenciada de los conjuntos de entrenamiento, validación y prueba.
* Relacionar complejidad del modelo y cantidad/calidad de datos con el riesgo de sobreajuste.
* Comprender la relación entre sesgo y varianza.
* Identificar estrategias generales para reducir el *overfitting*.
* Relacionar estos conceptos con la preparación del dataset y el posterior entrenamiento de la CNN del proyecto integrador.

---

# 2. El objetivo no es memorizar

Durante la Semana 1 introdujimos una idea fundamental:

**Machine Learning no significa memorizar los datos de entrenamiento.**

Un modelo puede aprender muy bien los ejemplos que ya ha visto y, aun así, comportarse mal frente a datos nuevos.

Supongamos que entrenamos un clasificador con 5.000 imágenes.

Después del entrenamiento obtenemos:

**Accuracy entrenamiento = 99%**

A primera vista podría parecer un excelente resultado.

Sin embargo, cuando utilizamos imágenes nuevas:

**Accuracy prueba = 63%**

Esto significa que el modelo aprendió demasiado bien las particularidades de los datos de entrenamiento, pero no logró capturar patrones suficientemente generales.

El verdadero objetivo es:

**aprender patrones útiles que funcionen también con datos no vistos.**

---

# 3. ¿Qué es generalización?

La **generalización** es la capacidad de un modelo para mantener un buen desempeño sobre datos nuevos que no fueron utilizados directamente durante su entrenamiento.

Podemos representarlo así:

**Datos de entrenamiento**
↓
**Modelo aprende patrones**
↓
**Datos nuevos**
↓
**Predicciones correctas**

Un modelo generaliza correctamente cuando lo aprendido durante el entrenamiento puede aplicarse a observaciones nuevas.

Por ejemplo, si entrenamos un clasificador de perros y gatos, no nos interesa que reconozca solamente las fotografías exactas utilizadas durante el entrenamiento.

Queremos que pueda clasificar:

* nuevas razas;
* diferentes fondos;
* distintas posiciones;
* condiciones de iluminación variables;
* imágenes capturadas desde otros dispositivos.

---

# 4. Memorización versus aprendizaje

Podemos comparar dos situaciones.

### Modelo A

Reconoce perfectamente las imágenes utilizadas durante el entrenamiento.

Pero falla con fotografías nuevas.

### Modelo B

Comete algunos errores durante el entrenamiento, pero mantiene un rendimiento similar frente a imágenes nuevas.

El Modelo B puede ser más útil.

Conceptualmente:

**Memorización**

Entrenamiento muy alto
↓
Datos nuevos considerablemente peores

**Generalización**

Entrenamiento alto
↓
Datos nuevos con desempeño similar

La diferencia entre ambos escenarios es central para evaluar la calidad real de un modelo.

---

# 5. ¿Qué es overfitting?

El **overfitting** o sobreajuste ocurre cuando un modelo se ajusta excesivamente a los datos de entrenamiento.

En lugar de aprender únicamente patrones generales, comienza a aprender:

* detalles irrelevantes;
* ruido;
* particularidades del dataset;
* excepciones específicas;
* correlaciones accidentales.

Como resultado:

**Excelente desempeño en entrenamiento**

pero:

**Peor desempeño en datos nuevos**

Podemos resumir:

**Modelo demasiado ajustado al training set → baja capacidad de generalización**

---

# 6. Ejemplo de overfitting

Supongamos que queremos clasificar imágenes de gatos y perros.

En nuestro dataset ocurre accidentalmente:

* casi todas las fotografías de gatos tienen fondo claro;
* casi todas las fotografías de perros tienen fondo oscuro.

El modelo podría aprender:

**fondo claro → gato**

**fondo oscuro → perro**

En los datos de entrenamiento podría obtener una excelente precisión.

Pero si posteriormente recibe:

**gato sobre fondo oscuro**

puede equivocarse.

El modelo aprendió una regularidad presente en el dataset, pero esa regularidad no representa realmente el problema que queríamos resolver.

Esto demuestra que el *overfitting* no siempre significa literalmente memorizar imágenes. También puede significar aprender **patrones accidentales o demasiado específicos**.

---

# 7. Train, Validation y Test

Durante la Semana 6 dividimos el dataset en tres conjuntos.

Ahora podemos comprender con mayor precisión por qué cada uno es necesario.

### Training set

Se utiliza para que el modelo aprenda.

Aquí se actualizan:

* pesos;
* sesgos;
* demás parámetros.

### Validation set

Permite observar cómo se comporta el modelo durante su desarrollo frente a datos que no utiliza directamente para actualizar parámetros.

Sirve para:

* detectar sobreajuste;
* comparar configuraciones;
* seleccionar hiperparámetros;
* decidir cuándo detener el entrenamiento.

### Test set

Se reserva para realizar una evaluación final.

No debería utilizarse continuamente para ajustar decisiones.

Por tanto:

**Train → aprender**

**Validation → controlar y seleccionar**

**Test → evaluar finalmente**

---

# 8. ¿Por qué no basta con training accuracy?

Supongamos:

| Métrica                | Resultado |
| ---------------------- | --------: |
| Accuracy entrenamiento |       99% |
| Accuracy validación    |       71% |

Si observamos solamente el entrenamiento podríamos concluir:

**“El modelo funciona muy bien.”**

Sin embargo, la validación muestra que existe una diferencia considerable.

Esta diferencia es una señal importante.

En cambio:

| Métrica                | Resultado |
| ---------------------- | --------: |
| Accuracy entrenamiento |       91% |
| Accuracy validación    |       89% |

Aquí el modelo parece comportarse de manera mucho más consistente.

Por esta razón:

**nunca debemos evaluar un modelo únicamente por su desempeño en entrenamiento.**

---

# 9. Curvas de entrenamiento

Durante el entrenamiento podemos registrar métricas después de cada época.

Por ejemplo:

| Época | Accuracy Train | Accuracy Validation |
| ----: | -------------: | ------------------: |
|     1 |            62% |                 60% |
|     2 |            72% |                 69% |
|     3 |            81% |                 76% |
|     4 |            88% |                 79% |
|     5 |            93% |                 78% |
|     6 |            97% |                 75% |

Podemos observar que:

**training accuracy continúa aumentando**

mientras:

**validation accuracy comienza a disminuir**

Este comportamiento puede ser una señal de *overfitting*.

---

# 10. Curvas de pérdida

También podemos observar la función de pérdida.

Un entrenamiento saludable podría mostrar:

**Training loss ↓**

**Validation loss ↓**

Pero en presencia de sobreajuste puede ocurrir:

**Training loss ↓ continuamente**

mientras:

**Validation loss comienza a ↑**

Conceptualmente:

**Épocas iniciales → ambos conjuntos mejoran**

↓

**Punto óptimo**

↓

**Entrenamiento continúa mejorando**

pero:

**Validación empeora**

Este momento resulta importante para decidir cuándo detener el entrenamiento.

---

# 11. Visualización conceptual

Podemos imaginar tres etapas:

### Etapa 1 — Subentrenamiento

El modelo todavía no aprende suficientemente.

**Train bajo**

**Validation bajo**

### Etapa 2 — Buen aprendizaje

El modelo captura patrones útiles.

**Train alto**

**Validation alto**

### Etapa 3 — Sobreajuste

El modelo comienza a especializarse excesivamente en los datos de entrenamiento.

**Train sigue mejorando**

**Validation empeora**

El objetivo es encontrar un equilibrio adecuado.

---

# 12. Underfitting

El problema contrario al *overfitting* es el **underfitting** o subajuste.

Ocurre cuando el modelo no logra representar adecuadamente ni siquiera los patrones presentes en los datos de entrenamiento.

Por ejemplo:

| Métrica                | Resultado |
| ---------------------- | --------: |
| Accuracy entrenamiento |       58% |
| Accuracy validación    |       56% |

Aquí el problema no es que el modelo haya memorizado.

El modelo simplemente **no está aprendiendo suficientemente bien**.

Puede deberse a:

* modelo demasiado simple;
* entrenamiento insuficiente;
* características inadecuadas;
* configuración incorrecta;
* datos de baja calidad.

---

# 13. Comparación: underfitting, ajuste adecuado y overfitting

Podemos resumir:

| Situación        | Entrenamiento | Validación              | Interpretación                                 |
| ---------------- | ------------- | ----------------------- | ---------------------------------------------- |
| **Underfitting** | Bajo          | Bajo                    | El modelo no aprende suficientemente           |
| **Buen ajuste**  | Alto          | Alto y similar          | El modelo generaliza adecuadamente             |
| **Overfitting**  | Muy alto      | Considerablemente menor | El modelo se ajusta demasiado al entrenamiento |

Esta comparación debe analizarse siempre en contexto.

No existe un valor universal que defina automáticamente cuándo un modelo está sobreajustado.

---

# 14. Complejidad del modelo

La complejidad del modelo influye en su capacidad de aprender.

Un modelo muy simple puede no ser capaz de representar adecuadamente el problema.

Un modelo extremadamente complejo puede tener suficiente capacidad para aprender incluso detalles irrelevantes.

Conceptualmente:

**Modelo demasiado simple → underfitting**

**Complejidad adecuada → generalización**

**Modelo demasiado complejo → mayor riesgo de overfitting**

Esto no significa que los modelos complejos sean necesariamente malos.

Significa que requieren:

* datos suficientes;
* estrategias adecuadas de entrenamiento;
* control de generalización.

---

# 15. Cantidad de datos y overfitting

Cuando disponemos de pocos datos, un modelo complejo puede ajustarse fácilmente a los ejemplos disponibles.

Supongamos:

**CNN con millones de parámetros**

pero solamente:

**100 imágenes de entrenamiento**

Existe un riesgo importante de que el modelo aprenda particularidades de esas imágenes.

En general:

**más datos variados pueden ayudar a reducir el riesgo de sobreajuste**

porque obligan al modelo a encontrar patrones que aparezcan de manera consistente en múltiples ejemplos.

Sin embargo, nuevamente:

**cantidad sin calidad ni diversidad no es suficiente.**

---

# 16. Diversidad y generalización

La diversidad del dataset es especialmente importante en clasificación de imágenes.

Supongamos que queremos reconocer manzanas.

Un dataset más útil debería incorporar variaciones de:

* tamaño;
* color;
* orientación;
* iluminación;
* fondo;
* cámara;
* distancia;
* posición.

Si todas las imágenes son prácticamente iguales, el modelo puede especializarse demasiado.

Por tanto:

**diversidad de ejemplos → mayor oportunidad de aprender patrones generales**

Esta relación será importante durante la Semana 8 cuando trabajemos *data augmentation*.

---

# 17. Sesgo y varianza

Dos conceptos utilizados para comprender el comportamiento de los modelos son **sesgo** y **varianza**.

### Sesgo alto

El modelo realiza supuestos demasiado simples y no logra representar adecuadamente el problema.

Puede asociarse conceptualmente con:

**underfitting**

### Varianza alta

El modelo cambia demasiado dependiendo de los datos específicos utilizados para entrenarlo.

Puede asociarse conceptualmente con:

**overfitting**

Podemos representar:

**Alto sesgo → modelo demasiado rígido**

**Alta varianza → modelo demasiado sensible a los datos de entrenamiento**

El objetivo consiste en encontrar un equilibrio razonable.

---

# 18. Bias-variance tradeoff

Existe una tensión conocida como **bias-variance tradeoff**.

A medida que aumentamos la capacidad del modelo:

* puede disminuir el sesgo;
* pero puede aumentar la varianza.

Conceptualmente:

**Modelo simple**

→ alto sesgo
→ baja varianza

**Modelo extremadamente complejo**

→ bajo sesgo
→ alta varianza

Buscamos una configuración donde el modelo posea suficiente capacidad para aprender el problema sin especializarse excesivamente en el dataset particular.

---

# 19. Causas frecuentes de overfitting

El sobreajuste puede aparecer por diferentes razones.

### Dataset pequeño

No existe suficiente variedad para aprender patrones generales.

### Modelo demasiado complejo

Posee una gran cantidad de parámetros en relación con los datos disponibles.

### Exceso de entrenamiento

El modelo continúa ajustándose después de alcanzar un buen desempeño de validación.

### Datos poco diversos

Los ejemplos representan condiciones demasiado similares.

### Ruido y errores

El modelo puede terminar intentando aprender información irrelevante.

### Fuga de datos

Puede producir resultados artificialmente altos y una falsa impresión de generalización.

---

# 20. Estrategias generales para reducir overfitting

Existen diferentes técnicas.

Durante esta etapa interesa comprenderlas conceptualmente.

### Obtener más datos

Aumentar la cantidad de ejemplos representativos.

### Mejorar la diversidad

Incorporar variabilidad real del problema.

### Reducir la complejidad

Utilizar una arquitectura menos compleja cuando corresponda.

### Regularización

Penalizar configuraciones excesivamente complejas.

### Dropout

Desactivar temporalmente determinadas neuronas durante el entrenamiento.

### Early stopping

Detener el entrenamiento cuando la validación deja de mejorar.

### Data augmentation

Generar variaciones de las imágenes existentes.

Algunas de estas estrategias serán desarrolladas más adelante.

---

# 21. Early stopping

El **early stopping** consiste en detener el entrenamiento cuando el desempeño sobre validación deja de mejorar.

Por ejemplo:

| Época | Val Accuracy |
| ----: | -----------: |
|     1 |          63% |
|     2 |          71% |
|     3 |          78% |
|     4 |          82% |
|     5 |          84% |
|     6 |          83% |
|     7 |          81% |

El mejor resultado ocurrió en:

**época 5**

Continuar entrenando no mejoró la capacidad de generalización.

Por tanto, podríamos conservar el modelo correspondiente al mejor desempeño de validación.

---

# 22. Dropout

**Dropout** es una técnica utilizada en redes neuronales para reducir dependencia excesiva entre neuronas.

Durante el entrenamiento, algunas unidades se desactivan temporalmente de forma aleatoria.

Conceptualmente:

**Red completa**

↓

**durante un paso de entrenamiento se desactivan algunas conexiones**

↓

**la red debe aprender representaciones menos dependientes de unidades específicas**

Esto puede ayudar a reducir el sobreajuste.

Por ejemplo:

```python
layers.Dropout(0.5)
```

indica conceptualmente que durante entrenamiento se desactivará aleatoriamente una proporción de unidades.

---

# 23. Regularización

La **regularización** busca limitar configuraciones excesivamente complejas del modelo.

Una estrategia habitual consiste en agregar una penalización relacionada con el tamaño de los pesos.

Conceptualmente:

**Pérdida total = Error de predicción + Penalización por complejidad**

Esto incentiva al modelo a buscar soluciones más simples cuando varias configuraciones producen resultados similares.

Existen técnicas como:

* L1;
* L2.

En esta etapa interesa comprender su propósito general:

**reducir la tendencia del modelo a ajustarse excesivamente a los datos de entrenamiento.**

---

# 24. Data augmentation como estrategia

En imágenes podemos generar variaciones controladas de los ejemplos disponibles.

Por ejemplo:

**Imagen original**

↓

* pequeña rotación;
* desplazamiento;
* cambio de escala;
* volteo horizontal;
* modificación moderada de contraste.

Estas transformaciones pueden producir nuevas versiones válidas de la misma clase.

Esto permite incrementar la diversidad efectiva del entrenamiento.

Por ejemplo:

**fotografía de gato**

↓

**rotación de 10°**

↓

sigue siendo:

**gato**

Esta técnica será desarrollada con mayor profundidad en la Semana 8.

---

# 25. Overfitting y el proyecto integrador

El proyecto integrador de clasificación de imágenes será especialmente sensible al sobreajuste.

Cada dupla deberá considerar:

* número de imágenes;
* cantidad de clases;
* distribución por clase;
* diversidad visual;
* arquitectura del modelo;
* número de épocas;
* comportamiento de train y validation.

No será suficiente reportar:

**“accuracy = 95%”**

Será necesario responder:

**¿95% sobre qué conjunto?**

Un resultado de:

**95% training**

puede tener un significado muy diferente de:

**95% test**

Por tanto, toda métrica debe estar acompañada por el contexto en el cual fue calculada.

---

# 26. Ejemplo conductor: clasificación de residuos

Supongamos que entrenamos el clasificador de residuos.

Después de 20 épocas obtenemos:

```text
Train accuracy: 98%
Validation accuracy: 72%
```

Existe una diferencia de:

**26 puntos porcentuales**

Esto sugiere que debemos investigar sobreajuste.

Posibles causas:

* pocas imágenes;
* demasiada similitud entre ejemplos;
* arquitectura demasiado compleja;
* exceso de épocas;
* falta de regularización.

Podríamos considerar:

* incorporar más imágenes;
* aplicar data augmentation;
* agregar dropout;
* reducir complejidad;
* utilizar early stopping.

Después de realizar cambios obtenemos:

```text
Train accuracy: 91%
Validation accuracy: 88%
```

Aunque el entrenamiento disminuyó, el segundo modelo podría ser **más útil** porque presenta mayor capacidad de generalización.

---

# 27. Interpretar gráficos de entrenamiento

En TensorFlow/Keras, el entrenamiento puede almacenar un historial.

Por ejemplo:

```python
historial = modelo.fit(
    train_ds,
    validation_data=validation_ds,
    epochs=20
)
```

Podemos obtener:

```python
historial.history["accuracy"]
historial.history["val_accuracy"]
```

y:

```python
historial.history["loss"]
historial.history["val_loss"]
```

Estas series permiten observar cómo evoluciona el modelo durante las épocas.

---

# 28. Visualización con Python

Podemos visualizar accuracy:

```python
import matplotlib.pyplot as plt

plt.plot(historial.history["accuracy"], label="train")
plt.plot(historial.history["val_accuracy"], label="validation")

plt.xlabel("Época")
plt.ylabel("Accuracy")
plt.legend()
plt.show()
```

Y pérdida:

```python
plt.plot(historial.history["loss"], label="train")
plt.plot(historial.history["val_loss"], label="validation")

plt.xlabel("Época")
plt.ylabel("Loss")
plt.legend()
plt.show()
```

Lo importante no es solamente generar el gráfico.

El estudiante debe ser capaz de **interpretarlo**.

---

# 29. ¿Qué buscamos en las curvas?

Un comportamiento deseable sería:

**Train mejora**

y:

**Validation también mejora**

manteniendo una diferencia razonable.

Una señal de alerta sería:

**Train continúa mejorando**

mientras:

**Validation comienza a empeorar**

Esto podría indicar que el modelo está comenzando a aprender detalles específicos de los datos de entrenamiento.

Las curvas permiten visualizar algo que una única métrica final podría ocultar.

---

# 30. El conjunto de prueba debe permanecer protegido

Existe una práctica incorrecta frecuente:

**evaluar muchas veces sobre test y modificar el modelo según esos resultados.**

Si hacemos esto, indirectamente estamos utilizando el conjunto de prueba para tomar decisiones.

Con el tiempo podemos terminar ajustándonos también a ese conjunto.

Por ello:

**Train → aprendizaje**

**Validation → decisiones durante el desarrollo**

**Test → evaluación final**

Esta separación protege la credibilidad de la evaluación final.

---

# 31. Data leakage y resultados engañosos

La fuga de datos puede crear una apariencia falsa de generalización.

Ejemplo:

Tenemos:

**imagen_original.jpg**

en entrenamiento.

Y:

**imagen_original_copia.jpg**

en prueba.

El modelo podría reconocer patrones casi idénticos y obtener una puntuación muy alta.

Pero ese resultado no representa correctamente su capacidad frente a imágenes realmente nuevas.

Por eso la preparación realizada en la Semana 6 influye directamente en la validez de la evaluación.

---

# 32. Generalización y contexto real

Incluso un buen resultado de test no garantiza que el modelo funcione en cualquier contexto.

Supongamos que:

**Train, validation y test**

provienen todos del mismo repositorio de fotografías tomadas en condiciones similares.

Pero en producción:

* cambia la cámara;
* cambia la iluminación;
* aparecen nuevos fondos;
* cambia la distancia al objeto.

El modelo podría disminuir su desempeño.

Por tanto, generalización también implica preguntarse:

**¿Los datos de evaluación representan razonablemente las condiciones reales de uso?**

---

# 33. Generalización no significa perfección

Un modelo que generaliza bien no necesariamente obtiene:

**100% de accuracy**

La generalización significa que el desempeño observado sobre datos conocidos se mantiene razonablemente cuando el modelo enfrenta información nueva.

Por ejemplo:

```text
Train: 92%
Validation: 89%
Test: 88%
```

puede representar un comportamiento más confiable que:

```text
Train: 100%
Validation: 65%
Test: 62%
```

La coherencia entre conjuntos resulta tan importante como el valor absoluto.

---

# 34. Seleccionar el mejor modelo

Durante el entrenamiento podemos obtener varias versiones del modelo.

Por ejemplo:

| Modelo | Train | Validation |
| ------ | ----: | ---------: |
| A      |   85% |        83% |
| B      |   94% |        88% |
| C      |   99% |        76% |

Si observamos solamente training:

**C parece mejor.**

Pero considerando generalización:

**B podría ser la alternativa más adecuada.**

Por tanto, seleccionar un modelo requiere analizar más de una métrica.

---

# 35. Caso conductor: dos modelos posibles

Consideremos dos modelos para clasificación de frutas.

### Modelo A

```text
Train accuracy: 99%
Validation accuracy: 70%
```

### Modelo B

```text
Train accuracy: 90%
Validation accuracy: 88%
```

Preguntas:

**¿Cuál presenta mayor evidencia de overfitting?**

Modelo A.

**¿Cuál parece generalizar mejor?**

Modelo B.

**¿Significa esto que 90% siempre es mejor que 99%?**

No.

La interpretación depende del conjunto sobre el que fue calculada la métrica.

---

# 36. Preguntas para discusión en clase

### Caso 1

Un modelo obtiene 100% en entrenamiento y 62% en validación.

**Pregunta:** ¿Qué fenómeno podría estar ocurriendo?

### Caso 2

Un modelo obtiene 55% tanto en entrenamiento como en validación.

**Pregunta:** ¿El problema principal parece overfitting o underfitting?

### Caso 3

Una dupla utiliza las imágenes de prueba para decidir cuántas épocas debe entrenar.

**Pregunta:** ¿Por qué esta estrategia es incorrecta?

### Caso 4

Un dataset contiene muchas imágenes, pero casi todas son fotografías tomadas desde el mismo ángulo y con el mismo fondo.

**Pregunta:** ¿Por qué la cantidad total puede no ser suficiente para garantizar generalización?

### Caso 5

Después de la época 8, la pérdida de entrenamiento continúa disminuyendo pero la pérdida de validación comienza a aumentar.

**Pregunta:** ¿Qué podría indicar este comportamiento?

### Caso 6

Un clasificador obtiene menor accuracy de entrenamiento después de aplicar dropout, pero mejora considerablemente en validación.

**Pregunta:** ¿Debe considerarse necesariamente un empeoramiento?

---

# 37. Síntesis de la Semana 7

Al finalizar esta sesión deben quedar instaladas ocho ideas fundamentales:

1. **Generalizar significa funcionar adecuadamente con datos nuevos, no solamente con los utilizados durante el entrenamiento.**
2. **Overfitting ocurre cuando un modelo se ajusta demasiado a las particularidades del conjunto de entrenamiento.**
3. **Underfitting ocurre cuando el modelo no logra aprender suficientemente ni siquiera los patrones presentes en entrenamiento.**
4. **Train, validation y test cumplen funciones diferentes y deben mantenerse claramente separados.**
5. **Una diferencia creciente entre entrenamiento y validación puede constituir una señal de sobreajuste.**
6. **Cantidad, diversidad de datos y complejidad del modelo influyen en la capacidad de generalización.**
7. **Early stopping, regularización, dropout y data augmentation son estrategias que pueden contribuir a reducir el overfitting.**
8. **La evaluación de un modelo debe considerar no solamente cuánto acierta, sino sobre qué datos se calculó ese desempeño.**

### Hacia la Semana 8

Durante la Unidad 2 hemos avanzado desde la construcción del dataset hacia la evaluación de su capacidad para producir modelos generalizables:

**Semana 5:** ¿Qué datos necesitamos?

**Semana 6:** ¿Cómo los limpiamos, etiquetamos, transformamos y dividimos?

**Semana 7:** ¿Cómo evitamos que el modelo simplemente memorice esos datos?

La **Semana 8** cerrará la unidad abordando:

**¿Cómo validamos de manera más robusta nuestros datos y modelos y cómo podemos aumentar la diversidad del conjunto de entrenamiento?**

Para ello estudiaremos **validación cruzada, evaluación del conjunto de datos y data augmentation**, cerrando además la preparación del **dataset definitivo del proyecto integrador**.

