# Laboratorio 2. Deep Learning para Series de Tiempo

CC3084 Data Science, Universidad del Valle de Guatemala, Semestre II 2026.

Fernando Hernández - 23645
Fernando Rueda - 23748

Pronóstico con redes LSTM de las series mensuales de visitantes internacionales
a Guatemala construidas en el [Laboratorio 1](https://github.com/Fercho1118/Laboratorio-1-Series-de-Tiempo),
usando **los mismos conjuntos de entrenamiento y prueba** de ese laboratorio
(partición temporal 70/30: entrenamiento 2009-01 a 2021-03, prueba 2021-04 a
2026-06). Además se explora la similitud de las siete series con el algoritmo
catch22.

## Series modeladas con LSTM

De las siete series del Laboratorio 1 se eligieron dos, y para cada una se
entrenaron **dos configuraciones de LSTM con tuneo de hiperparámetros**:

| Serie | Notebook | Configuraciones | Responsable |
|-------|----------|-----------------|-------------|
| Total de visitantes | `notebooks/lstm-serie-total.ipynb` | A: LSTM simple (1 capa) · B: LSTM apilada (2 capas) | Fernando Hernández 23645 |
| Frontera La Aurora | `notebooks/lstm-serie-la-aurora.ipynb` | A: LSTM + Dropout · B: LSTM bidireccional | Fernando Rueda 23748 |

Cada cuaderno cierra con la comparación del mejor LSTM contra el mejor modelo
del Laboratorio 1 para su serie.

## Similitud de las series con catch22

`notebooks/catch22-similitud.ipynb` (Fernando Hernández 23645) extrae las 22
características de catch22 de las siete series completas
(`data/processed/series_mensuales.csv`), construye la matriz series ×
características, la estandariza y la analiza con PCA, clustering jerárquico y
k-means, mapa de calor, correlaciones entre características y mapa de
distancias entre series, con la interpretación de los agrupamientos al final.

`notebooks/lstm-catch22-la-aurora.ipynb` (Fernando Rueda 23748) resuelve el
punto 2.14: construye un LSTM que usa las 22 características de catch22 como
variables adicionales para la serie La Aurora, extraídas sobre cada ventana
deslizante con pycatch22, y lo compara contra el mejor LSTM sin esas
características. El hallazgo es que las características no mejoran el pronóstico,
lo empeoran, por la poca cantidad de datos de entrenamiento y el cambio de
régimen del tramo de prueba.

En ambos casos el tuneo es una rejilla sobre el tamaño de ventana (lookback),
las unidades de la capa LSTM y la tasa de aprendizaje, seleccionando por RMSE
sobre un tramo de validación temporal (el 15% final del entrenamiento, sin
tocar nunca la prueba). El mejor modelo de cada configuración se reentrena y se
evalúa sobre la prueba con predicción a un paso y pronóstico multi-paso
iterativo, contra una línea base naive estacional.

## Estructura del proyecto

```
data/raw/         Base_Migracion_2009-2026jun.xlsx, el crudo original del Lab 1
data/processed/   series_train.csv y series_test.csv (salida de la etapa 6 del Lab 1)
notebooks/        dos cuadernos de LSTM (uno por serie), el de similitud con catch22 y el de LSTM con catch22
reports/figuras/  figuras que exportan los cuadernos (no se versionan)
requirements.txt  dependencias del proyecto
codebook.md       descripción de las series y de la partición
```

## Cómo correrlo

1. Crear el entorno e instalar dependencias (una sola vez). TensorFlow requiere
Python 3.10 a 3.12, no funciona con 3.13 ni 3.14, así que conviene crear el
entorno con una de esas versiones:

```
python3.12 -m venv .venv
source .venv/bin/activate        # macOS / Linux
.venv\Scripts\activate           # Windows
pip install -r requirements.txt
```

2. Abrir y ejecutar los cuadernos:

```
jupyter notebook notebooks/lstm-serie-total.ipynb
jupyter notebook notebooks/lstm-serie-la-aurora.ipynb
jupyter notebook notebooks/catch22-similitud.ipynb
jupyter notebook notebooks/lstm-catch22-la-aurora.ipynb
```

Los cuadernos fijan las semillas de NumPy y TensorFlow para ser reproducibles.
Los datos de `data/processed/` son la salida exacta de la etapa 6 del pipeline
del Laboratorio 1; para regenerarlos desde el crudo, correr ese pipeline.
