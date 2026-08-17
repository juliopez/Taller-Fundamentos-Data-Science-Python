# Semana 17 — Despliegue local y cloud, Edge Computing e Integración Continua

## 1. Propósito de la sesión

Comprender las principales alternativas para **desplegar y operar un modelo de Deep Learning en un entorno real**, diferenciando implementación local, servicios cloud y Edge Computing, e introduciendo además el concepto de **integración continua** aplicado al ciclo de vida de modelos de Machine Learning. 

Al finalizar la sesión, el estudiante debería ser capaz de:

* Diferenciar entrenamiento, integración y despliegue.
* Explicar qué significa desplegar un modelo.
* Comparar despliegue local, cloud y Edge.
* Identificar ventajas y limitaciones de cada alternativa.
* Comprender el concepto de servicio de inferencia.
* Reconocer el papel de una API en una arquitectura distribuida.
* Identificar servicios cloud orientados a Machine Learning.
* Comprender qué es Edge Computing.
* Analizar latencia, privacidad, conectividad y costo como criterios de despliegue.
* Explicar el concepto de integración continua.
* Comprender el versionado de código y modelos.
* Identificar un flujo básico de actualización y validación de modelos.
* Diseñar una estrategia de despliegue apropiada para el proyecto integrador.

---

# 2. ¿Qué significa desplegar un modelo?

Durante las semanas anteriores construimos:

**Modelo entrenado**

↓

**Modelo optimizado**

↓

**Modelo interoperable**

↓

**Aplicación funcional**

Ahora necesitamos decidir:

**¿Dónde funcionará esta solución?**

El **despliegue** o *deployment* corresponde al proceso mediante el cual una aplicación o modelo pasa desde el entorno de desarrollo hacia un entorno donde puede ser utilizado.

Conceptualmente:

**Desarrollo**

↓

**Pruebas**

↓

**Despliegue**

↓

**Uso**

En Machine Learning esto implica poner a disposición tanto:

* la aplicación;
* el modelo;
* las dependencias;
* el entorno de ejecución.

---

# 3. Entrenar no es desplegar

Es importante distinguir:

### Entrenamiento

```text
Datos
↓
Modelo
↓
Backpropagation
↓
Pesos aprendidos
```

### Despliegue

```text
Modelo entrenado
↓
Entorno operativo
↓
Nueva entrada
↓
Inferencia
```

Durante producción normalmente no entrenamos nuevamente el modelo cada vez que llega una imagen.

Ejecutamos:

**inferencia**

con un modelo ya aprendido.

---

# 4. Tres escenarios principales

Podemos considerar tres alternativas generales:

### Despliegue local

El modelo funciona en el mismo computador o infraestructura del usuario.

### Cloud

El modelo se ejecuta en infraestructura remota accesible mediante red.

### Edge Computing

El modelo funciona cerca del lugar donde se generan los datos, normalmente en dispositivos con recursos limitados.

Conceptualmente:

**Local**

**Cloud**

**Edge**

Cada enfoque presenta ventajas y restricciones diferentes.

---

# 5. Despliegue local

En una implementación local:

**Aplicación + Modelo**

se encuentran en el mismo equipo o infraestructura local.

Por ejemplo:

```text
Notebook/PC
│
├── Aplicación Streamlit
├── modelo.onnx
└── ONNX Runtime
```

El usuario ejecuta todo directamente en su computador.

No necesita enviar la imagen hacia un servidor externo.

---

# 6. Ventajas del despliegue local

Entre sus principales ventajas:

### Baja dependencia de Internet

La aplicación puede funcionar sin conexión.

### Privacidad

Los datos pueden permanecer dentro del dispositivo.

### Simplicidad

Puede resultar más sencillo para proyectos pequeños.

### Ausencia de costos cloud permanentes

No necesitamos necesariamente pagar infraestructura remota.

Para nuestro proyecto académico, esta opción puede ser perfectamente válida.

---

# 7. Limitaciones del despliegue local

También presenta dificultades:

### Instalación

Cada equipo debe disponer de:

* Python;
* dependencias;
* runtime;
* modelo.

### Actualización

Cambiar el modelo puede requerir distribuir nuevas versiones.

### Recursos limitados

El dispositivo debe ser capaz de ejecutar el modelo.

### Compatibilidad

Diferentes sistemas operativos pueden presentar problemas.

Por tanto, local no significa necesariamente:

**más sencillo en todos los escenarios.**

---

# 8. Despliegue cloud

En un despliegue cloud el modelo se ejecuta en infraestructura remota.

Conceptualmente:

```text
Usuario
↓
Internet
↓
Servidor Cloud
↓
Modelo
↓
Predicción
↓
Usuario
```

La aplicación cliente no necesita necesariamente almacenar ni ejecutar directamente la CNN.

Puede enviar la imagen hacia un servicio.

---

# 9. Separar cliente y modelo

Una arquitectura cloud puede dividirse en:

### Cliente

* interfaz;
* selección de imagen;
* presentación del resultado.

### Backend

* recibe solicitud;
* preprocesa;
* ejecuta modelo;
* devuelve predicción.

Conceptualmente:

```text
Cliente
↓
HTTP
↓
API
↓
Modelo
↓
Respuesta JSON
```

Esto permite centralizar la lógica del modelo.

---

# 10. ¿Qué es una API?

Una **API (Application Programming Interface)** permite que diferentes aplicaciones se comuniquen mediante una interfaz definida.

En nuestro contexto:

**Aplicación**

envía:

```text
imagen
```

y recibe:

```text
clase + confianza
```

Por ejemplo:

```json
{
  "clase": "vidrio",
  "confianza": 0.92
}
```

La API oculta los detalles internos del modelo.

---

# 11. Flujo de una API de inferencia

Conceptualmente:

**1. Cliente selecciona imagen**

↓

**2. Envía solicitud**

↓

**3. Servidor recibe imagen**

↓

**4. Preprocesa**

↓

**5. Ejecuta modelo**

↓

**6. Interpreta salida**

↓

**7. Devuelve respuesta**

↓

**8. Cliente muestra resultado**

La inteligencia permanece en el backend.

---

# 12. API con FastAPI

En Python podemos construir servicios utilizando herramientas como:

**FastAPI**

Por ejemplo, conceptualmente:

```python
from fastapi import FastAPI

app = FastAPI()
```

Después podemos crear una ruta:

```python
@app.post("/predict")
def predict():
    ...
```

Esta ruta puede recibir una imagen y devolver una predicción.

No necesitamos implementar una API completa en profundidad para comprender el concepto.

---

# 13. Ejemplo de respuesta

Una respuesta puede contener:

```json
{
  "prediction": "carton",
  "confidence": 0.87,
  "model_version": "v1.2"
}
```

Esto proporciona:

* resultado;
* confianza;
* versión del modelo.

La aplicación cliente decide después cómo presentar esta información.

---

# 14. Ventajas del cloud

Entre las ventajas:

### Centralización

Existe una sola versión operativa del modelo.

### Actualización rápida

Podemos reemplazar el modelo en el servidor.

### Escalabilidad

La infraestructura puede ajustarse según demanda.

### Acceso desde múltiples clientes

Web, móvil, escritorio.

### Recursos especializados

CPU, GPU y aceleradores pueden estar disponibles bajo demanda.

---

# 15. Limitaciones del cloud

También existen desafíos:

### Dependencia de conectividad

Sin Internet, el servicio puede dejar de estar disponible.

### Latencia de red

La imagen debe viajar hacia el servidor.

### Costos

Compute, almacenamiento y transferencia pueden generar gastos.

### Privacidad

Las imágenes salen del dispositivo.

### Seguridad

Debemos proteger:

* API;
* credenciales;
* datos;
* infraestructura.

Por tanto, cloud tampoco es automáticamente la mejor alternativa.

---

# 16. Servicios cloud de Machine Learning

Los proveedores cloud ofrecen servicios especializados para:

* entrenamiento;
* almacenamiento de modelos;
* endpoints de inferencia;
* monitoreo;
* escalamiento.

Ejemplos conocidos incluyen servicios de:

* AWS;
* Microsoft Azure;
* Google Cloud.

El descriptor exige conocer conceptualmente los **servicios cloud machine learning**, no necesariamente dominar un proveedor específico. 

---

# 17. Endpoint de inferencia

Un **endpoint** representa una dirección accesible mediante la cual podemos enviar datos al modelo.

Conceptualmente:

```text
POST /predict
```

Entrada:

```text
imagen
```

Salida:

```text
predicción
```

Por tanto:

**modelo desplegado + API accesible = servicio de inferencia**

El usuario no necesita conocer dónde está físicamente el modelo.

---

# 18. Escalamiento

Supongamos que inicialmente tenemos:

**10 predicciones al día**

Un servidor pequeño puede ser suficiente.

Después:

**10.000 predicciones por minuto**

La infraestructura deberá aumentar su capacidad.

El **escalamiento** permite adaptar recursos a la demanda.

En cloud esto puede realizarse de forma:

* manual;
* automática.

Esta es una ventaja importante frente a una instalación exclusivamente local.

---

# 19. Escalamiento vertical y horizontal

Podemos distinguir:

### Escalamiento vertical

Aumentar capacidad de una máquina.

Por ejemplo:

* más CPU;
* más RAM;
* GPU mejor.

### Escalamiento horizontal

Agregar más instancias.

Conceptualmente:

```text
Servidor 1
Servidor 2
Servidor 3
Servidor 4
```

y distribuir las solicitudes.

Esto permite manejar mayor carga.

---

# 20. Costo de inferencia

En cloud debemos considerar:

**cada inferencia consume recursos.**

Un modelo muy grande puede requerir:

* GPU;
* mucha memoria;
* mayor tiempo.

Esto incrementa costos.

Aquí se conecta directamente la Semana 14:

**modelo optimizado → menor consumo → potencial menor costo**

Por tanto, optimización y despliegue no son temas independientes.

---

# 21. Latencia total

En una arquitectura cloud, el tiempo observado por el usuario incluye más que la inferencia.

Conceptualmente:

**Latencia total =**

**subida de imagen**

*

**procesamiento**

*

**inferencia**

*

**respuesta**

Por ejemplo:

```text
Red: 120 ms
Preprocesamiento: 30 ms
Inferencia: 90 ms
Respuesta: 40 ms
```

Total:

```text
280 ms
```

La latencia del modelo es solo una parte.

---

# 22. ¿Qué es Edge Computing?

**Edge Computing** consiste en procesar información cerca del lugar donde se genera, en lugar de enviarla necesariamente hacia una infraestructura central.

Por ejemplo:

**Cámara**

↓

**Dispositivo local**

↓

**Modelo**

↓

**Predicción**

sin necesidad de enviar cada imagen a la nube.

El término:

**Edge**

hace referencia al borde de la red.

---

# 23. Ejemplos de Edge Computing

Podemos encontrar Edge ML en:

* teléfonos móviles;
* cámaras inteligentes;
* robots;
* vehículos;
* sensores industriales;
* Raspberry Pi;
* dispositivos IoT.

En estos casos el modelo se ejecuta:

**cerca de la fuente de los datos.**

Esto puede resultar muy importante cuando necesitamos respuestas rápidas o privacidad.

---

# 24. Ventajas del Edge Computing

### Baja latencia

No necesitamos esperar comunicación con un servidor remoto.

### Funcionamiento offline

Puede operar sin conexión.

### Privacidad

Los datos pueden permanecer localmente.

### Menor tráfico de red

No es necesario transferir continuamente imágenes.

### Respuesta inmediata

Útil en sistemas en tiempo real.

---

# 25. Limitaciones del Edge Computing

Los dispositivos Edge suelen tener:

* menos RAM;
* menor CPU;
* menor capacidad energética;
* almacenamiento limitado.

Por ello requieren modelos:

* pequeños;
* rápidos;
* eficientes.

Esto conecta directamente con:

**cuantificación**

y:

**poda**

estudiadas en Semana 14.

---

# 26. Cloud versus Edge

Podemos comparar:

| Aspecto         | Cloud                              | Edge                               |
| --------------- | ---------------------------------- | ---------------------------------- |
| Recursos        | Altos                              | Limitados                          |
| Conectividad    | Necesaria generalmente             | Puede funcionar offline            |
| Latencia de red | Existe                             | Muy baja                           |
| Privacidad      | Datos pueden salir del dispositivo | Datos pueden permanecer localmente |
| Actualización   | Centralizada                       | Puede ser más compleja             |
| Escalabilidad   | Alta                               | Depende del dispositivo            |

No existe una respuesta universal.

La arquitectura depende del contexto.

---

# 27. Arquitectura híbrida

También podemos combinar ambos enfoques.

Por ejemplo:

**Edge**

realiza:

* inferencia inmediata.

Y:

**Cloud**

realiza:

* almacenamiento;
* analítica;
* entrenamiento;
* actualización de modelos.

Conceptualmente:

```text
Dispositivo Edge
↓
Inferencia local
↓
Datos seleccionados
↓
Cloud
↓
Reentrenamiento
```

Esto permite aprovechar ventajas de ambos mundos.

---

# 28. ¿Dónde desplegar nuestro proyecto?

Para el proyecto integrador podemos considerar principalmente:

### Opción A

**Aplicación local**

Ejemplo:

Streamlit + ONNX Runtime.

### Opción B

**Aplicación cloud**

Ejemplo:

interfaz web + servicio de inferencia.

El descriptor permite explícitamente implementar **en la nube o en modo local**. 

Por tanto, no es obligatorio que todas las duplas utilicen cloud.

---

# 29. Criterios para elegir

La dupla debería justificar la alternativa seleccionada considerando:

* disponibilidad de Internet;
* privacidad;
* velocidad;
* recursos;
* costos;
* facilidad de implementación;
* cantidad esperada de usuarios;
* entorno objetivo.

Por ejemplo:

> Se seleccionó despliegue local porque el sistema debe funcionar sin conectividad y procesar imágenes sin enviarlas a servidores externos.

Eso representa una decisión técnica fundamentada.

---

# 30. ¿Qué es Integración Continua?

El descriptor incorpora además:

**Integración Continua**

o:

**Continuous Integration (CI)**. 

CI es una práctica en la que los cambios realizados en un proyecto se integran frecuentemente y son sometidos automáticamente a validaciones.

Conceptualmente:

**Cambio de código**

↓

**Repositorio**

↓

**Pruebas automáticas**

↓

**Resultado**

La finalidad es detectar problemas tempranamente.

---

# 31. Integración Continua en software tradicional

Supongamos que un desarrollador modifica:

```text
preprocessing.py
```

y accidentalmente cambia:

```text
224 × 224
```

por:

```text
256 × 256
```

Una prueba automática podría detectar:

```text
Input shape incorrect
```

antes de que la aplicación llegue a producción.

Por tanto:

**automatizar pruebas reduce errores de integración.**

---

# 32. Repositorio de código

CI se utiliza normalmente junto a sistemas de control de versiones.

Por ejemplo:

**Git**

El flujo puede ser:

```text
Cambio
↓
Commit
↓
Push
↓
Pipeline CI
↓
Pruebas
```

Esto permite registrar:

* qué cambió;
* quién lo cambió;
* cuándo;
* qué pruebas fueron ejecutadas.

La reproducibilidad también aplica al software.

---

# 33. CI en Machine Learning

En Machine Learning existen componentes adicionales.

No solamente cambia:

**código**

También puede cambiar:

* dataset;
* modelo;
* hiperparámetros;
* preprocesamiento;
* dependencias.

Por tanto, un pipeline puede verificar:

**Código**

*

**Modelo**

*

**Datos de prueba**

*

**Contrato de entrada/salida**

---

# 34. Ejemplo de prueba automatizada

Podemos definir:

```text
Modelo debe cargar correctamente
```

Otra:

```text
Input esperado = 224 × 224 × 3
```

Otra:

```text
Output debe contener 4 valores
```

Otra:

```text
Inferencia no debe generar error
```

Otra:

```text
Accuracy mínima >= 85%
```

Estas pruebas pueden ejecutarse automáticamente después de una actualización.

---

# 35. Pipeline conceptual

Un flujo básico de CI podría ser:

**1. Cambio en repositorio**

↓

**2. Instalar dependencias**

↓

**3. Cargar modelo**

↓

**4. Ejecutar pruebas**

↓

**5. Verificar resultados**

↓

### Si falla

**No desplegar**

### Si pasa

**Permitir nueva versión**

Esto reduce el riesgo de publicar una solución defectuosa.

---

# 36. Continuous Delivery y Continuous Deployment

Relacionado con CI encontramos:

### Continuous Delivery

La solución queda lista para ser desplegada, pero una persona puede aprobar el paso final.

### Continuous Deployment

Después de superar las pruebas, la nueva versión se despliega automáticamente.

Conceptualmente:

**CI**

↓

**Pruebas**

↓

**Delivery**

↓

**Deployment**

Para nuestro curso basta comprender esta relación general.

---

# 37. Modelo como artefacto versionado

En Machine Learning deberíamos mantener versiones como:

```text
modelo_v1.onnx
modelo_v2.onnx
modelo_v3.onnx
```

No simplemente:

```text
modelo_final.onnx
modelo_final2.onnx
modelo_final_ahora_si.onnx
```

Un versionado claro permite:

* comparar;
* revertir;
* identificar producción;
* documentar cambios.

---

# 38. Versión de código y modelo

Una aplicación puede registrar:

```text
app_version = 1.3
model_version = 2.1
```

Esto permite responder:

**¿Qué modelo estaba ejecutándose cuando ocurrió una determinada predicción?**

Si posteriormente descubrimos un problema:

**podemos identificar exactamente la versión afectada.**

---

# 39. Actualización del modelo

Supongamos que encontramos nuevas imágenes y entrenamos:

**modelo_v2**

Antes de reemplazar:

**modelo_v1**

deberíamos comprobar:

* accuracy;
* comportamiento por clase;
* latencia;
* tamaño;
* contrato de entrada;
* pruebas funcionales.

Conceptualmente:

**nuevo modelo ≠ automáticamente mejor versión**

Debe superar criterios previamente definidos.

---

# 40. Regression testing

En software, una regresión ocurre cuando un cambio rompe algo que antes funcionaba.

En ML también puede ocurrir.

Por ejemplo:

**V1**

```text
Metal recall = 90%
```

**V2**

```text
Metal recall = 61%
```

aunque:

```text
accuracy global aumentó
```

Una prueba de regresión podría detectar este deterioro.

Por tanto:

**comparar modelo nuevo con modelo anterior** es importante.

---

# 41. Criterios de aprobación de una nueva versión

Podemos definir reglas como:

```text
Accuracy >= 88%
```

```text
Ninguna clase con recall < 75%
```

```text
Latencia < 300 ms
```

```text
Tamaño < 50 MB
```

Si el modelo no cumple:

**no pasa a producción.**

Estas reglas representan requerimientos operacionales.

---

# 42. Prueba de contrato

También podemos verificar automáticamente:

```text
Input = [1,224,224,3]
Output = [1,4]
```

Si una nueva versión cambia:

```text
Output = [1,5]
```

la aplicación podría romperse.

La prueba debería detectar:

**cambio incompatible.**

Esto relaciona CI con el contrato definido en Semana 15.

---

# 43. Dependencias

Una aplicación depende de bibliotecas.

Por ejemplo:

```text
tensorflow
onnxruntime
numpy
pillow
streamlit
```

Cambiar versiones puede provocar incompatibilidades.

Por ello normalmente documentamos:

```text
requirements.txt
```

Ejemplo:

```text
onnxruntime==1.x
numpy==2.x
streamlit==1.x
```

Esto mejora la reproducibilidad del entorno.

---

# 44. Contenedores como concepto

En despliegues reales es frecuente utilizar contenedores.

Por ejemplo:

**Docker**

Un contenedor puede incluir:

* aplicación;
* dependencias;
* runtime;
* configuración.

Conceptualmente:

```text
Código + Dependencias
↓
Contenedor
↓
Entorno reproducible
```

No es necesario convertir Docker en contenido central, pero ayuda a comprender cómo se empaquetan soluciones para producción.

---

# 45. Cloud y contenedores

Un servicio cloud puede ejecutar:

**contenedor**

que contiene:

```text
API
modelo.onnx
ONNX Runtime
dependencias
```

La infraestructura se encarga de ejecutar la instancia.

Esto facilita trasladar una aplicación entre diferentes entornos.

De nuevo aparece:

**portabilidad**

como concepto transversal.

---

# 46. Seguridad de una API

Si publicamos un endpoint en Internet debemos pensar en:

* autenticación;
* autorización;
* límites de tamaño;
* validación de archivos;
* tráfico malicioso;
* cifrado;
* registro.

Una API abierta sin control podría recibir:

* archivos enormes;
* solicitudes masivas;
* contenido inválido.

Aunque el proyecto sea académico, reconocer estas amenazas forma parte de una implementación profesional.

---

# 47. Límites de entrada

Por ejemplo, podemos definir:

```text
Formatos permitidos:
JPG / PNG
```

```text
Tamaño máximo:
5 MB
```

Esto reduce riesgos y evita cargas innecesarias.

La validación estudiada en Semana 16 sigue siendo necesaria incluso cuando el modelo se despliega en cloud.

---

# 48. Monitoreo

Después del despliegue necesitamos saber si la solución sigue funcionando.

Podemos monitorear:

* cantidad de solicitudes;
* latencia;
* errores;
* uso de CPU;
* memoria;
* clases predichas.

Conceptualmente:

**desplegar no significa terminar el trabajo.**

Una solución en producción debe observarse.

---

# 49. Monitoreo del modelo

También podemos registrar:

```text
confianza promedio
```

```text
proporción por clase
```

```text
cantidad de predicciones con baja confianza
```

Si cambia drásticamente el comportamiento, podría indicar:

* datos diferentes;
* errores;
* cambio en condiciones reales.

Esto introduce un concepto relevante:

**data drift.**

---

# 50. Data Drift

**Data Drift** ocurre cuando la distribución de los datos reales cambia respecto de los utilizados para construir el modelo.

Ejemplo:

El clasificador se entrenó con:

**fotografías de buena iluminación**

pero seis meses después los usuarios comienzan a utilizar:

**una nueva cámara con características diferentes.**

El desempeño podría degradarse.

El modelo no cambió.

Cambió:

**el mundo que recibe como entrada.**

---

# 51. Concept drift

Existe también:

**Concept Drift**

cuando cambia la relación entre entrada y resultado.

Por ejemplo, categorías o criterios de clasificación pueden evolucionar.

Aunque no profundizaremos técnicamente en estos fenómenos, son importantes para comprender que:

**un modelo desplegado puede necesitar mantenimiento.**

---

# 52. MLOps

El conjunto de prácticas destinadas a gestionar el ciclo de vida de modelos suele relacionarse con:

**MLOps**

Conceptualmente integra:

* Machine Learning;
* desarrollo de software;
* operaciones.

Incluye aspectos como:

* versionado;
* automatización;
* despliegue;
* monitoreo;
* reentrenamiento.

Nuestro descriptor no exige profundizar en MLOps, pero integración continua se entiende mejor dentro de este contexto.

---

# 53. Ciclo de vida en producción

Podemos representar:

**Datos**

↓

**Entrenamiento**

↓

**Evaluación**

↓

**Modelo**

↓

**Despliegue**

↓

**Monitoreo**

↓

**Nuevos datos**

↓

**Reentrenamiento**

↓

**Nueva versión**

Este ciclo puede repetirse múltiples veces.

Por tanto:

**el modelo no es un producto estático.**

---

# 54. Estrategia para el proyecto integrador

Cada dupla debería poder definir:

### Entorno

**Local o cloud**

### Justificación

¿Por qué?

### Modelo

¿Qué versión?

### Runtime

¿Cómo se ejecuta?

### Aplicación

¿Cómo recibe imágenes?

### Optimización

¿Qué técnica se utilizó?

### Pruebas

¿Cómo se validó?

### Actualización

¿Cómo podría reemplazarse el modelo?

Esto forma un plan básico de implementación.

---

# 55. Ejemplo de despliegue local

Proyecto:

**Clasificación de residuos**

Arquitectura:

```text
Notebook del usuario
↓
Streamlit
↓
preprocessing.py
↓
modelo_quantized.onnx
↓
ONNX Runtime
↓
Resultado
```

Características:

* funciona offline;
* las imágenes permanecen localmente;
* no requiere infraestructura cloud.

Esta sería una implementación válida.

---

# 56. Ejemplo de despliegue cloud

Alternativa:

```text
Usuario
↓
Aplicación web
↓
API
↓
Servidor cloud
↓
modelo.onnx
↓
Respuesta
```

Características:

* acceso desde diferentes dispositivos;
* modelo centralizado;
* actualizaciones más sencillas;
* requiere conectividad.

También sería válida.

---

# 57. Ejemplo Edge

Supongamos una estación automática de reciclaje.

```text
Cámara
↓
Mini PC / dispositivo Edge
↓
CNN cuantificada
↓
Clasificación
↓
Mecanismo físico
```

La decisión debe ocurrir:

**inmediatamente**

sin depender necesariamente de Internet.

Aquí Edge Computing resulta especialmente apropiado.

---

# 58. Comparación de escenarios

| Criterio        | Local             | Cloud                | Edge                 |
| --------------- | ----------------- | -------------------- | -------------------- |
| Internet        | No necesariamente | Generalmente sí      | No necesariamente    |
| Privacidad      | Alta              | Depende del servicio | Alta                 |
| Recursos        | Equipo local      | Altos/escalables     | Limitados            |
| Actualización   | Manual            | Centralizada         | Puede ser compleja   |
| Latencia de red | Nula              | Existe               | Nula o mínima        |
| Caso típico     | PC                | Servicio web         | Dispositivo dedicado |

Esta tabla ayuda a justificar decisiones de arquitectura.

---

# 59. Integración continua mínima para el proyecto

No es necesario construir una plataforma MLOps completa.

Una propuesta mínima podría ser:

```text
Repositorio Git
↓
Nueva versión
↓
Ejecutar pruebas
↓
Verificar modelo
↓
Verificar aplicación
↓
Aprobar versión
```

El estudiante debería comprender:

**qué debería automatizarse**

aunque la implementación técnica completa de CI no sea obligatoria.

---

# 60. Posibles pruebas de CI

Por ejemplo:

```text
✓ aplicación inicia
```

```text
✓ modelo carga
```

```text
✓ input = 224 × 224 × 3
```

```text
✓ output = 4 clases
```

```text
✓ inferencia de imagen de prueba funciona
```

```text
✓ latencia < límite definido
```

```text
✓ accuracy mínima cumplida
```

Esto demuestra cómo la integración continua puede proteger el producto.

---

# 61. Caso conductor: actualización del clasificador

Tenemos:

```text
modelo_v1
Accuracy = 89%
Latencia = 120 ms
```

Después entrenamos:

```text
modelo_v2
Accuracy = 91%
Latencia = 430 ms
```

¿Debemos reemplazar automáticamente V1?

No necesariamente.

Si el requisito era:

```text
latencia < 200 ms
```

V2 no cumple.

Esto demuestra que una nueva versión debe evaluarse contra:

**criterios técnicos y funcionales.**

---

# 62. Rollback

Si una nueva versión falla en producción, deberíamos poder volver a una anterior.

Este proceso recibe el nombre de:

**rollback**

Conceptualmente:

```text
Modelo V1
↓
Desplegar V2
↓
Problema
↓
Rollback
↓
Volver V1
```

Esto requiere conservar versiones anteriores.

El versionado es, por tanto, una medida operativa, no únicamente documental.

---

# 63. Integración continua y reproducibilidad

Un pipeline automatizado ayuda a asegurar que:

**cada versión atraviese el mismo proceso.**

Por ejemplo:

```text
instalación
↓
pruebas
↓
evaluación
↓
aprobación
```

Esto reduce decisiones improvisadas.

La misma lógica de experimentación reproducible estudiada en UA3 se extiende ahora al despliegue.

---

# 64. Decisiones técnicas basadas en contexto

No existe una arquitectura universalmente correcta.

Por ejemplo:

### Aplicación educativa en notebook

**Local**

puede ser suficiente.

### Plataforma para miles de usuarios

**Cloud**

puede resultar apropiado.

### Cámara industrial sin conectividad

**Edge**

puede ser necesario.

Por tanto:

**la arquitectura debe responder al problema completo y no solamente al modelo.**

---

# 65. Preguntas para discusión en clase

### Caso 1

Una aplicación debe funcionar en una zona sin conectividad estable.

**Pregunta:** ¿Cloud parece la alternativa más apropiada?

### Caso 2

Una clínica no quiere que determinadas imágenes salgan de su infraestructura.

**Pregunta:** ¿Qué ventaja puede ofrecer una implementación local o Edge?

### Caso 3

Una aplicación recibe 50.000 solicitudes por minuto.

**Pregunta:** ¿Qué ventaja puede ofrecer una infraestructura cloud escalable?

### Caso 4

El modelo tarda 100 ms, pero la respuesta completa tarda 2 segundos debido a transferencia de archivos.

**Pregunta:** ¿La latencia del modelo explica por sí sola la experiencia del usuario?

### Caso 5

Una nueva versión del modelo mejora accuracy, pero rompe el formato de salida esperado por la aplicación.

**Pregunta:** ¿Qué tipo de prueba debería detectar este problema?

### Caso 6

Un equipo reemplaza directamente el modelo en producción sin conservar la versión anterior.

**Pregunta:** ¿Qué problema aparece si la nueva versión falla?

### Caso 7

Una cámara industrial debe clasificar imágenes en tiempo real y no puede depender de Internet.

**Pregunta:** ¿Qué enfoque parece especialmente adecuado?

### Caso 8

Un modelo desplegado comienza a recibir imágenes muy diferentes a las observadas durante entrenamiento.

**Pregunta:** ¿Qué fenómeno puede estar ocurriendo?

---

# 66. Síntesis de la Semana 17

Al finalizar esta sesión deben quedar instaladas diez ideas fundamentales:

1. **Desplegar significa trasladar una solución desde desarrollo hacia un entorno donde puede ser utilizada.**
2. **El despliegue puede realizarse localmente, en cloud o mediante Edge Computing.**
3. **La implementación local favorece simplicidad, privacidad y funcionamiento offline, pero exige recursos y mantenimiento en el dispositivo.**
4. **Cloud permite centralización, escalabilidad y actualización, pero incorpora costos, conectividad, latencia de red y consideraciones de privacidad.**
5. **Edge Computing ejecuta el modelo cerca de donde se generan los datos y resulta especialmente útil cuando se requiere baja latencia, privacidad u operación offline.**
6. **Una API permite separar la aplicación cliente del servicio de inferencia.**
7. **Integración Continua automatiza validaciones para detectar problemas antes de incorporar una nueva versión.**
8. **Código, modelo, dependencias y contrato de entrada/salida deben versionarse y probarse.**
9. **Un modelo desplegado requiere monitoreo y puede necesitar actualización cuando cambian los datos o las condiciones de uso.**
10. **La decisión de implementación debe equilibrar precisión, velocidad, recursos, privacidad, conectividad, costos y mantenibilidad.**

### Hacia la Semana 18

Con esta sesión se completan los principales contenidos técnicos de la asignatura:

**Semana 14:** optimización.

**Semana 15:** interoperabilidad mediante ONNX.

**Semana 16:** integración con una aplicación.

**Semana 17:** despliegue, cloud, Edge e integración continua.

