# Recursos complementarios para Machine Learning y Deep Learning

Esta carpeta reúne materiales de consulta para **comprender, evaluar e interpretar modelos de Machine Learning y redes neuronales**. Son recursos educativos abiertos: puedes utilizarlos a tu propio ritmo, sin estar inscrito en un curso ni seguir un calendario académico.

## Contenidos disponibles

| Recurso | ¿Para qué sirve? |
|---|---|
| [Guía de métricas de evaluación](Metricas_Evaluacion_Modelos_Machine_Learning.md) | Explica **Accuracy, Precision, Recall, F1-score, AUC-ROC, Loss, MAE, MSE, RMSE y R²**, además de *baseline*, curvas de aprendizaje y criterios para interpretar resultados de clasificación y regresión. |
| [Ilustración de overfitting](overfitting.png) | Ayuda a reconocer señales de **sobreajuste** al comparar el comportamiento del modelo en entrenamiento y validación. |
| [Ilustración de underfitting](underfitting.png) | Ayuda a identificar el **subajuste**, cuando un modelo todavía no consigue aprender adecuadamente los patrones de los datos. |

Las dos imágenes se encuentran incorporadas y explicadas en la [guía de métricas](Metricas_Evaluacion_Modelos_Machine_Learning.md); no es necesario consultarlas de forma aislada.

## Cómo estudiar con estos recursos

1. **Antes de evaluar un modelo:** revisa qué representa cada métrica y por qué su elección depende del problema.
2. **Durante el entrenamiento:** compara las curvas de entrenamiento y validación para reconocer señales de aprendizaje, sobreajuste o subajuste.
3. **Al interpretar los resultados:** contrasta el desempeño con un *baseline*, analiza las diferencias entre clases cuando corresponda y evita considerar una única cifra como prueba suficiente de calidad.
4. **Al experimentar:** formula una hipótesis, modifica de manera controlada el modelo o sus datos y documenta qué cambió en las métricas.

> **Idea central:** evaluar un modelo no consiste en alcanzar un número arbitrario, sino en **interpretar evidencias y justificar decisiones** en relación con los datos, el problema y los errores relevantes.

## Continuar aprendiendo

- [Ruta de notebooks prácticos](../03_Notebooks/README.md): actividades ejecutables en Google Colab, desde exploración de datos hasta redes neuronales y despliegue.
- [Datasets](../02_Datasets/): conjuntos de datos y archivos utilizados en las prácticas.
- [Simuladores interactivos](../06_Simuladores/): recursos visuales para explorar conceptos de redes neuronales y CNN.
- [Portal de recursos educativos abiertos](https://juliopez.github.io/Taller-Fundamentos-Data-Science-Python/): acceso general al material disponible.

Puedes volver a esta carpeta cada vez que necesites interpretar los resultados de un experimento: **no es una etapa obligatoria de una secuencia fija, sino un material de apoyo transversal**.
