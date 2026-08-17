# Semana 16 — Integración del modelo con una aplicación

## 1. Propósito de la sesión

Comprender cómo integrar un modelo de Deep Learning previamente entrenado y optimizado dentro de una **aplicación funcional**, construyendo el flujo completo desde la carga de una imagen hasta la presentación de la clase predicha y su nivel de confianza.

Al finalizar la sesión, el estudiante debería ser capaz de:

* Explicar la diferencia entre modelo y aplicación.
* Identificar los componentes de una aplicación de inferencia.
* Comprender el flujo completo de una predicción.
* Cargar y validar una imagen proporcionada por un usuario.
* Aplicar el mismo preprocesamiento utilizado durante entrenamiento.
* Ejecutar inferencia utilizando un modelo entrenado.
* Interpretar las salidas del modelo.
* Convertir probabilidades o *scores* en una clase comprensible.
* Incorporar un nivel de confianza en la salida.
* Manejar entradas inválidas y errores básicos.
* Comprender la importancia de separar lógica de interfaz, preprocesamiento e inferencia.
* Relacionar este flujo con el producto final del proyecto integrador.

---

# 2. El modelo no es el producto final

Hasta la Semana 15 hemos construido:

**Problema**

↓

**Dataset**

↓

**CNN**

↓

**Modelo entrenado**

↓

**Modelo optimizado**

↓

**Modelo interoperable**

Pero un usuario normalmente no interactúa directamente con:

```text
modelo.keras
```

o:

```text
modelo.onnx
```

El usuario necesita una aplicación que le permita realizar una tarea concreta.

Por ejemplo:

**Seleccionar una imagen**

↓

**Presionar “Clasificar”**

↓

**Obtener resultado**

Por tanto:

**Modelo ≠ Aplicación**

El modelo constituye el núcleo predictivo de la aplicación.

---

# 3. ¿Qué componentes necesita la aplicación?

Una aplicación sencilla de clasificación de imágenes puede contener:

### Interfaz de usuario

Permite cargar o seleccionar una imagen.

### Validación de entrada

Comprueba que el archivo sea válido.

### Preprocesamiento

Transforma la imagen al formato esperado por el modelo.

### Motor de inferencia

Ejecuta la CNN.

### Postprocesamiento

Interpreta la salida numérica.

### Presentación del resultado

Muestra:

* clase predicha;
* nivel de confianza;
* eventualmente otras probabilidades.

Conceptualmente:

**Usuario → Aplicación → Modelo → Resultado**

---

# 4. Flujo completo de inferencia

El producto final puede representarse como:

**1. Usuario carga una imagen**

↓

**2. Aplicación valida el archivo**

↓

**3. Imagen se transforma**

↓

**4. Modelo recibe tensor**

↓

**5. Modelo realiza inferencia**

↓

**6. Se obtiene vector de salida**

↓

**7. Se identifica la clase**

↓

**8. Se calcula o interpreta confianza**

↓

**9. Aplicación muestra resultado**

Este flujo constituye la funcionalidad central del proyecto integrador.

---

# 5. Entrada del usuario

La aplicación puede permitir diferentes mecanismos de entrada:

* seleccionar archivo;
* arrastrar imagen;
* tomar fotografía;
* enviar imagen mediante una API.

Para nuestro producto mínimo, basta con:

**usuario selecciona una imagen desde su equipo**

La aplicación debe posteriormente determinar si puede procesarla correctamente.

---

# 6. Validar el archivo

Antes de ejecutar el modelo deberíamos comprobar aspectos como:

* archivo existente;
* formato compatible;
* archivo legible;
* imagen válida.

Por ejemplo, si el usuario selecciona:

```text
documento.pdf
```

pero nuestra aplicación espera:

```text
JPG / JPEG / PNG
```

debemos informarlo.

Una buena aplicación no debería simplemente fallar.

---

# 7. Manejo de errores

Podemos diferenciar:

### Error controlado

```text
Formato no compatible.
Seleccione una imagen JPG o PNG.
```

### Error no controlado

```text
Traceback...
FileNotFoundError...
ValueError...
```

Para el usuario final, la primera respuesta es mucho más adecuada.

Por tanto:

**manejo de errores también forma parte de la implementación.**

---

# 8. El preprocesamiento debe reproducirse

Durante el entrenamiento posiblemente utilizamos:

**RGB**

**224 × 224**

**normalización 0–1**

La aplicación debe repetir exactamente estas operaciones.

Por ejemplo:

**Imagen cargada**

↓

**Convertir RGB**

↓

**Resize 224 × 224**

↓

**Convertir a float32**

↓

**Dividir por 255**

↓

**Agregar batch**

↓

**Modelo**

Si omitimos o modificamos una etapa, la distribución de entrada puede cambiar.

---

# 9. El problema del preprocesamiento inconsistente

Supongamos que el modelo fue entrenado con:

```text
0–1
```

pero la aplicación entrega:

```text
0–255
```

El modelo recibe valores hasta 255 veces mayores que los utilizados durante entrenamiento.

La arquitectura sigue funcionando técnicamente, pero:

**la predicción puede ser incorrecta.**

Esto muestra una regla importante:

**el pipeline de inferencia debe ser coherente con el pipeline de entrenamiento.**

---

# 10. Función de preprocesamiento

Conviene centralizar estas operaciones dentro de una función.

Por ejemplo:

```python
from PIL import Image
import numpy as np

def preparar_imagen(ruta):
    imagen = Image.open(ruta).convert("RGB")
    imagen = imagen.resize((224, 224))

    x = np.array(
        imagen,
        dtype=np.float32
    )

    x = x / 255.0
    x = np.expand_dims(x, axis=0)

    return x
```

Conceptualmente:

**archivo → tensor listo para inferencia**

Esto evita repetir lógica en distintas partes de la aplicación.

---

# 11. Separación de responsabilidades

Una buena estructura puede dividir el sistema en componentes.

### Función 1

```text
cargar_imagen()
```

### Función 2

```text
preprocesar()
```

### Función 3

```text
predecir()
```

### Función 4

```text
interpretar_resultado()
```

### Interfaz

Coordina esas funciones.

Esto mejora:

* claridad;
* mantenibilidad;
* pruebas;
* reutilización.

---

# 12. Cargar un modelo Keras

Si utilizamos el formato original:

```python
from tensorflow import keras

modelo = keras.models.load_model(
    "modelo.keras"
)
```

Después:

```python
resultado = modelo.predict(x)
```

Conceptualmente:

**Tensor de entrada**

↓

**CNN**

↓

**Vector de salida**

No existe entrenamiento en esta etapa.

Estamos ejecutando:

**inferencia.**

---

# 13. Cargar un modelo ONNX

Si utilizamos ONNX:

```python
import onnxruntime as ort

sesion = ort.InferenceSession(
    "modelo.onnx"
)
```

Podemos obtener el nombre de entrada:

```python
input_name = (
    sesion.get_inputs()[0].name
)
```

Después:

```python
resultado = sesion.run(
    None,
    {
        input_name: x
    }
)
```

La lógica funcional sigue siendo la misma.

---

# 14. El resultado del modelo

Supongamos:

```text
[0.04, 0.07, 0.85, 0.04]
```

Este vector todavía no es directamente útil para una persona.

Necesitamos interpretar:

**¿qué representa cada posición?**

Supongamos:

```text
0 → plástico
1 → vidrio
2 → cartón
3 → metal
```

El mayor valor corresponde a:

```text
2
```

Por tanto:

**Cartón**

---

# 15. Obtener la clase predicha

En Python:

```python
indice = np.argmax(resultado)
```

Después:

```python
clases = [
    "plastico",
    "vidrio",
    "carton",
    "metal"
]

clase = clases[indice]
```

Si:

```text
indice = 2
```

obtenemos:

```text
carton
```

La aplicación transforma así una salida matemática en información comprensible.

---

# 16. Obtener confianza

Podemos obtener el valor asociado con la clase seleccionada:

```python
confianza = resultado[indice]
```

Por ejemplo:

```text
0.85
```

Podemos transformarlo en:

```text
85%
```

La salida podría mostrarse:

**Clase: Cartón**

**Confianza: 85%**

Este es exactamente el comportamiento definido para el producto final.

---

# 17. Confianza no significa certeza

Debemos recordar la distinción trabajada anteriormente.

Un resultado:

```text
Cartón — 95%
```

no significa:

**95% de certeza absoluta sobre la realidad.**

Significa que el modelo asignó un valor elevado a esa clase dentro de su distribución de salida.

Un modelo puede estar:

**muy confiado y equivocado.**

Por ello, la aplicación debería evitar representar la salida como una verdad infalible.

---

# 18. Umbral mínimo

Podemos definir un umbral de confianza.

Por ejemplo:

```python
if confianza >= 0.70:
    mostrar_prediccion()
else:
    mostrar_baja_confianza()
```

Resultado posible:

```text
No se puede clasificar
con suficiente confianza.
```

Esto puede resultar más adecuado que mostrar una categoría cuando:

```text
Clase A = 27%
Clase B = 26%
Clase C = 24%
Clase D = 23%
```

---

# 19. ¿Cómo elegir el umbral?

No existe necesariamente un valor universal como:

```text
70%
```

La selección debería basarse en:

* comportamiento del modelo;
* distribución de probabilidades;
* importancia del error;
* contexto del proyecto.

En nuestro caso académico podemos utilizar un umbral simple y justificarlo.

Lo importante es comprender que:

**la interpretación de la salida también forma parte de la lógica de aplicación.**

---

# 20. Mostrar las principales probabilidades

Otra alternativa es mostrar más de una clase.

Por ejemplo:

```text
Cartón: 72%
Plástico: 18%
Metal: 7%
Vidrio: 3%
```

Esto permite observar si la clasificación es:

**muy clara**

o:

**ambigua.**

Puede resultar útil durante las pruebas del proyecto.

---

# 21. Top-K

En problemas con muchas clases puede utilizarse:

**Top-K**

Por ejemplo:

**Top-3**

muestra las tres categorías con mayor puntuación.

Conceptualmente:

```text
1. Labrador → 65%
2. Golden Retriever → 25%
3. Beagle → 4%
```

Para proyectos con pocas clases puede no ser necesario, pero constituye una estrategia habitual.

---

# 22. Interfaz mínima

El producto final no necesita una interfaz extremadamente compleja.

Un mínimo funcional puede incluir:

**Título**

↓

**Botón para cargar imagen**

↓

**Vista previa**

↓

**Botón “Clasificar”**

↓

**Resultado**

```text
Clase: Vidrio
Confianza: 92%
```

Esto es suficiente para demostrar que:

**el modelo fue realmente integrado en una solución funcional.**

---

# 23. Streamlit como alternativa

Una forma sencilla de construir aplicaciones de Machine Learning en Python es utilizar:

**Streamlit**

Por ejemplo:

```python
import streamlit as st

st.title(
    "Clasificador de imágenes"
)

archivo = st.file_uploader(
    "Seleccione una imagen",
    type=["jpg", "jpeg", "png"]
)
```

Streamlit permite crear rápidamente interfaces web sin desarrollar directamente HTML, CSS y JavaScript.

---

# 24. Mostrar imagen en Streamlit

Si el usuario carga una imagen:

```python
if archivo is not None:
    imagen = Image.open(archivo)

    st.image(
        imagen,
        caption="Imagen seleccionada"
    )
```

Esto mejora la interacción porque el usuario puede comprobar qué archivo será clasificado.

Posteriormente utilizaremos esa misma imagen para inferencia.

---

# 25. Integrar el preprocesamiento

Podemos reutilizar:

```python
def preparar_imagen(imagen):
    imagen = imagen.convert("RGB")
    imagen = imagen.resize((224, 224))

    x = np.array(
        imagen,
        dtype=np.float32
    )

    x = x / 255.0
    x = np.expand_dims(
        x,
        axis=0
    )

    return x
```

Así:

**la interfaz no necesita conocer los detalles internos del preprocesamiento.**

---

# 26. Integrar la predicción

Podemos construir:

```python
def predecir(x):
    predicciones = modelo.predict(
        x
    )[0]

    indice = np.argmax(
        predicciones
    )

    clase = clases[indice]
    confianza = predicciones[indice]

    return clase, confianza
```

La función recibe:

**tensor**

y devuelve:

**clase + confianza**

Esto mantiene una estructura clara.

---

# 27. Flujo con Streamlit

Podemos conectar:

```python
if archivo is not None:
    imagen = Image.open(archivo)

    st.image(imagen)

    if st.button("Clasificar"):
        x = preparar_imagen(
            imagen
        )

        clase, confianza = predecir(
            x
        )

        st.write(
            "Clase:",
            clase
        )
```

Después podemos incorporar el porcentaje.

---

# 28. Mostrar confianza

Por ejemplo:

```python
st.write(
    f"Confianza: {confianza:.2%}"
)
```

Si:

```text
confianza = 0.9134
```

la interfaz mostrará:

```text
Confianza: 91.34%
```

Esta transformación facilita la interpretación.

---

# 29. Umbral en la aplicación

Podemos hacer:

```python
if confianza >= 0.70:
    st.success(
        f"{clase} - {confianza:.1%}"
    )
else:
    st.warning(
        "Predicción con baja confianza"
    )
```

Esto convierte una salida numérica en una decisión de presentación.

El umbral debe estar documentado.

---

# 30. Aplicación con ONNX Runtime

Podemos reemplazar la función de predicción.

Por ejemplo:

```python
def predecir_onnx(x):
    resultado = sesion.run(
        None,
        {
            input_name: x
        }
    )[0][0]

    indice = np.argmax(
        resultado
    )

    return (
        clases[indice],
        resultado[indice]
    )
```

La interfaz podría mantenerse prácticamente igual.

Esto demuestra el beneficio de separar:

**interfaz**

de:

**motor de inferencia.**

---

# 31. Arquitectura modular

Podemos estructurar el proyecto:

```text
proyecto/
│
├── app.py
├── model/
│   └── clasificador.onnx
│
├── inference.py
├── preprocessing.py
├── classes.py
└── requirements.txt
```

Cada archivo cumple una función.

### app.py

Interfaz.

### preprocessing.py

Transformación de imágenes.

### inference.py

Carga y ejecución del modelo.

### classes.py

Definición de clases.

Esta organización mejora mantenibilidad.

---

# 32. No cargar el modelo en cada predicción

Una mala práctica sería:

```text
Usuario carga imagen
↓
Cargar modelo desde disco
↓
Predecir
```

y repetir esta operación cada vez.

La carga puede consumir tiempo.

Es preferible:

**iniciar aplicación**

↓

**cargar modelo una vez**

↓

**esperar imágenes**

↓

**ejecutar inferencias**

Esto reduce latencia.

---

# 33. Medir tiempos dentro de la aplicación

Podemos registrar:

```python
import time

inicio = time.perf_counter()

clase, confianza = predecir(x)

fin = time.perf_counter()

latencia = fin - inicio
```

Después:

```text
Tiempo de inferencia:
0,124 segundos
```

Esto permite comparar el desempeño real del modelo integrado.

La métrica obtenida en producción puede ser más relevante que una medición aislada en notebook.

---

# 34. Inferencia y preprocesamiento

También podemos medir por separado:

### Tiempo de preprocesamiento

```text
35 ms
```

### Tiempo del modelo

```text
90 ms
```

### Tiempo total

```text
125 ms
```

Esto ayuda a identificar dónde se encuentra el costo.

Un modelo rápido puede estar dentro de una aplicación lenta debido a otros componentes.

---

# 35. Validación funcional

La aplicación debería probarse con:

### Imagen conocida correctamente clasificada

¿Entrega el resultado esperado?

### Imagen de cada clase

¿Todas las clases funcionan?

### Archivo inválido

¿Muestra mensaje adecuado?

### Imagen muy grande

¿La procesa correctamente?

### Imagen con baja confianza

¿Se maneja correctamente?

Las pruebas deben cubrir más que el caso ideal.

---

# 36. Pruebas fuera del dataset

También es importante utilizar imágenes:

**que no pertenecen exactamente al dataset original.**

Por ejemplo:

* fotografías tomadas por estudiantes;
* imágenes obtenidas bajo condiciones distintas;
* diferentes fondos;
* diferentes cámaras.

Esto permite evaluar el comportamiento del producto en condiciones más cercanas al uso real.

---

# 37. Out-of-distribution

Supongamos que nuestro modelo clasifica:

* plástico;
* vidrio;
* cartón;
* metal.

El usuario carga:

**una fotografía de un perro.**

El modelo igualmente podría responder:

```text
Plástico — 48%
```

porque su salida está diseñada para escoger entre las clases conocidas.

El modelo no necesariamente sabe:

**“esto no pertenece a ninguna clase”.**

Este fenómeno es importante en aplicaciones reales.

---

# 38. Clase desconocida

Una posible estrategia consiste en incorporar:

**umbral de confianza**

Si ninguna clase supera cierto valor:

```text
No reconocido
```

Sin embargo, esta estrategia no resuelve completamente el problema de datos fuera de distribución.

Una solución más robusta puede requerir:

* clase adicional;
* detección de anomalías;
* modelos especializados.

Para nuestro proyecto, basta con reconocer esta limitación.

---

# 39. Vista previa y transparencia

La aplicación puede mostrar:

**Imagen recibida**

junto con:

**resultado**

Esto facilita que el usuario comprenda sobre qué entrada se realizó la inferencia.

Por ejemplo:

```text
[imagen]

Predicción:
Vidrio

Confianza:
92%
```

---

# 40. Metadatos del modelo

La aplicación podría incluir información técnica como:

```text
Modelo: MobileNetV2 Fine-Tuned
Versión: 1.2
Input: 224 × 224
Clases: 4
```

No es obligatorio mostrarlo permanentemente al usuario, pero debería estar documentado.

Esto mejora:

* trazabilidad;
* reproducibilidad;
* soporte.

---

# 41. Versionado de la aplicación y modelo

Es importante distinguir:

```text
Aplicación v1.0
Modelo v2.1
```

Podemos actualizar el modelo sin modificar necesariamente toda la aplicación.

Por ejemplo:

```text
app 1.0 + modelo 1.0
```

después:

```text
app 1.0 + modelo 1.1
```

si el contrato de entrada/salida permanece igual.

Esto prepara la lógica de integración continua que veremos posteriormente.

---

# 42. Contrato del modelo

Recordemos el contrato definido en Semana 15.

Por ejemplo:

### Input

```text
RGB
224 × 224
float32
0–1
```

### Output

```text
4 probabilidades
```

### Clases

```text
0 plástico
1 vidrio
2 cartón
3 metal
```

La aplicación debe respetar este contrato.

Si cambia, debe modificarse la integración.

---

# 43. Cambiar el modelo sin romper la aplicación

Supongamos que reemplazamos:

**CNN propia**

por:

**MobileNetV2**

Pero ambos modelos mantienen:

```text
Input = 224 × 224 × 3
Output = 4 clases
```

La aplicación podría requerir pocos cambios.

Sin embargo, si MobileNet utiliza otro preprocesamiento, debemos actualizar:

**preprocessing.py**

Este ejemplo muestra por qué separar componentes facilita mantenimiento.

---

# 44. API versus aplicación directa

Existen dos formas generales de integrar el modelo.

### Modelo dentro de la aplicación

```text
Aplicación
↓
Modelo local
```

### Modelo detrás de una API

```text
Aplicación
↓
HTTP
↓
Servicio ML
↓
Modelo
```

La primera es más sencilla para nuestro producto mínimo.

La segunda facilita arquitecturas distribuidas y despliegues cloud.

Será retomada en la Semana 17.

---

# 45. Aplicación local

Una solución local puede ejecutar:

**Streamlit + Python + ONNX**

en el mismo computador.

Ventajas:

* sencilla;
* sin dependencia de Internet;
* menor complejidad.

Desventajas:

* requiere instalar dependencias;
* utiliza recursos del dispositivo;
* actualización manual.

Esta alternativa puede ser perfectamente válida para el proyecto integrador.

---

# 46. Aplicación cloud

Otra alternativa es desplegar la aplicación en un servidor.

Conceptualmente:

**Usuario**

↓

**Navegador**

↓

**Servidor**

↓

**Modelo**

↓

**Predicción**

Ventajas:

* acceso remoto;
* entorno centralizado;
* actualización controlada.

Desventajas:

* dependencia de red;
* costos;
* seguridad;
* latencia adicional.

Estas decisiones serán profundizadas en la próxima sesión.

---

# 47. Privacidad de las imágenes

Cuando el usuario entrega una imagen debemos analizar dónde se procesa.

### Inferencia local

La imagen puede permanecer en el dispositivo.

### Inferencia cloud

La imagen debe enviarse hacia un servidor.

Esto introduce aspectos como:

* privacidad;
* almacenamiento;
* seguridad;
* consentimiento.

Aunque nuestro proyecto sea académico, estas decisiones forman parte del diseño profesional de una solución.

---

# 48. No almacenar imágenes innecesariamente

Si la aplicación solamente necesita:

**recibir → clasificar → responder**

no siempre existe razón para guardar permanentemente la fotografía.

Conceptualmente:

**Imagen temporal**

↓

**Inferencia**

↓

**Resultado**

↓

**Eliminar**

Esto puede reducir riesgos de privacidad y almacenamiento.

El diseño debe justificar si los datos se conservan o no.

---

# 49. Registro de predicciones

Sí puede resultar útil registrar ciertos datos operacionales.

Por ejemplo:

```text
fecha
versión modelo
clase predicha
confianza
latencia
```

Esto permite posteriormente:

* analizar rendimiento;
* detectar errores;
* monitorear uso.

Sin embargo, registrar información también exige considerar privacidad y propósito.

---

# 50. Ejemplo de log

Podríamos registrar:

```text
2026-10-14 15:22
modelo_v3
predicción: vidrio
confianza: 0.92
latencia: 0.14 s
```

Este tipo de información será útil si posteriormente necesitamos saber:

**qué versión del modelo produjo una determinada predicción.**

Esto se relaciona con MLOps e integración continua.

---

# 51. Evitar complejidad innecesaria

Una interfaz con:

* autenticación;
* base de datos;
* múltiples dashboards;
* usuarios;
* administración;

puede resultar interesante, pero no constituye el objetivo principal de la asignatura.

La prioridad sigue siendo:

**Machine Learning + Deep Learning + implementación**

Por tanto, una aplicación sencilla pero técnicamente correcta puede ser mejor evidencia que una interfaz compleja con un modelo mal integrado.

---

# 52. Aplicación al proyecto integrador

La etapa actual toma el modelo generado anteriormente:

**CNN definitiva**

y lo transforma en:

**Producto funcional**

La integración debería demostrar:

**Imagen nueva**

↓

**Preprocesamiento correcto**

↓

**Modelo**

↓

**Inferencia**

↓

**Clase**

↓

**Confianza**

Esto corresponde directamente al producto final definido en el syllabus. 

---

# 53. Ejemplo conductor: aplicación para clasificación de residuos

Interfaz:

```text
CLASIFICADOR DE RESIDUOS

[Seleccionar imagen]

[Vista previa]

[Clasificar]
```

Usuario carga:

**botella.jpg**

La aplicación realiza:

```text
RGB
↓
224 × 224
↓
normalización
↓
modelo ONNX
```

Salida:

```text
Predicción:
Vidrio

Confianza:
92,4%

Tiempo:
0,11 s
```

Este flujo representa un producto final completo desde la perspectiva funcional.

---

# 54. Comparación antes y después de la integración

Antes:

```text
Notebook
↓
modelo.predict()
↓
array
```

Después:

```text
Usuario
↓
Interfaz
↓
Imagen
↓
Modelo
↓
Resultado comprensible
```

La diferencia no está en la inteligencia del modelo.

Está en convertir esa capacidad en una **solución utilizable**.

---

# 55. Prueba end-to-end

Una prueba especialmente importante es:

**End-to-End**

o de extremo a extremo.

Conceptualmente:

**Usuario carga imagen real**

↓

**Aplicación**

↓

**Preprocesamiento**

↓

**Modelo**

↓

**Postprocesamiento**

↓

**Resultado**

Si este flujo funciona correctamente, estamos evaluando el sistema completo.

Una prueba aislada de:

```python
modelo.predict()
```

no verifica toda la aplicación.

---

# 56. Fallo del modelo versus fallo de la aplicación

Debemos distinguir:

### Error del modelo

La aplicación funciona pero clasifica incorrectamente.

### Error de integración

La aplicación alimenta incorrectamente el modelo.

### Error de interfaz

El usuario no puede cargar el archivo.

### Error de infraestructura

El modelo no puede cargarse.

Identificar el origen facilita la resolución de problemas.

---

# 57. Matriz simple de pruebas

Podemos documentar:

| Prueba | Entrada        | Resultado esperado |
| ------ | -------------- | ------------------ |
| P1     | JPG válido     | Clasificación      |
| P2     | PNG válido     | Clasificación      |
| P3     | PDF            | Mensaje de error   |
| P4     | Imagen clase A | Clase A            |
| P5     | Imagen clase B | Clase B            |
| P6     | Baja confianza | Advertencia        |

Esto aporta una evidencia clara de validación funcional.

---

# 58. Preguntas para discusión en clase

### Caso 1

Un modelo funciona correctamente en el notebook, pero falla dentro de la aplicación.

**Pregunta:** ¿Podemos concluir inmediatamente que el modelo está incorrecto?

### Caso 2

La CNN fue entrenada con imágenes normalizadas entre 0 y 1, pero la aplicación utiliza 0–255.

**Pregunta:** ¿Qué problema existe?

### Caso 3

El modelo produce:

```text
[0.28, 0.27, 0.24, 0.21]
```

**Pregunta:** ¿Sería razonable presentar la primera clase como una predicción de alta confianza?

### Caso 4

La aplicación carga nuevamente el modelo de 200 MB cada vez que un usuario presiona “Clasificar”.

**Pregunta:** ¿Qué problema de eficiencia existe?

### Caso 5

El modelo predice correctamente el índice 1, pero la interfaz muestra “metal” cuando la clase 1 era “vidrio”.

**Pregunta:** ¿Qué componente falló?

### Caso 6

Una aplicación funciona solo cuando se ejecuta desde el notebook del estudiante.

**Pregunta:** ¿Podemos considerarla completamente implementada como producto funcional?

### Caso 7

Se reemplaza el modelo por una nueva versión que utiliza imágenes 256 × 256.

**Pregunta:** ¿Qué parte de la aplicación probablemente deberá actualizarse?

### Caso 8

Una fotografía no pertenece a ninguna de las clases conocidas, pero el modelo igualmente selecciona una.

**Pregunta:** ¿Qué limitación de los clasificadores cerrados estamos observando?

---

# 59. Síntesis de la Semana 16

Al finalizar esta sesión deben quedar instaladas diez ideas fundamentales:

1. **El modelo constituye el núcleo predictivo, pero no equivale por sí solo a una aplicación funcional.**
2. **Una solución de inferencia requiere entrada, validación, preprocesamiento, modelo, postprocesamiento y presentación de resultados.**
3. **El preprocesamiento de producción debe ser consistente con el utilizado durante el entrenamiento.**
4. **La salida numérica del modelo debe transformarse correctamente en una clase y nivel de confianza comprensibles.**
5. **El orden de las clases constituye parte del contrato de integración y debe conservarse.**
6. **Un umbral de confianza puede ayudar a evitar presentar como concluyentes predicciones demasiado ambiguas.**
7. **Separar interfaz, preprocesamiento e inferencia mejora la mantenibilidad de la aplicación.**
8. **El modelo debería cargarse una vez y reutilizarse para múltiples inferencias, evitando costos innecesarios.**
9. **La solución debe probarse de extremo a extremo, incluyendo entradas válidas, inválidas y condiciones diferentes a las del dataset original.**
10. **El producto final del proyecto integrador debe permitir que un usuario cargue una imagen y obtenga una clasificación funcional mediante la CNN desarrollada durante el curso.**

### Hacia la Semana 17

En la Unidad 4 hemos avanzado desde:

**Semana 14:** optimizar el modelo.

**Semana 15:** convertirlo en un componente interoperable.

**Semana 16:** integrarlo dentro de una aplicación.

Ahora queda resolver:

**¿Dónde ejecutaremos esta solución y cómo mantenemos el modelo cuando comienza a utilizarse en un entorno real?**

La **Semana 17** abordará **despliegue local y cloud, servicios cloud de Machine Learning, Edge Computing e integración continua**, preparando directamente la presentación del producto final.
