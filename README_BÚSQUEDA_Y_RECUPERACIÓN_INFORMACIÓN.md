# Predicción de Deserción Estudiantil (Búsqueda y Recuperación de Información)

## Descripción

Este proyecto aplica tres modelos clásicos de recuperación de información — **Booleano**, **Vectorial** y **Probabilístico** — al problema de predecir la deserción estudiantil universitaria. Cada estudiante es tratado como un "documento", y sus características académicas y financieras como "términos", permitiendo identificar a los estudiantes con mayor riesgo de abandono mediante técnicas de búsqueda y recuperación de información.

## Dataset

- **Fuente:** Archivos Excel institucionales con registros académicos por período (`2024-2.xlsx`, entre otros), alojados en Google Drive.
- **Descripción:** Cada archivo contiene información por período académico (año y semestre). Las columnas principales utilizadas incluyen `StudentID`, `GPA` y `FinancialAid`. Las columnas de identificación personal (`Apellidos y Nombres`, `Documento`, `Teléfono`) son eliminadas antes del procesamiento.
- **Etiqueta objetivo:** `Desercion` — booleano que indica si un estudiante presente en un período no aparece en el período siguiente.

## Tecnologías

- Python 3
- pandas
- numpy
- scikit-learn (`StandardScaler`, `PCA`, `cosine_similarity`, métricas de evaluación)
- gensim (`corpora.Dictionary`, `models.TfidfModel`)
- nltk
- matplotlib
- seaborn

## Implementación

Se implementaron tres modelos de recuperación de información:

**1. Modelo Booleano**  
Cada estudiante es clasificado usando operadores lógicos sobre sus características:
- `Modelo 1 (AND)`: predice deserción si el estudiante tiene GPA bajo **y** problemas financieros.
- `Modelo 2 (OR)`: predice deserción si tiene GPA bajo **o** problemas financieros.

También se incluye un mini-motor booleano de demostración con operadores AND, OR y NOT sobre un corpus de documentos de ejemplo.

**2. Modelo Vectorial (TF-IDF + Similitud Coseno)**  
Las características de cada estudiante se transforman en términos de un vocabulario controlado (`bajo_gpa`, `gpa_normal`, `problemas_financieros`, `sin_problemas_financieros`). Se aplica TF-IDF con Gensim para ponderar la importancia de cada término y se calcula la similitud coseno entre cada estudiante y una consulta que representa el perfil de riesgo. Adicionalmente, se aplica PCA para reducir la dimensionalidad y visualizar los estudiantes en 2D.

**3. Modelo Probabilístico (BM25)**  
Se adapta el algoritmo BM25 (Best Match 25) para puntuar a cada estudiante según su similitud con una consulta de riesgo. Los parámetros utilizados son `k1 = 1.5` y `b = 0.75`. Los estudiantes con un score BM25 superior a la mediana son clasificados como posibles desertores.

## Ejecución

1. Cargar el notebook `TRABAJO_21_4_26.ipynb` en Google Colab.
2. Montar Google Drive y asegurarse de que los archivos Excel estén en la ruta `/content/drive/MyDrive/Estructuras de Datos/TRABAJO_21_4_26/`.
3. Ejecutar las celdas en orden:
   - Celda 1: Carga y preprocesamiento de datos + Modelo Booleano
   - Celda 2: Modelo Vectorial (PCA)
   - Celda 3: Modelo Probabilístico (BM25)
   - Celda 4: Visualizaciones comparativas

```bash
# Si se ejecuta localmente:
pip install pandas numpy scikit-learn gensim nltk matplotlib seaborn openpyxl
jupyter notebook TRABAJO_21_4_26.ipynb
```

## Resultados

- **Modelo Booleano AND:** Alta precisión, bajo recall — solo identifica estudiantes con ambos factores de riesgo presentes simultáneamente.
- **Modelo Booleano OR:** Mayor recall — captura más casos en riesgo al requerir solo uno de los factores.
- **Modelo Vectorial (TF-IDF):** Permite rankear estudiantes por similitud al perfil de riesgo y visualizar agrupaciones mediante PCA.
- **Modelo Probabilístico (BM25):** Pondera la relevancia de cada característica según su frecuencia e importancia relativa en el corpus, ofreciendo un ranking continuo de riesgo de deserción.

Los tres modelos son evaluados con métricas de Accuracy, Precision, Recall y F1-Score cuando hay casos positivos de deserción disponibles.

## Autores

- Andres Zapata Calle
