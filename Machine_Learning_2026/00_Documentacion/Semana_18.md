# Semana 18 — Evaluación 4 y presentación del producto final

## 1. Propósito de la sesión

Al finalizar la sesión, el estudiante debería ser capaz de:

* Presentar una solución funcional de clasificación de imágenes.
* Explicar la arquitectura general del producto.
* Identificar el modelo final utilizado.
* Justificar las decisiones de optimización.
* Demostrar la ejecución de inferencia sobre imágenes nuevas.
* Explicar el entorno de despliegue seleccionado.
* Evidenciar la integración entre aplicación y modelo.
* Interpretar clase predicha y nivel de confianza.
* Mostrar resultados de pruebas funcionales.
* Explicar limitaciones, riesgos y posibles mejoras.
* Relacionar las cuatro evaluaciones como partes de un único proyecto integrador.

---

# 2. Cierre del proyecto integrador

Durante el semestre el proyecto avanzó progresivamente a través de cuatro etapas:

**Problema + prototipo inicial**

↓

**Dataset preparado**

↓

**CNN entrenada y evaluada**

↓

**Modelo optimizado + aplicación funcional + implementación**

El resultado esperado es una solución completa:

**Usuario carga una imagen**

↓

**Aplicación procesa la entrada**

↓

**Modelo realiza inferencia**

↓

**Aplicación muestra clase predicha**

↓

**Aplicación muestra nivel de confianza**

Esta sesión verifica que el proyecto funcione de extremo a extremo. 

---

# 3. Producto mínimo esperado

Cada dupla deberá presentar una aplicación capaz de realizar, como mínimo, el siguiente flujo:

**1. Recibir una imagen**

↓

**2. Validar la entrada**

↓

**3. Preprocesar correctamente**

↓

**4. Ejecutar el modelo**

↓

**5. Obtener la predicción**

↓

**6. Interpretar la salida**

↓

**7. Mostrar la clase**

↓

**8. Mostrar nivel de confianza**

El producto debe funcionar con una imagen que no forme parte del conjunto utilizado durante entrenamiento.

---

# 4. La evaluación no es solo una demostración visual

Una interfaz funcional constituye solamente una parte del producto.

La dupla también debe demostrar comprensión técnica de:

* problema;
* dataset;
* arquitectura;
* entrenamiento;
* evaluación;
* optimización;
* interoperabilidad;
* integración;
* despliegue.

Por tanto, la sesión no debería convertirse únicamente en:

> “subimos una imagen y el sistema respondió”.

La presentación debe explicar:

**cómo se llegó técnicamente a ese resultado.**

---

# 5. Estructura sugerida de la presentación

La presentación puede organizarse siguiendo el mismo ciclo del proyecto:

### 1. Problema

¿Qué se busca clasificar?

### 2. Dataset

¿Qué imágenes se utilizaron?

### 3. Modelo

¿Qué CNN se seleccionó?

### 4. Entrenamiento

¿Cómo se entrenó?

### 5. Evaluación

¿Qué resultados obtuvo?

### 6. Optimización

¿Qué se modificó para mejorar eficiencia?

### 7. Aplicación

¿Cómo se integró?

### 8. Despliegue

¿Dónde se ejecuta?

### 9. Demostración

¿Cómo funciona con una imagen nueva?

### 10. Conclusiones

¿Qué limitaciones y mejoras existen?

---

# 6. Componente 1: definición del problema

La dupla debe recordar brevemente:

**¿Qué necesidad motivó el proyecto?**

Por ejemplo:

> Clasificar automáticamente residuos en cuatro categorías mediante fotografías.

Debe indicar:

* contexto;
* clases;
* tipo de problema;
* tipo de aprendizaje.

Conceptualmente:

**Aprendizaje supervisado**

↓

**Clasificación multiclase**

↓

**Deep Learning**

↓

**CNN**

---

# 7. Componente 2: dataset

La presentación debe resumir el dataset definitivo.

Por ejemplo:

```text
Total: 4.400 imágenes

Clases:
- plástico: 1.100
- vidrio: 1.100
- cartón: 1.100
- metal: 1.100
```

Además:

```text
Train: 70%
Validation: 15%
Test: 15%
```

Debe indicarse:

* origen;
* limpieza;
* etiquetado;
* balance;
* resolución;
* data augmentation.

---

# 8. Componente 3: arquitectura final

La dupla debe indicar cuál fue el modelo seleccionado.

Por ejemplo:

```text
MobileNetV2
+
Global Average Pooling
+
Dense
+
Softmax
```

o:

```text
CNN propia
Conv 32
↓
Pooling
↓
Conv 64
↓
Pooling
↓
Conv 128
↓
Pooling
↓
Clasificador
```

Debe ser capaz de responder:

**¿Por qué se eligió esta arquitectura?**

---

# 9. Modelo desde cero o preentrenado

La dupla debe indicar si utilizó:

### CNN desde cero

o:

### Transfer Learning

y eventualmente:

### Fine-Tuning

Por ejemplo:

```text
Modelo base:
MobileNetV2

Pesos:
ImageNet

Primera etapa:
base congelada

Segunda etapa:
Fine-Tuning últimas 30 capas
```

La decisión debe justificarse con relación a:

* dataset;
* desempeño;
* recursos;
* implementación.

---

# 10. Componente 4: resultados de entrenamiento

Debe mostrarse evidencia del proceso.

Por ejemplo:

```text
Train accuracy: 92,4%
Validation accuracy: 89,8%
```

También pueden incorporarse curvas de:

* accuracy;
* loss.

La dupla debe ser capaz de explicar si observó:

* overfitting;
* underfitting;
* convergencia.

La gráfica debe interpretarse, no solamente mostrarse.

---

# 11. Evaluación final del modelo

El modelo definitivo debe evaluarse con el conjunto de test.

Por ejemplo:

```text
Test accuracy = 89,5%
```

Además puede mostrarse:

* matriz de confusión;
* precision;
* recall;
* F1-score.

La matriz de confusión permite analizar qué categorías presentan mayores dificultades.

---

# 12. Ejemplo de matriz de confusión

| Real / Predicha | Plástico | Vidrio | Cartón | Metal |
| --------------- | -------: | -----: | -----: | ----: |
| Plástico        |       92 |      2 |      5 |     1 |
| Vidrio          |        3 |     86 |      2 |     9 |
| Cartón          |        4 |      1 |     93 |     2 |
| Metal           |        2 |      8 |      1 |    89 |

La dupla debería observar, por ejemplo:

> El principal error aparece entre vidrio y metal.

Este análisis demuestra comprensión del comportamiento del modelo.

---

# 13. Selección del modelo definitivo

Si se realizaron varios experimentos, debería mostrarse una comparación.

Por ejemplo:

| Modelo                 | Test Accuracy | Tamaño |
| ---------------------- | ------------: | -----: |
| CNN propia             |           82% |  38 MB |
| MobileNetV2            |           88% |  14 MB |
| MobileNetV2 Fine-Tuned |           90% |  14 MB |

Después:

> Se seleccionó MobileNetV2 Fine-Tuned porque presentó el mejor equilibrio entre desempeño y tamaño.

Esto demuestra una decisión técnica basada en evidencia.

---

# 14. Componente 5: baseline de implementación

Antes de optimizar debería existir una versión original.

Por ejemplo:

```text
Modelo original

Accuracy: 90,1%
Tamaño: 84 MB
Latencia promedio: 240 ms
```

Esta información constituye la:

**baseline**

contra la cual evaluaremos la optimización.

---

# 15. Componente 6: optimización

La dupla deberá explicar qué técnica aplicó.

Por ejemplo:

### Cuantificación

```text
FP32 → INT8
```

o:

### Poda

```text
Pruning → 50% sparsity
```

o eventualmente ambas.

No basta con señalar que:

**“se optimizó el modelo”.**

Debe indicarse:

**qué transformación se realizó.**

---

# 16. Comparación antes y después

Una evidencia clara podría ser:

| Métrica  | Original | Optimizado |
| -------- | -------: | ---------: |
| Accuracy |    90,1% |      89,6% |
| Tamaño   |    84 MB |      23 MB |
| Latencia |   240 ms |     105 ms |

Interpretación:

**Accuracy:** -0,5 puntos

**Tamaño:** reducción significativa

**Latencia:** mejora importante

La dupla deberá justificar si el compromiso obtenido es aceptable.

---

# 17. Optimización como decisión

Una buena conclusión podría ser:

> Se seleccionó la versión cuantificada porque redujo significativamente el tamaño y la latencia, manteniendo una disminución acotada del desempeño predictivo.

La decisión debe responder al contexto del proyecto.

No existe obligación de que toda optimización mejore absolutamente todas las métricas.

---

# 18. Componente 7: interoperabilidad

Si se utilizó ONNX, la dupla debe explicar:

**Modelo original**

↓

**Conversión**

↓

**Modelo ONNX**

↓

**ONNX Runtime**

Debe demostrar que la conversión fue validada.

Por ejemplo:

```text
Keras:
Vidrio — 91,8%

ONNX:
Vidrio — 91,7%
```

Esto evidencia que la funcionalidad se conserva.

---

# 19. Contrato del modelo

La presentación debería indicar claramente:

### Entrada

```text
224 × 224 × 3
RGB
float32
```

### Preprocesamiento

```text
Normalización 0–1
```

### Salida

```text
4 scores
```

### Orden

```text
0 plástico
1 vidrio
2 cartón
3 metal
```

Esto demuestra control sobre la integración.

---

# 20. Componente 8: arquitectura de la aplicación

La dupla debería mostrar un diagrama sencillo:

**Usuario**

↓

**Interfaz**

↓

**Carga de imagen**

↓

**Preprocesamiento**

↓

**Modelo**

↓

**Postprocesamiento**

↓

**Resultado**

Si es local:

```text
Streamlit
+
ONNX Runtime
+
modelo.onnx
```

Si es cloud:

```text
Cliente
↓
API
↓
Servidor
↓
Modelo
```

---

# 21. Entorno de despliegue

La dupla debe indicar:

**LOCAL**

o:

**CLOUD**

según la alternativa seleccionada.

El descriptor permite ambas opciones. 

La decisión debe justificarse.

Por ejemplo:

> Se utilizó despliegue local porque la aplicación debe poder funcionar sin conexión y mantener las imágenes dentro del dispositivo.

---

# 22. Si se utiliza cloud

La presentación debería indicar conceptualmente:

**Cliente**

↓

**Internet**

↓

**Servicio**

↓

**Modelo**

↓

**Respuesta**

Puede mencionar:

* endpoint;
* API;
* servicio cloud utilizado.

No es necesario incorporar una arquitectura empresarial compleja.

El foco sigue siendo demostrar la implementación del modelo.

---

# 23. Si se utiliza local

Puede mostrarse:

```text
Equipo local
│
├── Aplicación
├── Modelo
├── Runtime
└── Dependencias
```

Debe indicar cómo se ejecuta.

Por ejemplo:

```text
streamlit run app.py
```

El evaluador debe poder observar que la solución realmente funciona fuera del notebook de entrenamiento.

---

# 24. Componente 9: demostración en vivo

La parte central será una demostración.

El flujo debería ser:

**1. Iniciar aplicación**

↓

**2. Seleccionar imagen nueva**

↓

**3. Mostrar imagen**

↓

**4. Ejecutar clasificación**

↓

**5. Mostrar resultado**

Ejemplo:

```text
Predicción:
Vidrio

Confianza:
92,4%
```

La imagen no debería corresponder simplemente a una observación previamente mostrada durante el entrenamiento.

---

# 25. Probar más de una clase

La demostración debería incorporar idealmente varias categorías.

Por ejemplo:

**Imagen 1 → plástico**

**Imagen 2 → vidrio**

**Imagen 3 → cartón**

Esto disminuye la posibilidad de que el producto funcione solamente para un ejemplo preparado.

---

# 26. Caso de baja confianza

También puede resultar útil mostrar una imagen difícil.

Por ejemplo:

```text
Cartón → 42%
Plástico → 34%
Vidrio → 14%
Metal → 10%
```

La aplicación podría responder:

```text
Predicción con baja confianza
```

Esto demuestra que se consideró la incertidumbre operacional.

---

# 27. Entrada inválida

También puede demostrarse:

```text
archivo.pdf
```

Resultado esperado:

```text
Formato no compatible.
Seleccione JPG o PNG.
```

Esto permite evidenciar manejo básico de errores.

---

# 28. Imagen fuera del dominio

Puede probarse una imagen de un objeto no perteneciente a las clases.

Por ejemplo:

**un perro**

en un clasificador de residuos.

La dupla debe reconocer que:

**el modelo puede igualmente forzar una clasificación.**

Esto constituye una limitación importante de un clasificador cerrado.

---

# 29. Limitaciones del sistema

La presentación debe incluir explícitamente limitaciones.

Por ejemplo:

* dataset pequeño;
* determinadas condiciones de iluminación;
* clases visualmente similares;
* baja confianza en imágenes lejanas;
* modelo incapaz de reconocer clases desconocidas;
* necesidad de conexión a Internet;
* hardware limitado.

Reconocer limitaciones no debilita la presentación.

Demuestra comprensión técnica.

---

# 30. Riesgos de generalización

La dupla debería preguntarse:

**¿Qué ocurrirá si las imágenes reales cambian respecto del dataset?**

Por ejemplo:

* nueva cámara;
* iluminación distinta;
* nuevos fondos;
* objetos parcialmente ocultos.

Esto puede producir:

**data drift**

y degradar el comportamiento.

Por tanto, una solución real requeriría monitoreo.

---

# 31. Componente 10: integración continua

No es necesario implementar una plataforma MLOps completa.

Pero debe existir al menos una propuesta de actualización.

Por ejemplo:

```text
Nuevo modelo
↓
Prueba automática
↓
Verificar entrada
↓
Verificar salida
↓
Verificar accuracy
↓
Verificar latencia
↓
Aprobar
↓
Desplegar
```

Esto responde al contenido de integración continua del descriptor. 

---

# 32. Pruebas mínimas propuestas

Una estrategia simple podría verificar:

```text
✓ Modelo carga
```

```text
✓ Aplicación inicia
```

```text
✓ Input correcto
```

```text
✓ Output correcto
```

```text
✓ Imagen válida clasificada
```

```text
✓ Archivo inválido rechazado
```

```text
✓ Latencia dentro del límite
```

Esta propuesta demuestra cómo mantener la calidad del producto.

---

# 33. Versionado

La dupla debería mostrar una lógica clara de versiones.

Por ejemplo:

```text
modelo_v1.keras
modelo_v2_finetuned.keras
modelo_v3_quantized.onnx
```

y:

```text
app_v1.0
```

Esto permite identificar exactamente:

**qué versión del modelo utiliza la aplicación.**

---

# 34. Plan de rollback

También puede indicarse:

> Si la nueva versión del modelo no supera las pruebas definidas o genera errores en producción, se conservará la versión anterior como alternativa de recuperación.

Conceptualmente:

**V1 estable**

↓

**V2 nueva**

↓

**Falla**

↓

**Volver a V1**

Esta estrategia se denomina:

**rollback**.

---

# 35. Documentación del producto

La entrega debería permitir que otra persona comprenda:

* qué hace el sistema;
* qué necesita;
* cómo ejecutarlo;
* qué modelo utiliza;
* qué clases reconoce.

Puede incluir un:

```text
README.md
```

con información básica.

Por ejemplo:

```text
1. Instalar dependencias
2. Ejecutar aplicación
3. Seleccionar imagen
4. Presionar clasificar
```

---

# 36. Dependencias

Una lista básica podría contener:

```text
streamlit
onnxruntime
numpy
pillow
```

o las bibliotecas correspondientes al proyecto.

Mantener estas dependencias registradas mejora reproducibilidad.

Por ejemplo:

```text
requirements.txt
```

---

# 37. Reproducibilidad del producto

Una solución académica sólida debería poder ejecutarse nuevamente.

No debería depender de:

* archivos desconocidos;
* rutas absolutas del computador;
* notebooks abiertos;
* configuraciones no documentadas.

Por tanto:

**“funciona en mi computador”**

no constituye una evidencia suficiente de implementación.

---

# 38. Prueba en un entorno limpio

Idealmente, antes de presentar:

**cerrar notebook**

↓

**reiniciar aplicación**

↓

**cargar modelo**

↓

**clasificar imagen**

Si funciona:

**la solución posee mayor independencia del entorno de desarrollo.**

Esta es una prueba muy útil antes de la evaluación.

---

# 39. Calidad del código

No esperamos necesariamente una aplicación empresarial.

Pero sí conviene mantener:

* funciones;
* nombres claros;
* separación lógica;
* manejo de errores;
* ausencia de código innecesario.

Por ejemplo:

```text
preprocessing.py
inference.py
app.py
```

es preferible a un único archivo desordenado con cientos de líneas mezcladas.

---

# 40. Evaluación del producto completo

Podemos pensar en cuatro dimensiones:

### Modelo

¿Predice adecuadamente?

### Eficiencia

¿Utiliza razonablemente los recursos?

### Integración

¿La aplicación utiliza correctamente el modelo?

### Operación

¿Puede ejecutarse de manera estable?

El producto final debe equilibrar estas dimensiones.

---

# 41. Ejemplo final: clasificación de residuos

### Problema

Clasificar:

* plástico;
* vidrio;
* cartón;
* metal.

### Dataset

```text
4.400 imágenes
```

### Modelo

```text
MobileNetV2 Fine-Tuned
```

### Test Accuracy

```text
90,2%
```

### Optimización

```text
INT8
```

### Tamaño

```text
82 MB → 24 MB
```

### Aplicación

```text
Streamlit
```

### Runtime

```text
ONNX Runtime
```

### Implementación

```text
Local
```

### Resultado

```text
Usuario carga botella.jpg
↓
Vidrio — 93%
```

Este ejemplo resume el tipo de solución esperada.

---

# 42. Presentación técnica versus relato cronológico

No es necesario contar cada detalle ocurrido.

Conviene presentar:

**decisiones**

**resultados**

**evidencia**

**producto**

Por ejemplo:

No:

> “Primero probamos esto, después pasó esto, después pensamos...”

Mejor:

> “Se evaluaron tres arquitecturas. MobileNetV2 Fine-Tuned fue seleccionada porque presentó 90,2% de accuracy y menor tamaño que las alternativas.”

La presentación final debería ser concisa y técnica.

---

# 43. Justificar decisiones

La dupla debería estar preparada para responder:

**¿Por qué esas clases?**

**¿Por qué ese dataset?**

**¿Por qué esa arquitectura?**

**¿Por qué esa optimización?**

**¿Por qué ONNX?**

**¿Por qué local o cloud?**

**¿Por qué ese umbral de confianza?**

La capacidad de justificar decisiones constituye una evidencia de aprendizaje superior a simplemente mostrar código.

---

# 44. Demostración sin editar resultados

La aplicación debe mostrar el resultado generado realmente por el modelo.

No debería utilizar:

* resultados escritos manualmente;
* capturas preparadas;
* predicciones precalculadas.

El flujo debe ser ejecutado durante la presentación.

Conceptualmente:

**entrada real → inferencia real → salida real.**

---

# 45. ¿Qué ocurre si el modelo falla durante la presentación?

Una predicción incorrecta no significa automáticamente que todo el proyecto sea incorrecto.

Puede convertirse en una oportunidad para analizar:

**¿por qué falló?**

Por ejemplo:

* imagen fuera de distribución;
* fondo diferente;
* clase visualmente similar;
* baja iluminación.

Una explicación técnica adecuada demuestra comprensión.

---

# 46. Una buena demostración incluye errores

Mostrar solamente:

**tres imágenes fáciles correctamente clasificadas**

puede ocultar limitaciones.

Una demostración más rica puede incluir:

* un caso sencillo;
* un caso difícil;
* un error;
* una entrada inválida.

Esto muestra una evaluación más realista del sistema.

---

# 47. Reflexión final

La dupla debería responder:

**¿Qué haríamos si este producto debiera utilizarse realmente?**

Por ejemplo:

* recopilar más imágenes;
* ampliar clases;
* mejorar balance;
* optimizar más;
* desplegar cloud;
* monitorear drift;
* automatizar CI;
* incorporar usuarios.

Esto permite distinguir:

**prototipo académico**

de:

**sistema de producción completo.**

---

# 48. Proyecto académico versus producción real

Nuestro producto final demuestra:

**viabilidad técnica**

pero un sistema real podría necesitar además:

* seguridad;
* escalabilidad;
* monitoreo;
* auditoría;
* privacidad;
* mantenimiento;
* disponibilidad;
* soporte.

Por tanto, la evaluación final representa:

**un prototipo funcional implementado**

y no necesariamente:

**una solución empresarial completa.**

---

# 49. Lista de verificación antes de presentar

La dupla debería comprobar:

* aplicación inicia;
* modelo carga;
* clases están correctamente mapeadas;
* imágenes se procesan correctamente;
* predicciones aparecen;
* confianza se muestra;
* existe manejo de errores;
* versión del modelo está identificada;
* documentación está disponible;
* las pruebas fueron ejecutadas.

Esto reduce fallos evitables durante la evaluación.

---

# 50. Evidencias técnicas recomendadas

La presentación debería contener al menos evidencia de:

### Dataset

Distribución de clases.

### Entrenamiento

Curvas o métricas.

### Evaluación

Test y matriz de confusión.

### Optimización

Comparación antes/después.

### Aplicación

Captura o demostración.

### Despliegue

Arquitectura local/cloud.

### Pruebas

Resultados básicos.

Así se evidencia el recorrido completo.

---

# 51. Preguntas para discusión/evaluación

### Caso 1

El modelo posee 95% de accuracy pero la aplicación no puede cargarlo.

**Pregunta:** ¿Tenemos un producto final funcional?

### Caso 2

La aplicación funciona correctamente pero el modelo obtiene 55% en test.

**Pregunta:** ¿La buena interfaz compensa el bajo desempeño predictivo?

### Caso 3

El modelo optimizado es más pequeño pero pierde 15 puntos de accuracy.

**Pregunta:** ¿Podemos afirmar automáticamente que la optimización fue exitosa?

### Caso 4

La solución fue implementada localmente.

**Pregunta:** ¿Esto incumple el descriptor por no utilizar cloud?

### Caso 5

Una nueva versión del modelo produce cinco clases, mientras la aplicación espera cuatro.

**Pregunta:** ¿Qué mecanismo de validación debería detectar este cambio?

### Caso 6

La aplicación muestra “95% de certeza”.

**Pregunta:** ¿Es técnicamente apropiado interpretar directamente Softmax como certeza absoluta?

### Caso 7

El modelo falla con fotografías tomadas en condiciones distintas al dataset.

**Pregunta:** ¿Qué problema de generalización puede existir?

### Caso 8

El equipo puede demostrar técnicamente todas las etapas, pero no justificar por qué seleccionó su modelo final.

**Pregunta:** ¿Qué componente del aprendizaje falta evidenciar?

---

# 52. Síntesis de la Semana 18

Al finalizar esta sesión deben quedar consolidadas diez ideas fundamentales:

1. **El proyecto integrador culmina en una solución funcional de clasificación de imágenes mediante Deep Learning.**
2. **Las cuatro evaluaciones representan etapas acumulativas de un mismo proyecto: problema, datos, modelo e implementación.**
3. **El producto final debe ejecutar inferencia real sobre imágenes nuevas.**
4. **La aplicación debe reproducir correctamente el preprocesamiento utilizado durante entrenamiento.**
5. **El modelo final debe estar respaldado por evidencia de entrenamiento, validación y evaluación.**
6. **La optimización debe justificarse comparando desempeño y eficiencia antes y después.**
7. **El despliegue puede ser local o cloud, siempre que exista una justificación técnica coherente.**
8. **ONNX, versionado e integración continua permiten mejorar interoperabilidad y mantenibilidad de la solución.**
9. **Reconocer limitaciones y errores constituye parte de la evaluación profesional de un modelo.**
10. **El resultado final no es solamente una CNN entrenada, sino un prototipo funcional capaz de transformar una imagen proporcionada por un usuario en una clasificación interpretable.**

