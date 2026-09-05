# Diccionario de datos — `clientes_retail_video_limpieza.csv`

## Descripción

Dataset pedagógico que representa información ficticia de clientes de una empresa de retail.

El conjunto de datos fue construido para actividades de nivelación y preparación de datos en Machine Learning. Contiene deliberadamente algunos problemas de calidad de datos para que puedan ser identificados y tratados durante el desarrollo del notebook correspondiente.

> **Importante:** los datos son ficticios y fueron generados exclusivamente con fines académicos. No representan clientes ni transacciones reales.

## Variables

| Variable | Tipo | Descripción | Valores / Unidad |
|---|---|---|---|
| `id_cliente` | Numérica discreta / Identificador | Identificador asignado a cada cliente. No representa una característica cuantitativa del cliente y no debe interpretarse como variable numérica de análisis. | Número entero |
| `edad` | Numérica | Edad del cliente. | Años |
| `genero` | Categórica nominal | Género informado por el cliente. | `F`, `M`, `No informa` |
| `region` | Categórica nominal | Región asociada al cliente. | `Metropolitana`, `Valparaíso`, `Biobío`, `Coquimbo` |
| `canal_preferido` | Categórica nominal | Canal de compra preferido por el cliente. | Principalmente `Online` y `Tienda` |
| `categoria_frecuente` | Categórica nominal | Categoría de productos comprada con mayor frecuencia por el cliente. | `Tecnología`, `Hogar`, `Vestuario`, `Deportes` |
| `frecuencia_compra` | Numérica discreta | Cantidad de compras asociadas al cliente en el período representado. | Número de compras |
| `monto_total` | Numérica continua | Monto total asociado a las compras del cliente. | Unidad monetaria ficticia |
| `satisfaccion` | Numérica ordinal | Nivel de satisfacción registrado para el cliente. | Escala de `1` a `5` |

## Observaciones sobre calidad de datos

El archivo corresponde a una versión inicial destinada a ejercicios de preparación de datos. Por esta razón, contiene deliberadamente situaciones que deben ser detectadas y tratadas durante el análisis, entre ellas:

- valores faltantes;
- registros duplicados;
- categorías escritas de manera inconsistente;
- valores numéricos fuera de rangos esperados.

Estas situaciones forman parte del diseño pedagógico del dataset y no corresponden a errores accidentales del archivo.

## Archivo asociado

`clientes_retail_video_limpieza.csv`
