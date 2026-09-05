# Diccionario de datos
## Encuesta Nacional de Participación y Opinión Ciudadana 2026

### Descripción general

Conjunto de datos ficticio elaborado con fines académicos para representar los resultados de una encuesta nacional sobre participación, opinión ciudadana y hábitos de información.

Cada registro corresponde a una persona encuestada. Los datos no representan personas reales ni resultados oficiales de una encuesta existente.

---

## Variables

| Variable | Tipo de dato | Descripción | Valores / unidad esperada |
|---|---|---|---|
| `id_encuestado` | Entero | Identificador único asignado a cada persona encuestada. | Número entero positivo. |
| `edad` | Numérica discreta | Edad de la persona encuestada al momento de responder la encuesta. | Años cumplidos. Personas adultas. |
| `region` | Categórica nominal | Región de residencia de la persona encuestada. | Nombre de una región de Chile. |
| `zona` | Categórica nominal | Tipo de zona en la que reside la persona encuestada. | `Urbana`, `Rural`. |
| `medio_informacion` | Categórica nominal | Principal medio utilizado por la persona para informarse sobre asuntos públicos y políticos. | `Televisión`, `Radio`, `Prensa digital`, `Redes sociales`. |
| `interes_politica` | Numérica ordinal | Nivel declarado de interés por temas políticos y asuntos públicos. | Escala de 1 a 10, donde 1 representa muy bajo interés y 10 muy alto interés. |
| `confianza_instituciones` | Numérica | Nivel general de confianza declarado hacia las instituciones públicas. | Escala de 0 a 100, donde 0 representa ninguna confianza y 100 máxima confianza. |
| `ingreso_hogar` | Numérica continua | Ingreso mensual aproximado total del hogar de la persona encuestada. | Pesos chilenos (CLP). |
| `participacion_anterior` | Categórica nominal | Indica si la persona declara haber participado en la elección nacional anterior. | `Sí`, `No`. |

---

## Consideraciones de uso

- `id_encuestado` corresponde exclusivamente a un identificador y no representa una característica cuantitativa de la persona.
- Las variables categóricas representan categorías sin una magnitud numérica asociada, salvo que se indique explícitamente lo contrario.
- Las escalas de `interes_politica` y `confianza_instituciones` deben interpretarse de acuerdo con los rangos señalados en este diccionario.
- `ingreso_hogar` se expresa en pesos chilenos y puede presentar una escala considerablemente mayor que otras variables numéricas.
- Antes de utilizar el conjunto de datos en un proceso de Machine Learning, corresponde revisar su calidad, consistencia y preparación.

---

**Nota:** Este conjunto de datos es completamente ficticio y ha sido creado exclusivamente con fines pedagógicos.
