## 1️⃣ **¿Por qué la red no analiza la imagen completa de una vez?**

Porque sería _carísimo_ computacionalmente y perdería información local.  
Las convoluciones permiten analizar la imagen **por zonas pequeñas**, detectando patrones locales.

> “Porque las CNN aprenden patrones locales primero —bordes, curvas— y luego los combinan.”

---
## 2️⃣ **¿Quién decide cuántos filtros usar?**

**Tú, el diseñador de la red.**  
El algoritmo no elige automáticamente.

> “La arquitectura la diseña el humano; los pesos los aprende la red.”

---
## 3️⃣ **¿Qué es exactamente un filtro?**

Es una pequeña matriz (3×3, 5×5, etc.) cuyos valores la red aprende.

> “Un filtro es un detector de patrones que la red aprende sola.”

---
## 4️⃣ **¿Por qué ReLU funciona mejor que otras activaciones?**

Porque es simple, computacionalmente eficiente y reduce problemas de gradientes.

> “Porque es rápida, estable y funciona muy bien en redes profundas.”

---
## 5️⃣ **¿Pooling es una convolución?**

No.  
Pooling NO tiene pesos y NO usa activación.

> “No, pooling solo resume la información. No aprende nada.”

---
## 6️⃣ **¿Por qué se necesita hacer “flatten” antes de la capa densa?**

Porque las capas densas solo trabajan con vectores, no con mapas espaciales.

> “Flatten convierte las características detectadas en una lista que la red puede clasificar.”

---
## 7️⃣ **¿Las CNN pueden procesar imágenes en escala de grises?**

Sí.  
No necesitan RGB.  
Pueden recibir 1, 3, 4 o muchos canales.

> “Sí, solo necesitan un tensor. El número de canales da igual.”

---
## 8️⃣ **¿Cómo sabe la red qué patrón buscar?**

No lo sabe.  
Empieza con filtros aleatorios y aprende a través del gradiente qué filtros producen menor error.

> “La red inventa los filtros que mejor reducen el error.”

---
## 9️⃣ **¿Por qué el tamaño del kernel suele ser 3×3?**

3×3 es suficientemente pequeño para capturar detalles y suficientemente grande para ser expresivo. Es estándar moderno.

> “3×3 es un buen equilibrio entre detalle y eficiencia.”

---
## 🔟 **¿Qué pasa si aumento o disminuyo el número de filtros?**

- Más filtros → más capacidad, más cómputo, más riesgo de overfitting.
    
- Menos filtros → menos capacidad, más rápido, pero puede aprender menos.
    

> “Más filtros = más capacidad, pero más riesgo; menos filtros = más simple pero menos potente.”

---
## 1️⃣1️⃣ **¿La CNN “ve” la imagen como nosotros?**

No.  
Ve matrices, intensidades, contrastes y patrones matemáticos.

> “No ve objetos: ve patrones. La interpretación es emergente.”

---
## 1️⃣2️⃣ **¿Necesito normalizar las imágenes antes de entrenar?**

Sí, siempre.  
Normalizar ayuda al gradiente y evita problemas numéricos.

> “Sí, siempre normalizamos. Le facilita la vida al algoritmo.”

---
## 1️⃣3️⃣ **¿El modelo aprende solo o tengo que definir todo?**

El modelo aprende:
- pesos
- bias
- filtros

Pero TÚ defines:
- arquitectura
- learning rate
- optimizador
- capas
- tamaño de kernel
- funciones de activación

> “El modelo aprende parámetros; tú defines la estructura.”

---
## 1️⃣4️⃣ **¿Por qué necesitamos tantas convoluciones seguidas?**

Porque las primeras detectan patrones simples y las siguientes combinan esos patrones en estructuras más complejas.

> “Primero detecta bordes, luego formas, luego partes, luego objetos.”

---
## 1️⃣5️⃣ **¿Qué es un mapa de características (feature map)?**

Es la versión filtrada de la imagen, donde se destacan patrones específicos.

> “Es la imagen transformada después de aplicar un filtro.”

---
## 1️⃣6️⃣ **¿Qué diferencia hay entre “stride” y “padding”?**

- **Stride**: cuántos pixeles avanza el filtro.
- **Padding**: agregar bordes para no perder información.

> “Stride es el paso; padding es el borde.”

---
## 1️⃣7️⃣ **¿Por qué las CNN funcionan mejor que un MLP para imágenes?**

Porque respetan la estructura 2D y reutilizan pesos (convoluciones), reduciendo parámetros y aprendiendo patrones espaciales.

> “Porque entienden espacio. Los MLP destruyen la estructura de la imagen.”

---
## 1️⃣8️⃣ **¿Qué es lo que realmente aprende una CNN?**

Aprende filtros que detectan patrones útiles:
- bordes
- curvas
- texturas
- formas
- partes de objetos

No aprende “qué es un gato”, aprende las **características** que distinguen un gato.

> “No aprende objetos: aprende patrones.”