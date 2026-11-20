---
layout: default
---

## Introducción y Relevancia del Problema

La elección del dataset de la encuesta CASEN 2024 radica en que permite estudiar las condiciones de vida en los hogares en Chile, a escala nacional, y observar desigualdades que no siempre se pueden ver en estudios generales. Contiene información sobre pobreza multidimensional, vivienda, composición del hogar y características demográficas. Su análisis es relevante para las políticas públicas, en especial para orientar recursos y evaluar necesidades sociales.
Aún así, los datos no nos dicen todo. Aspectos como las decisiones que se realizan para manejar estos datos, como qué variables utilizar, cómo representar a distintos grupos, qué modelos construir y qué información publicar pueden beneficiar o perjudicar a distintas personas.
Por esto, abordamos la encuesta CASEN 2024 desde una perspectiva de activismo de datos, usando la evidencia para hacer visibles las desigualdades que pueden haber y realizar un tratamiento responsable de la información.

## Desarrollo de los talleres:

### Taller 1: ¿A quiénes representan los datos?

Exploramos las variables del dataset y analizamos la representación de distintos grupos, considerando características como pertenencia indígena, orientación sexual e identidad de género. Logramos observar desbalance entre categorías y discutimos sus posibles consecuencias. Entre ellas, encontramos que cuando algunos grupos aparecen con poca frecuencia, los análisis y modelos pueden describir peor sus realidades. También identificamos la importancia de revisar los datos y sus categorías para poder sacar conclusiones.

### Taller 2: ¿Un modelo preciso es siempre justo?

Entrenamos un árbol de decisión para predecir la pobreza multidimensional y evaluamos su desempeño con métricas de clasificación y equidad. Al comparar grupos en cuanto a variables relacionadas con discapacidad, identificamos diferencias importantes en los errores del modelo. Además, los falsos negativos fueron superiores en personas pobres siendo clasificadas como no pobres.
Por otro lado, utilizamos una técnica de mitigación de sesgos. En nuestro análisis, las diferencias entre grupos disminuyeron, pero también disminuyó levemente la exactitud general. Esto demuestra que evaluar solamente el accuracy no es suficiente cuando las predicciones podrían influir en el acceso a ayudas sociales.

### Taller 3: ¿Podemos explicar las decisiones de la IA?

Utilizamos LIME para examinar como un modelo de lenguaje clasificaba comentarios hipotéticos sobre beneficios sociales. Encontramos casos en que expresiones de agradecimiento acompañadas de críticas eran clasificadas como neutras, lo que podría llevar a que problemas relevantes de las experiencias de las personas no sean escuchados.
Además, aplicamos SHAP a un modelo de random forest relacionado con pobreza multidimensional. Esto nos permitió entender qué variables influían en sus predicciones. Explicaciones como estas ayudan a auditar modelos, pero aún así no reemplazan la revisión humana, ni aseguran relaciones causales.

### Taller 4: ¿Cómo proteger la privacidad sin perder utilidad en la información?

Revisamos la encuesta desde un enfoque de derechos humanos y la regulación chilena sobre protección de datos. Luego generamos datos sintéticos con DataSynthesizer, usando siete variables: pobreza multidimensional, sexo, edad, integrantes del hogar, área, pertenencia indígena y hacinamiento. Elegimos el modo correlated_attribute para intentar conservar algunas distribuciones y relaciones estadísticas.
Al comparar los datos originales con los sintéticos mediante ModelInspector, pudimos observar distribuciones bastante similares en variablesc como sexo, área y pertenencia indígena. Además, se mantuvieron algunas relaciones, como la de índice de hacinamiento con pobreza multidimensional. Aún así, hubieron diferencias en categorías menos frecuentes y algunas variaciones en la proporción de pobreza.

## Visualizaciones Representativas de los Talleres

Las siguientes cuatro visualizaciones provienen directamente de los talleres desarrollados durante el semestre y se enlazan en un mismo hilo conductor en torno a la pobreza multidimensional y la protección de grupos vulnerables en CASEN 2024.

---

### 1. Representación y desbalance en variables sensibles (Taller 1)

![Distribución de la población por orientación sexual y etnia](./assets/taller1_representacion.png)

- **Figura 1:** _Distribución de la población por orientación sexual y pertenencia a pueblo indígena (`r3` × `os1`)._
- **Origen:** Taller 1 — Análisis y exploración de datos.

Evidencia un fuerte desbalance entre categorías (83,98% heterosexual no indígena frente a grupos minoritarios bajo el 0,1%). Este desbalance abre la pregunta si respecto a si entrenamos un modelo para predecir pobreza multidimensional sobre estos datos, ¿sus errores afectan por igual a todos los grupos?

### 2. Brechas de error y equidad al predecir pobreza multidimensional (Taller 2)

![False Negative Rate y False Discovery Rate por grupo](./assets/taller2_equidad_fnr_fdr.png)

- **Figura 2:** _Comparación de False Negative Rate (FNR) y False Discovery Rate (FDR) entre grupos privilegiados y no privilegiados (`r3`, `h7e`, `h7f`)._
- **Origen:** Taller 2 — Sesgos, justicia y discriminación (Aequitas).

Revela diferencias marcadas en los errores del árbol de decisión: en variables de discapacidad (`h7e` y `h7f`), la tasa de falsos negativos es notablemente mayor en el grupo no privilegiado (personas en situación de pobreza clasificadas erróneamente como no pobres). Al constatar estas disparidades, surge la necesidad de entender qué variables están guiando realmente las decisiones del modelo.

### 3. Explicabilidad de las variables que impulsan la predicción (Taller 3)

![Explicabilidad SHAP beeswarm del Random Forest para pobreza multidimensional](./assets/taller3_shap_pobreza.png)

- **Figura 3:** _Valores SHAP (beeswarm) del modelo Random Forest para la predicción de pobreza multidimensional._
- **Origen:** Taller 3 — Interpretabilidad y explicabilidad (SHAP).

Permite auditar qué atributos pesan más y en qué dirección influyen en la predicción de pobreza, destacando variables estructurales del hogar como el número de integrantes (`tot_per_h`) y la edad (`edad`). Identificar qué variables concentran la señal socioeconómica nos lleva al desafío de cómo es posible compartir estos datos valiosos para política pública sin exponer la identidad de las personas?

### 4. Utilidad estadística y resguardo de la privacidad (Taller 4)

![Comparación de información mutua entre datos originales y sintéticos](./assets/taller4_privacidad_mutual_info.png)

- **Figura 4:** _Comparación de información mutua por pares entre los datos originales (Private) y los datos sintéticos generados con DataSynthesizer (Synthetic)._
- **Origen:** Taller 4 — Enfoque de derechos humanos y privacidad diferencial (`ModelInspector`).

Retoma variables centrales de los talleres previos (`r3`, `pobreza_multi`, `tot_per_h`, `edad`, junto a `area`, `sexo` e `ind_hacina`) y muestra que la generación sintética en modo `correlated_attribute` conserva las principales dependencias estadísticas (como hacinamiento y tamaño del hogar con pobreza multidimensional), habilitando el análisis y activismo de datos con menor riesgo de reidentificación.

---

## 3. Metodología: Proceso y Tratamiento de Datos

El trabajo completo se construyó sobre la encuesta CASEN 2024, verificando primero la ausencia de valores nulos y revisando los valores extremos de las variables numéricas mediante el rango intercuartílico (IQR), una medida de dispersión que ayuda a identificar cifras inusualmente altas o bajas. Aunque se detectaron valores estadísticamente extremos (por ejemplo, en gasto eléctrico `v24b` o tamaño del hogar `tot_per_h`), estos correspondían a rangos válidos según el libro de códigos de CASEN, por lo que se conservaron para no distorsionar la realidad socioeconómica observada. A partir de esta base común, cada etapa aplicó un tratamiento específico:

- **Análisis de representación:** Se seleccionaron variables sensibles y demográficas especialmente pertenencia a pueblo indígena (`r3`) y orientación sexual (`os1`) para construir cruces interseccionales calculando frecuencias absolutas y porcentajes respecto del total de la muestra.
- **Modelamiento y auditoría de equidad:** Se definió `pobreza_multi` como variable objetivo 82,4% No pobre y 17,6% Pobre y se dividió el conjunto de forma estratificada en 80% para entrenamiento y 20% para prueba, asegurando que ambos subconjuntos mantuvieran la misma proporción de clases.

  Las 34 variables categóricas se transformaron con `OneHotEncoder` (técnica que convierte cada categoría en una columna independiente de 0 y 1 para evitar que el modelo asuma un orden artificial entre ellas) y las 7 numéricas se ajustaron con `StandardScaler` (método que lleva las variables a una escala comparable para que cifras de distinta magnitud no dominen el cálculo). Con estos ,datos se entrenó un árbol de decisión usando `class_weight="balanced"`, un parámetro que da mayor peso a la clase minoritaria Pobre para compensar el desbalance.

  Luego, las predicciones se auditaron con Aequitas, agrupando en dos categorías Privilegiado y No Privilegiado, tres atributos sensibles: pertenencia indígena (`r3`), dificultad de cuidado personal (`h7e`) y dificultad para comunicarse (`h7f`).

  Finalmente, se evaluaron dos estrategias de mitigación de sesgos: `EqOddsPostprocessing`, que ajusta las predicciones después del entrenamiento para equiparar las tasas de error entre grupos, y `Reweighing`, que repondera el peso de los registros antes de entrenar para equilibrar la influencia de cada grupo.

- **Explicabilidad de predicciones:** Manteniendo la división estratificada 80/20, se entrenó un `RandomForestClassifier`, un modelo que combina múltiples árboles de decisión para lograr predicciones más estables. Para interpretar qué variables impulsaban la predicción de la clase Pobre, se aplicó `shap.TreeExplainer`, un método basado en valores **SHAP** que cuantifica cuánto y en qué dirección influye cada atributo en la decisión del modelo sobre una muestra de 50 observaciones del conjunto de prueba.
- **Privacidad diferencial y datos sintéticos:** Se acotó la base a 7 variables clave que combinan perfil demográfico y condiciones de vida. Con DataSynthesizer se construyó un modelo probabilístico que aprende cómo se relacionan las variables entre sí, aplicando privacidad diferencial. Por último, la fidelidad del conjunto sintético se evaluó con ModelInspector, comparando las distribuciones mediante histogramas y un mapa de calor.

---

## 4. Conclusión Final y Activismo de Datos

### 4.1 Síntesis del Proyecto

- [Resumen de los aprendizajes principales del semestre aplicados al dataset]

### 4.2 Propuesta desde el Activismo de Datos

- [Cómo usar estos hallazgos para cuestionar injusticias, exigir rendición de cuentas al Estado o proteger a las comunidades vulnerables]
- [Llamado a la acción o recomendaciones para el tratamiento ético de estos datos]

---

## 5. Referencias y Marco Teórico

- Centro de Microdatos - Universidad de Chile. (2024). _Manual de trabajo de campo: Encuesta de Caracterización Socioeconómica Nacional (Casen 2024)_.
- Ministerio de Desarrollo Social y Familia. (2024). _Manual para la investigación: Encuesta Casen 2024_.
- Office of the United Nations High Commissioner for Human Rights [OHCHR]. (2018). _A human rights-based approach to data: Leaving no one behind in the 2030 Agenda for Sustainable Development_.
