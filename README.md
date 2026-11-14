# Modelado y evaluación de métricas para predicción de Churn

## Descripción

Proyecto de Machine Learning orientado a la predicción de abandono de clientes mediante un modelo de Regresión Logística.

El objetivo es entrenar un modelo de clasificación binaria, evaluar su rendimiento mediante diferentes métricas y analizar la importancia de los falsos negativos en el contexto de la pérdida de clientes.

El desarrollo se realiza mediante un Jupyter Notebook.

## Tecnologías utilizadas

- Python
- Pandas
- Scikit-learn
- Matplotlib
- Jupyter Notebook

## Dataset

Se utiliza un dataset sintético de 40 clientes, almacenado en el archivo datos/clientes.csv.

Las variables utilizadas son:

| Variable | Tipo |
|---|---|
| edad | Numérica |
| antiguedad_meses | Numérica |
| gasto_mensual | Numérica |
| plan | Categórica |
| medio_pago | Categórica |
| churn | Variable objetivo |

La variable objetivo indica si el cliente abandonó el servicio:

- 0: cliente que permanece.
- 1: cliente que abandona.

La columna cliente_id se excluye del entrenamiento porque corresponde únicamente a un identificador.

## Preparación de los datos

Se realiza una división de los datos mediante train_test_split:

| Conjunto | Porcentaje | Registros |
|---|---:|---:|
| Entrenamiento | 80% | 32 |
| Prueba | 20% | 8 |

Se utiliza random_state=42 para garantizar la reproducibilidad y stratify=y para conservar la distribución de clases.

## Preprocesamiento

Se utiliza ColumnTransformer para aplicar diferentes transformaciones según el tipo de variable.

### Variables numéricas

- Imputación de valores faltantes mediante la mediana.
- Escalado mediante StandardScaler.

### Variables categóricas

- Imputación mediante el valor más frecuente.
- Codificación mediante OneHotEncoder.
- Manejo de categorías desconocidas mediante handle_unknown='ignore'.

El preprocesamiento y el modelo se integran en un único Pipeline.

El pipeline se entrena exclusivamente con los datos de entrenamiento para evitar la fuga de datos (data leakage).

## Modelo utilizado

Se utiliza LogisticRegression como modelo de clasificación binaria.

Configuración:

- max_iter=1000
- random_state=42

El modelo se entrena utilizando los datos de entrenamiento y se evalúa sobre el conjunto de prueba.

## Métricas de evaluación

Se calculan las siguientes métricas:

- Accuracy: proporción total de predicciones correctas.
- Precision: proporción de verdaderos positivos entre las predicciones positivas.
- Recall: proporción de positivos reales detectados correctamente.
- F1-Score: media armónica entre Precision y Recall.
- Matriz de Confusión: distribución de aciertos y errores del modelo.

Se considera Churn = 1 como la clase positiva.

## Resultados obtenidos

| Métrica | Resultado |
|---|---:|
| Accuracy | 0.8750 |
| Precision | 0.8000 |
| Recall | 1.0000 |
| F1-Score | 0.8889 |

El modelo clasificó correctamente 7 de los 8 registros del conjunto de prueba.

### Reporte de clasificación

| Clase | Precision | Recall | F1-Score | Support |
|---|---:|---:|---:|---:|
| 0 | 1.00 | 0.75 | 0.86 | 4 |
| 1 | 0.80 | 1.00 | 0.89 | 4 |
| Macro avg | 0.90 | 0.88 | 0.87 | 8 |
| Weighted avg | 0.90 | 0.88 | 0.87 | 8 |

### Matriz de confusión

| Clase real / predicha | No Churn (0) | Churn (1) |
|---|---:|---:|
| No Churn (0) | 3 | 1 |
| Churn (1) | 0 | 4 |

Interpretación:

- Verdaderos negativos: 3.
- Falsos positivos: 1.
- Falsos negativos: 0.
- Verdaderos positivos: 4.

El modelo identificó correctamente todos los clientes que abandonaron el servicio dentro del conjunto de prueba.

## Interpretación de las métricas

En el contexto de Churn, se considera prioritario el Recall de la clase positiva cuando el costo de perder un cliente supera el costo de realizar una acción de retención innecesaria.

Un falso negativo representa un cliente que abandonará el servicio, pero que no es detectado por el modelo.

Esto puede generar:

- Pérdida de ingresos futuros.
- Pérdida de oportunidades de retención.
- Incremento de los costos de adquisición de nuevos clientes.

Por otro lado, los falsos positivos pueden generar gastos innecesarios en promociones o descuentos.

En este proyecto se obtuvo un Recall del 100%, sin falsos negativos en el conjunto de prueba.

F1-Score también resulta relevante porque permite evaluar el equilibrio entre Precision y Recall.

La elección definitiva de las métricas debe considerar los costos comerciales de cada tipo de error.

## Visualizaciones

El notebook incluye:

- Gráfico de barras con las métricas de evaluación.
- Visualización de la matriz de confusión.

Estas visualizaciones facilitan la interpretación del rendimiento del modelo.

## Rendimiento

El tiempo de entrenamiento se midió mediante time.perf_counter().

| Operación | Tiempo de ejecución (segundos) |
|---|---:|
| Entrenamiento del pipeline | 0.114566 |

Este tiempo incluye el ajuste del preprocesador y del modelo de Regresión Logística.

El resultado corresponde a una ejecución realizada en el equipo de desarrollo y puede variar según el hardware y las condiciones de ejecución.

## Estructura del proyecto

```text
churn-modelado-metricas/
├── datos/
│   └── clientes.csv
├── modelado_metricas.ipynb
├── requirements.txt
├── .gitignore
└── README.md
```

## Instalación

Clonar el repositorio y acceder a la carpeta del proyecto.

Instalar las dependencias:

```bash
pip install -r requirements.txt
```

## Ejecución

### Opción 1: Visual Studio Code

1. Abrir la carpeta del proyecto.
2. Instalar las extensiones Python y Jupyter.
3. Abrir modelado_metricas.ipynb.
4. Seleccionar el entorno de Python correspondiente.
5. Ejecutar todas las celdas mediante Run All.

### Opción 2: Jupyter Notebook

Desde la carpeta raíz del proyecto, ejecutar:

```bash
jupyter notebook
```

Abrir el archivo modelado_metricas.ipynb y ejecutar todas las celdas en orden.

## Limitaciones

El dataset utilizado es sintético y contiene únicamente 40 clientes.

El conjunto de prueba está compuesto por ocho registros, por lo que las métricas obtenidas no permiten garantizar el rendimiento del modelo en escenarios reales.

Como trabajo futuro, se propone utilizar un dataset de mayor tamaño, aplicar validación cruzada y comparar diferentes modelos de clasificación.

## Conclusión

El proyecto demuestra cómo entrenar y evaluar un modelo de clasificación mediante diferentes métricas de Machine Learning.

La integración del preprocesamiento y el estimador en un Pipeline permite evitar la fuga de datos y garantizar la consistencia de las transformaciones.

El análisis de Recall y de la matriz de confusión permite comprender la importancia de los falsos negativos en los problemas de predicción de abandono de clientes.
