# 📘 Dataset: datos_taller_integrador_STEM.csv
**Versión:** 1.0  
**Autor:** Dr. Julio López Nunez (Curso Fundamentos de Data Science con Python, UDLA-2025)  
**Descripción:**  
Este dataset ficticio contiene información académica, demográfica y conductual de 450 estudiantes inscritos en cursos STEM (matemáticas, física y estadística).  
Su finalidad es servir como base para el **Taller Integrador de la Sesión 6**, donde los participantes aplicarán técnicas de preprocesamiento, EDA, visualización y modelado predictivo con MLP.

---

## Objetivos del Dataset
- Permitir el desarrollo de un flujo completo de análisis de datos.
- Incluir casos reales de *dirty data* para trabajo de limpieza.
- Facilitar la creación de modelos predictivos (regresión o clasificación).
- Integrar reflexión ética y análisis de sesgos.

---

## Estructura General del Dataset

- **Registros:** 450 estudiantes  
- **Columnas:** 17 variables  
- **Tipo de datos:** mixtos (numéricos, categóricos, ordinales)
- **Incluye outliers, valores inconsistentes y nulos** para promover un análisis realista.

---

## Diccionario de Datos

| Variable | Tipo | Descripción |
|---------|------|-------------|
| `id_estudiante` | Entero | Identificador único del estudiante. |
| `sexo` | Categórica | Sexo: M, F, *No responde*, X. |
| `edad` | Entero | Edad del estudiante (incluye outliers). |
| `dependencia_colegio` | Categórica | Tipo de colegio: municipal, subvencionado, pagado. Contiene variaciones de formato y nulos. |
| `nota_diagnostico_matematica` | Float | Nota diagnóstica (1.0–7.0) con outliers y nulos. |
| `nivel_algebra` | Entero | Nivel 1–4. Contiene valores inválidos (0, 5). |
| `promedio_lab_fisica` | Float | Nota promedio de laboratorio (nulos incluidos). |
| `quizzes_estadistica` | Float | Resultados en quizzes de estadística (nulos incluidos). |
| `horas_estudio_semanal` | Entero | Horas semanales, incluye valores extremos (40, 50, -2). |
| `asistencia_porcentaje` | Entero | Porcentaje de asistencia, incluye valores fuera del 0–100. |
| `participacion_clases` | Mixto | Escala 1–5, con valores ruidosos (“NA”, 0). |
| `entregas_atrasadas` | Entero | Actividades entregadas tarde (incluye outliers 15 y 20). |
| `uso_plataforma_aprendizaje` | Entero | 0=no, 1=sí. Contiene valores inválidos (2). |
| `asiste_tutorias` | Entero | 0=no, 1=sí. |
| `nota_final` | Float | Nota final del curso (1.0–7.0). |
| `aprueba` | Entero | 1=aprueba, 0=reprueba (según nota_final). |
| `riesgo_desercion` | Entero | 0–1, con ruido introducido para análisis ético. |

---

## Particularidades del Dataset (para limpieza)

Este dataset incluye deliberadamente:

- **Valores faltantes** (en diagnóstico, laboratorio, asistencia, etc.).  
- **Outliers** (horas de estudio, asistencia, edad).  
- **Datos inconsistentes** (“NA”, valores fuera de rango, errores de tipeo).  
- **Variables categóricas con ruido** (municipal/MUNICIPAL).  
- **Relaciones no lineales** para justificar el uso de MLP.  

Los participantes deberán detectar, justificar y corregir estas anomalías.

---

## Posibles Tareas de Análisis

- Limpieza y transformación de los datos.  
- EDA descriptivo y visualizaciones clave.  
- Modelos predictivos:
  - Regresión → predecir `nota_final`
  - Clasificación → predecir `aprueba` o `riesgo_desercion`
- Entrenamiento de una **red neuronal MLP obligatoria**.

---

## Consideraciones Éticas
Este dataset es ficticio y no representa información real de estudiantes.  
Se utiliza exclusivamente con fines académicos para promover análisis crítico y ético.

---

## Archivo
- **Nombre:** `datos_taller_integrador_STEM.csv`  
- **Formato:** CSV con codificación UTF-8  
- **Filas:** 450  
- **Columnas:** 17  

---

## 📝 Licencia
Uso académico y formativo libre dentro del marco del taller.  


