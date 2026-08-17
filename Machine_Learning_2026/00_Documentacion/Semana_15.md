# Semana 14 — Optimización de modelos: cuantificación y poda de redes

## 1. Propósito de la sesión

Comprender por qué un modelo de Deep Learning debe ser **optimizado antes de su implementación**, analizando el impacto que tienen su tamaño, cantidad de parámetros, consumo de memoria y velocidad de inferencia sobre una solución real.

Al finalizar la sesión, el estudiante debería ser capaz de:

* Explicar por qué un modelo entrenado no necesariamente está listo para producción.
* Diferenciar entrenamiento e inferencia desde la perspectiva de recursos computacionales.
* Identificar métricas relevantes para evaluar la eficiencia de un modelo.
* Comprender la relación entre cantidad de parámetros, tamaño y velocidad.
* Explicar conceptualmente qué es la cuantificación.
* Diferenciar representaciones FP32, FP16 e INT8.
* Comprender las ventajas y posibles efectos de cuantificar un modelo.
* Explicar qué es la poda o *pruning*.
* Diferenciar poda de pesos y poda estructurada.
* Analizar el compromiso entre precisión y eficiencia.
* Diseñar una estrategia inicial de optimización para el modelo del proyecto integrador.

---

# 2. Del modelo entrenado al modelo implementable

Al finalizar la Unidad 3 disponemos conceptualmente de:

**Dataset preparado**

↓

**CNN entrenada**

↓

**Modelo evaluado**

↓

**Modelo definitivo**

Sin embargo, existe una diferencia importante entre:

**un modelo que funciona**

y:

**un modelo que puede implementarse eficientemente.**

Durante el entrenamiento podemos utilizar:

* GPU;
* gran cantidad de RAM;
* equipos potentes;
* procesamiento durante varios minutos u horas.

Pero el modelo final podría tener que funcionar en:

* un servidor;
* un computador convencional;
* un teléfono móvil;
* un dispositivo Edge;
* un servicio cloud con costos asociados.

Por tanto, antes de implementar debemos preguntarnos:

**¿Qué tan eficiente es nuestro modelo?**

---

# 3. Entrenamiento versus producción

Durante el entrenamiento nuestra prioridad suele ser:

**aprender correctamente**

Por ello utilizamos:

* múltiples épocas;
* backpropagation;
* cálculo de gradientes;
* optimización;
* almacenamiento de estados.

En producción, normalmente necesitamos:

**realizar inferencia**

Es decir:

**Nueva imagen → Modelo → Predicción**

Ya no necesitamos:

* calcular gradientes;
* ejecutar backpropagation;
* modificar parámetros.

Esto cambia las prioridades.

En producción nos interesan especialmente:

* tiempo de respuesta;
* uso de memoria;
* tamaño del modelo;
* consumo de recursos;
* costo de ejecución.

---

# 4. ¿Qué significa optimizar un modelo?

**Optimizar un modelo para implementación** significa modificar o transformar su representación buscando mejorar su eficiencia sin degradar excesivamente su capacidad predictiva.

Conceptualmente:

**Modelo original**

↓

**Optimización**

↓

**Modelo más eficiente**

Idealmente buscamos:

**menos tamaño**

*

**menor uso de memoria**

*

**menor tiempo de inferencia**

manteniendo:

**un nivel aceptable de precisión**

La palabra clave es:

**compromiso o trade-off.**

---

# 5. Accuracy no es suficiente

Durante UA3 evaluamos principalmente:

* accuracy;
* pérdida;
* matriz de confusión;
* generalización.

Ahora debemos incorporar nuevas preguntas:

**¿Cuánto pesa el modelo?**

**¿Cuánto tarda en clasificar una imagen?**

**¿Cuánta memoria utiliza?**

**¿En qué hardware puede ejecutarse?**

Por ejemplo:

### Modelo A

```text
Accuracy: 94%
Tamaño: 480 MB
Inferencia: 1,8 segundos
```

### Modelo B

```text
Accuracy: 92%
Tamaño: 35 MB
Inferencia: 0,15 segundos
```

Si necesitamos una aplicación rápida y con recursos limitados, el Modelo B podría ser más conveniente.

---

# 6. Métricas de eficiencia

Al analizar un modelo para producción podemos considerar diferentes indicadores.

### Tamaño del modelo

Por ejemplo:

**150 MB**

### Latencia

Tiempo necesario para procesar una entrada.

Por ejemplo:

**120 ms por imagen**

### Throughput

Cantidad de entradas procesadas por unidad de tiempo.

Por ejemplo:

**50 imágenes por segundo**

### Consumo de memoria

Cantidad de RAM o memoria especializada requerida.

### Uso de CPU/GPU

Recursos necesarios durante la inferencia.

Estas métricas complementan las métricas predictivas.

---

# 7. Latencia

La **latencia** corresponde al tiempo transcurrido desde que entregamos una entrada hasta que obtenemos una salida.

Conceptualmente:

**Imagen**

↓

**Modelo**

↓

**Predicción**

Tiempo:

**0,18 segundos**

En determinadas aplicaciones, una pequeña diferencia puede ser irrelevante.

En otras:

* aplicaciones interactivas;
* cámaras en tiempo real;
* robots;
* sistemas de seguridad;

la velocidad puede resultar crítica.

---

# 8. Throughput

El **throughput** indica cuántas inferencias puede ejecutar el sistema durante un determinado período.

Por ejemplo:

**100 imágenes / segundo**

No es exactamente lo mismo que latencia.

Un sistema puede procesar grandes lotes eficientemente pero mantener cierta latencia individual.

Por tanto:

**latencia → tiempo por respuesta**

**throughput → volumen procesado**

Ambas métricas pueden importar dependiendo del caso.

---

# 9. Tamaño del modelo

El tamaño depende principalmente de:

* cantidad de parámetros;
* representación numérica utilizada;
* estructura almacenada.

Supongamos:

**10 millones de parámetros**

Si cada parámetro utiliza:

**32 bits = 4 bytes**

entonces:

**10.000.000 × 4 bytes ≈ 40 MB**

solo para almacenar los pesos.

Reducir la precisión numérica puede disminuir significativamente este tamaño.

Aquí aparece la primera técnica central de esta sesión:

**cuantificación.**

---

# 10. Precisión numérica

Los pesos de una red neuronal normalmente se representan mediante números.

Durante entrenamiento es habitual utilizar:

**FP32**

es decir:

**floating point de 32 bits.**

Esto permite representar números decimales con bastante precisión.

Por ejemplo:

```text
0.8237641
-0.1420387
2.0924176
```

Sin embargo, almacenar y operar con esta precisión tiene un costo.

La pregunta es:

**¿Necesitamos realmente 32 bits para cada peso durante inferencia?**

---

# 11. ¿Qué es cuantificación?

La **cuantificación** consiste en representar los valores del modelo utilizando una precisión numérica menor.

Por ejemplo:

**FP32**

↓

**FP16**

o:

**INT8**

Esto permite reducir:

* tamaño del modelo;
* memoria utilizada;
* cantidad de datos que deben transferirse;
* costo de determinadas operaciones.

Conceptualmente:

**Mayor precisión numérica**

↓

**Representación más compacta**

sin intentar modificar radicalmente:

**el comportamiento aprendido por la red.**

---

# 12. FP32

**FP32** utiliza números de punto flotante de 32 bits.

Es habitual durante entrenamiento.

Ventajas:

* amplio rango;
* buena precisión;
* soporte generalizado.

Desventajas:

* mayor almacenamiento;
* mayor transferencia de memoria;
* puede ser innecesariamente costoso para inferencia.

Por ejemplo:

**1 parámetro FP32 = 4 bytes**

Por tanto:

**25 millones de parámetros ≈ 100 MB**

---

# 13. FP16

**FP16** utiliza 16 bits.

Conceptualmente:

**la mitad de bits que FP32**

Esto puede reducir aproximadamente el espacio requerido por los pesos.

Ejemplo:

**25 millones de parámetros**

FP32:

**≈ 100 MB**

FP16:

**≈ 50 MB**

Dependiendo del hardware, FP16 puede además acelerar operaciones.

---

# 14. INT8

Otra alternativa es representar determinados valores mediante enteros de 8 bits.

**INT8**

utiliza:

**8 bits = 1 byte**

Conceptualmente:

**FP32 → 4 bytes**

**INT8 → 1 byte**

Esto puede reducir considerablemente el tamaño.

Un modelo de:

**100 MB en FP32**

podría acercarse conceptualmente a:

**25 MB**

si sus pesos pudieran representarse completamente mediante INT8.

En la práctica, el tamaño final depende de la arquitectura y el formato utilizado.

---

# 15. ¿Cómo representar decimales con INT8?

Los pesos originales pueden contener valores como:

```text
-0.82
0.17
1.24
```

INT8 representa números enteros dentro de un rango limitado.

Por ello, la cuantificación utiliza factores que permiten mapear:

**valores continuos**

hacia:

**valores discretos**

Conceptualmente:

**FP32**

↓

**Escalamiento**

↓

**INT8**

Durante inferencia, estas representaciones permiten aproximar las operaciones originales.

---

# 16. Cuantificar implica aproximar

La cuantificación reduce precisión numérica.

Por tanto:

**valor original**

podría no representarse exactamente.

Ejemplo conceptual:

```text
FP32: 0,734821
```

podría transformarse en una representación equivalente aproximada:

```text
INT8: 94
```

junto con parámetros de escala.

Esta aproximación puede producir pequeñas diferencias en las predicciones.

Por ello, después de cuantificar debemos:

**volver a evaluar el modelo.**

---

# 17. Post-Training Quantization

Una estrategia es:

**Post-Training Quantization**

o cuantificación posterior al entrenamiento.

El proceso es:

**Modelo FP32 ya entrenado**

↓

**Conversión**

↓

**Modelo cuantificado**

↓

**Evaluación**

La ventaja es que no necesitamos volver a realizar todo el entrenamiento original.

Es una estrategia atractiva cuando ya disponemos de un modelo definitivo.

---

# 18. Quantization Aware Training

Otra alternativa es:

**Quantization Aware Training (QAT)**

En este caso, durante el entrenamiento se simulan los efectos que tendrá posteriormente la cuantificación.

Conceptualmente:

**Entrenamiento**

↓

**Simular precisión reducida**

↓

**Modelo aprende a tolerar esa representación**

↓

**Cuantificación final**

Esto puede ayudar a conservar mejor la precisión.

Sin embargo, requiere un proceso de entrenamiento adicional y aumenta la complejidad.

---

# 19. Comparación conceptual

| Estrategia                  | Momento                 | Complejidad | Ventaja                         |
| --------------------------- | ----------------------- | ----------- | ------------------------------- |
| Post-Training Quantization  | Después de entrenar     | Menor       | Conversión rápida               |
| Quantization Aware Training | Durante/reentrenamiento | Mayor       | Puede conservar mejor precisión |

Para nuestro proyecto, la primera alternativa puede resultar suficiente como experiencia de optimización.

---

# 20. Cuantificación y hardware

La mejora no depende únicamente del modelo.

También depende de si el hardware puede ejecutar eficientemente la precisión utilizada.

Por ejemplo:

* determinadas CPU optimizan INT8;
* determinadas GPU aceleran FP16;
* algunos dispositivos Edge poseen aceleradores especializados.

Por tanto:

**modelo cuantificado + hardware compatible**

puede producir una mejora significativa.

Sin soporte adecuado, la reducción de tamaño no siempre implica una mejora proporcional de velocidad.

---

# 21. Compromiso de la cuantificación

Supongamos:

### Original

```text
Accuracy: 91,5%
Tamaño: 120 MB
Latencia: 350 ms
```

### Cuantificado

```text
Accuracy: 90,9%
Tamaño: 32 MB
Latencia: 130 ms
```

Perdimos:

**0,6 puntos porcentuales**

pero obtuvimos:

* reducción considerable del tamaño;
* inferencia mucho más rápida.

Dependiendo del proyecto, este compromiso podría ser perfectamente aceptable.

---

# 22. ¿Cuánta precisión podemos perder?

No existe un valor universal.

La respuesta depende del contexto.

Por ejemplo:

### Clasificador académico de productos

Una caída de:

**0,5%**

podría resultar aceptable.

### Aplicación crítica

Una caída pequeña podría ser significativa.

Por ello, la decisión debe responder a:

**requisitos de la solución**

y no únicamente a:

**conseguir el modelo más pequeño posible.**

---

# 23. Cuantificación con TensorFlow Lite

Una forma conceptual de cuantificar un modelo Keras consiste en convertirlo mediante TensorFlow Lite.

Por ejemplo:

```python
import tensorflow as tf

converter = tf.lite.TFLiteConverter.from_keras_model(modelo)

converter.optimizations = [
    tf.lite.Optimize.DEFAULT
]

modelo_tflite = converter.convert()
```

Después podemos guardar:

```python
with open("modelo.tflite", "wb") as f:
    f.write(modelo_tflite)
```

El objetivo de este código es mostrar el flujo:

**Modelo Keras**

↓

**Conversión**

↓

**Modelo optimizado**

---

# 24. Evaluar después de convertir

Una mala práctica sería:

**convertir → asumir que funciona igual**

La metodología correcta debería ser:

**Modelo original**

↓

**Medir accuracy y latencia**

↓

**Cuantificar**

↓

**Medir nuevamente accuracy y latencia**

↓

**Comparar**

Esto permite justificar si la optimización fue conveniente.

---

# 25. Segunda técnica: poda de redes

Otra estrategia de optimización es:

**Pruning**

o:

**poda de redes**

La idea consiste en identificar parámetros que aportan poco al comportamiento del modelo y reducir su influencia o eliminarlos.

Conceptualmente:

**Red completa**

↓

**Identificar conexiones poco relevantes**

↓

**Eliminar/reducir conexiones**

↓

**Red más dispersa**

---

# 26. ¿Por qué existen pesos poco relevantes?

Durante el entrenamiento una red puede aprender millones de parámetros.

Algunos pueden poseer valores muy pequeños.

Por ejemplo:

```text
0.00003
-0.00001
0.00007
```

Su influencia puede resultar muy reducida.

La poda puede establecerlos en:

**0**

Conceptualmente:

**peso de poca importancia → eliminar conexión efectiva**

Esto produce una red más **sparse** o dispersa.

---

# 27. Sparsity

La **sparsity** representa la proporción de pesos que se encuentran en cero.

Por ejemplo:

**Modelo con 1.000.000 de pesos**

Si:

**500.000 = 0**

entonces:

**sparsity = 50%**

Conceptualmente:

**mayor sparsity → más conexiones eliminadas**

Sin embargo, una alta sparsity no garantiza automáticamente:

**menor tiempo de inferencia**

porque el hardware y software deben poder aprovechar esa estructura dispersa.

---

# 28. Magnitude Pruning

Una estrategia frecuente consiste en eliminar pesos según su magnitud.

Conceptualmente:

**Peso grande → conservar**

**Peso muy pequeño → candidato a poda**

Por ejemplo:

```text
0,83   → conservar
-0,42  → conservar
0,0002 → podar
```

La idea es que los valores más pequeños podrían contribuir menos al resultado.

Este procedimiento puede aplicarse gradualmente.

---

# 29. Poda gradual

Eliminar una gran cantidad de pesos de manera repentina puede perjudicar fuertemente el modelo.

Una alternativa consiste en aumentar progresivamente la sparsity.

Por ejemplo:

```text
Inicio → 0%
↓
Entrenamiento
↓
20%
↓
40%
↓
50%
```

Esto permite que el modelo se adapte durante el proceso.

Posteriormente puede realizarse un reentrenamiento o ajuste.

---

# 30. Pruning y Fine-Tuning

Después de podar podemos realizar un nuevo entrenamiento corto.

Conceptualmente:

**Modelo entrenado**

↓

**Poda**

↓

**Accuracy disminuye**

↓

**Fine-Tuning**

↓

**Parte de la precisión se recupera**

Por tanto, poda y Fine-Tuning pueden complementarse.

El objetivo es obtener:

**menos parámetros efectivos**

manteniendo:

**desempeño razonable.**

---

# 31. Poda no estructurada

En la **poda no estructurada** eliminamos pesos individuales.

Por ejemplo:

```text
[0,7  0,0  0,4]
[0,0  0,8  0,0]
[0,2  0,0  0,5]
```

La matriz continúa teniendo las mismas dimensiones, pero contiene muchos ceros.

Ventaja:

* gran flexibilidad.

Desventaja:

* no todo hardware aprovecha eficientemente esa dispersión.

---

# 32. Poda estructurada

En la **poda estructurada** eliminamos elementos completos de la arquitectura.

Por ejemplo:

* filtros completos;
* canales;
* neuronas;
* bloques.

Conceptualmente:

**Conv2D con 64 filtros**

↓

**Eliminar filtros poco relevantes**

↓

**Conv2D efectiva con menos filtros**

Esto puede producir una arquitectura realmente más pequeña y fácil de acelerar.

---

# 33. Ejemplo de poda estructurada

Supongamos:

```text
Conv1 → 32 filtros
Conv2 → 64 filtros
Conv3 → 128 filtros
```

Después del análisis:

```text
Conv1 → 32
Conv2 → 48
Conv3 → 80
```

Se reduce:

* cantidad de parámetros;
* operaciones;
* memoria.

Sin embargo, decidir qué filtros eliminar puede ser más complejo que realizar pruning de pesos individuales.

---

# 34. Cuantificación versus poda

Ambas buscan eficiencia, pero actúan sobre aspectos diferentes.

### Cuantificación

Reduce:

**precisión de cada parámetro**

Ejemplo:

**FP32 → INT8**

### Poda

Reduce:

**cantidad de parámetros efectivos**

Ejemplo:

**peso pequeño → 0**

Podemos resumir:

**Quantization → menos bits**

**Pruning → menos conexiones útiles**

---

# 35. Pueden combinarse

Las técnicas no son necesariamente excluyentes.

Podemos realizar:

**Modelo original**

↓

**Pruning**

↓

**Fine-Tuning**

↓

**Quantization**

↓

**Modelo optimizado**

Conceptualmente obtenemos:

**menos parámetros efectivos**

y:

**representación numérica más compacta**

Sin embargo, cada transformación debe validarse.

---

# 36. Optimización acumulativa

Si aplicamos varias técnicas, debemos medir el efecto de cada etapa.

Por ejemplo:

| Modelo    | Accuracy | Tamaño | Latencia |
| --------- | -------: | -----: | -------: |
| Original  |    92,0% | 120 MB |   320 ms |
| Pruned    |    91,8% |  95 MB |   280 ms |
| Quantized |    91,1% |  28 MB |   110 ms |

Esta tabla permite responder:

**¿Qué ganamos?**

y:

**¿Qué sacrificamos?**

---

# 37. Medir antes de optimizar

Antes de aplicar cualquier técnica debemos establecer una:

**baseline**

o línea base.

Por ejemplo:

```text
Modelo original

Accuracy test: 91,8%
Tamaño: 84 MB
Latencia promedio: 210 ms
```

Después aplicamos la optimización.

Sin una baseline no podemos saber objetivamente si el cambio fue beneficioso.

---

# 38. Medir latencia correctamente

Una sola inferencia puede ser engañosa.

Por ejemplo:

```python
inicio = time.time()
modelo.predict(imagen)
fin = time.time()
```

La primera ejecución puede incluir:

* carga;
* inicialización;
* compilación interna.

Es mejor realizar múltiples inferencias.

Por ejemplo:

```text
100 ejecuciones
```

y calcular:

**latencia promedio**

También podemos considerar:

* mínimo;
* máximo;
* desviación.

---

# 39. Warm-up

Antes de medir, es frecuente ejecutar algunas inferencias iniciales.

Conceptualmente:

**Warm-up**

↓

**Sistema estabilizado**

↓

**Medición**

Esto reduce el efecto de tareas de inicialización.

Por ejemplo:

```python
for _ in range(10):
    modelo.predict(imagen)
```

Luego comenzamos a registrar tiempos.

---

# 40. Inferencia individual versus batch

También debemos definir qué estamos midiendo.

### Una imagen

Relevante para una aplicación interactiva.

### Batch de imágenes

Relevante para procesamiento masivo.

Por ejemplo:

**1 imagen → 100 ms**

pero:

**32 imágenes → 600 ms**

Esto significa que el throughput del batch puede ser mucho mayor.

Las métricas deben reflejar el contexto de uso.

---

# 41. Tamaño del archivo no es toda la historia

Reducir el archivo de:

**100 MB → 25 MB**

es útil.

Pero durante ejecución el modelo puede requerir más memoria que el tamaño del archivo debido a:

* activaciones;
* buffers;
* estructuras internas;
* datos de entrada.

Por tanto, debemos distinguir:

**tamaño almacenado**

de:

**memoria utilizada durante inferencia.**

---

# 42. Modelo grande versus modelo pequeño

Supongamos dos modelos:

### ResNet

* mayor capacidad;
* buen desempeño;
* más parámetros.

### MobileNet

* arquitectura eficiente;
* menos parámetros;
* pensada para dispositivos limitados.

La selección de arquitectura realizada en UA3 ya constituye una primera decisión de optimización.

Es más fácil implementar eficientemente un modelo razonablemente pequeño que intentar reducir radicalmente uno excesivamente grande.

---

# 43. Optimización comienza desde el diseño

No deberíamos pensar:

**Primero construyo cualquier modelo y después veo cómo reducirlo.**

Una mejor lógica es:

**Requisitos de implementación**

↓

**Seleccionar arquitectura apropiada**

↓

**Entrenar**

↓

**Optimizar**

Por ejemplo, si sabemos desde el comienzo que el modelo debe ejecutarse en un teléfono, puede ser razonable considerar:

**MobileNet**

en lugar de una arquitectura mucho más pesada.

---

# 44. Restricciones del entorno

Antes de optimizar debemos conocer dónde se ejecutará el modelo.

### Cloud

Puede existir:

* CPU potente;
* GPU;
* escalamiento;
* mayor memoria.

### Computador local

Recursos intermedios.

### Smartphone

Restricciones mayores.

### Edge device

Puede existir:

* memoria limitada;
* baja energía;
* CPU específica.

La misma optimización no será necesariamente apropiada para todos estos escenarios.

---

# 45. Precisión versus velocidad

Podemos imaginar una frontera de soluciones:

**Modelo A**

Muy preciso, muy lento.

**Modelo B**

Muy rápido, menos preciso.

**Modelo C**

Equilibrio entre ambos.

La decisión óptima depende de los requisitos.

Por ejemplo:

**Aplicación interactiva**

puede requerir:

**respuesta < 500 ms**

Si un modelo tarda:

**4 segundos**

aunque tenga excelente accuracy puede no resultar útil.

---

# 46. Optimización y experiencia de usuario

La velocidad no es únicamente un aspecto técnico.

También afecta la experiencia.

Flujo deseado:

**Usuario carga imagen**

↓

**Procesamiento rápido**

↓

**Resultado**

Si el modelo requiere:

**15 segundos**

cada vez, la solución puede parecer poco funcional.

Por tanto, la latencia forma parte de la calidad del producto final.

---

# 47. Optimización y costo cloud

En servicios cloud normalmente pagamos por recursos utilizados.

Un modelo que requiere:

**GPU potente**

para cada inferencia puede ser mucho más costoso que otro que funciona correctamente sobre CPU.

Por tanto:

**optimización**

también puede significar:

**reducción de costos operacionales.**

Un pequeño descenso de accuracy puede justificarse si produce una disminución significativa del costo sin comprometer el objetivo.

---

# 48. Optimización y energía

En Edge Computing y dispositivos móviles también importa el consumo energético.

Un modelo grande puede requerir:

* más operaciones;
* mayor tiempo de CPU/GPU;
* más energía.

Esto afecta:

* batería;
* temperatura;
* autonomía.

Por ello, optimizar un modelo también significa utilizar los recursos de manera eficiente.

---

# 49. ¿Qué optimizaremos en el proyecto?

El primer paso será establecer una baseline:

**Accuracy**

**Tamaño**

**Tiempo de inferencia**

Posteriormente deberá analizar alguna estrategia de optimización, por ejemplo:

**cuantificación**

y/o:

**poda**

según corresponda.

El descriptor exige que la implementación considere explícitamente velocidad y optimización de recursos. 

---

# 50. Propuesta de flujo para el proyecto

Una secuencia coherente sería:

**1. Cargar modelo**

↓

**2. Medir desempeño original**

↓

**3. Medir tamaño**

↓

**4. Medir latencia**

↓

**5. Aplicar optimización**

↓

**6. Medir nuevamente**

↓

**7. Comparar**

↓

**8. Decidir si la optimización es aceptable**

Este proceso transforma la optimización en una evaluación objetiva y no solamente en una operación técnica.

---

# 51. Ejemplo conductor: clasificador de residuos

Supongamos que el modelo final de UA3 tiene:

```text
Accuracy test: 92,1%
Tamaño: 86 MB
Latencia: 240 ms
```

Aplicamos cuantificación:

```text
Accuracy test: 91,5%
Tamaño: 24 MB
Latencia: 105 ms
```

Resultado:

### Cambio de accuracy

**-0,6 puntos**

### Reducción de tamaño

aproximadamente:

**72%**

### Mejora de latencia

aproximadamente:

**56%**

La pregunta no es solamente:

**“¿bajó el accuracy?”**

La pregunta correcta es:

**“¿el compromiso obtenido mejora la solución para su contexto de implementación?”**

---

# 52. Poda en el ejemplo conductor

Supongamos ahora:

**Modelo original**

```text
92,1%
86 MB
240 ms
```

Después de pruning:

```text
91,9%
70 MB
205 ms
```

La mejora es menor que con cuantificación.

Podríamos concluir:

**la cuantificación produce una mejora más significativa para este caso.**

Otro modelo podría mostrar resultados distintos.

Por eso debemos experimentar.

---

# 53. Seleccionar con evidencia

Una decisión técnica debería expresarse de manera similar a:

> La versión cuantificada fue seleccionada porque redujo significativamente el tamaño y la latencia del modelo, mientras la disminución observada en accuracy fue limitada.

Esto es mucho más sólido que:

> “Utilizamos cuantificación porque hace el modelo más rápido.”

La primera afirmación está respaldada por datos del propio experimento.

---

# 54. Optimización no debe modificar el problema

Después de optimizar, el flujo funcional continúa siendo:

**Imagen**

↓

**Preprocesamiento**

↓

**Modelo optimizado**

↓

**Predicción**

↓

**Clase + confianza**

El objetivo del modelo no cambia.

Cambia principalmente:

**cómo está representado y ejecutado.**

---

# 55. Verificación funcional

Después de la optimización debemos comprobar también que:

* el modelo carga correctamente;
* acepta el mismo formato de entrada;
* entrega la cantidad esperada de clases;
* produce predicciones razonables;
* conserva el orden correcto de las etiquetas.

Un modelo puede convertirse correctamente pero integrarse incorrectamente en la aplicación.

Por tanto:

**optimización técnica + prueba funcional**

son necesarias.

---

# 56. Modelo original como respaldo

Es recomendable mantener:

```text
modelo_original.keras
```

y generar una nueva versión:

```text
modelo_optimizado.tflite
```

En lugar de reemplazar directamente el original.

Esto permite:

* comparar;
* recuperar versiones;
* repetir experimentos;
* documentar resultados.

La trazabilidad utilizada durante el tratamiento de datos también debe aplicarse a los modelos.

---

# 57. Versionado de modelos

Podríamos utilizar nombres como:

```text
modelo_v1_cnn.keras
modelo_v2_finetuned.keras
modelo_v3_quantized.tflite
```

y documentar:

| Versión | Cambio       | Accuracy | Tamaño |
| ------- | ------------ | -------: | -----: |
| V1      | CNN original |      86% |  40 MB |
| V2      | Fine-Tuning  |      91% |  40 MB |
| V3      | Quantization |    90,5% |  12 MB |

Esto facilita reproducibilidad y selección.

---

# 58. ¿Qué no debemos hacer?

Algunas malas prácticas serían:

### Optimizar sin baseline

No podemos medir beneficio.

### Reportar solo tamaño

Ignoramos precisión y velocidad.

### Reportar solo accuracy

Ignoramos eficiencia.

### Aplicar muchas técnicas simultáneamente

No sabemos cuál produjo el cambio.

### No volver a evaluar

No sabemos si la transformación dañó el modelo.

### Sobrescribir el modelo original

Perdemos capacidad de comparación.

---

# 59. Preguntas para discusión en clase

### Caso 1

Dos modelos obtienen:

```text
A: 94% accuracy, 500 MB
B: 93% accuracy, 40 MB
```

**Pregunta:** ¿Podemos afirmar automáticamente que A es el mejor modelo para producción?

### Caso 2

Un modelo pasa de FP32 a INT8.

**Pregunta:** ¿Qué característica de sus parámetros estamos modificando principalmente?

### Caso 3

Después de cuantificar:

```text
Accuracy: 91% → 90,7%
Tamaño: 120 MB → 31 MB
```

**Pregunta:** ¿Cómo evaluaríamos este resultado?

### Caso 4

Un algoritmo establece en cero los pesos con magnitud muy pequeña.

**Pregunta:** ¿Qué técnica está aplicando?

### Caso 5

Un modelo posee 70% de sparsity, pero la velocidad no cambia.

**Pregunta:** ¿Por qué la reducción de pesos efectivos no garantiza automáticamente una reducción proporcional de latencia?

### Caso 6

Una dupla aplica pruning y quantization simultáneamente y obtiene una mejora.

**Pregunta:** ¿Puede determinar qué técnica generó la mayor parte de la mejora?

### Caso 7

Un clasificador será utilizado en un smartphone.

**Pregunta:** ¿Qué métricas adicionales al accuracy deberían considerarse?

---

# 60. Síntesis de la Semana 14

Al finalizar esta sesión deben quedar instaladas diez ideas fundamentales:

1. **Un modelo correctamente entrenado no necesariamente está preparado para producción.**
2. **La implementación exige considerar accuracy, tamaño, memoria, latencia y recursos computacionales.**
3. **Optimizar significa buscar una mejor relación entre desempeño predictivo y eficiencia.**
4. **La cuantificación reduce la precisión numérica utilizada para representar parámetros, por ejemplo de FP32 a FP16 o INT8.**
5. **Reducir precisión puede disminuir tamaño y acelerar inferencia, pero puede afectar las predicciones.**
6. **La poda elimina o reduce la influencia de parámetros considerados poco relevantes.**
7. **La poda puede ser no estructurada o estructurada, y su impacto real depende del entorno de ejecución.**
8. **Cuantificación y poda pueden combinarse, pero cada cambio debe ser evaluado objetivamente.**
9. **Toda optimización requiere comparar un modelo original con una versión optimizada utilizando una baseline.**
10. **La solución final debe buscar un modelo suficientemente preciso, pero también suficientemente eficiente para el contexto donde será implementado.**

### Hacia la Semana 15

Hasta ahora tenemos:

**Problema**

↓

**Dataset**

↓

**CNN**

↓

**Modelo entrenado**

↓

**Modelo optimizado**

La siguiente pregunta será:

**¿Cómo trasladamos este modelo desde el entorno donde fue entrenado hacia otros lenguajes, frameworks o plataformas de ejecución?**

La **Semana 15** abordará **interoperabilidad y ONNX**, estudiando cómo convertir y transportar modelos para facilitar su integración posterior en aplicaciones y distintos entornos de producción.

