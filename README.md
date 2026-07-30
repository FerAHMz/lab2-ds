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
notebooks/        un cuaderno de modelado LSTM por serie
reports/figuras/  figuras que exportan los cuadernos (no se versionan)
requirements.txt  dependencias del proyecto
codebook.md       descripción de las series y de la partición
```

## Cómo correrlo

1. Crear el entorno e instalar dependencias (una sola vez):

```
python3 -m venv .venv
source .venv/bin/activate        # macOS / Linux
.venv\Scripts\activate           # Windows
pip install -r requirements.txt
```

2. Abrir y ejecutar los cuadernos de arriba hacia abajo:

```
jupyter notebook notebooks/lstm-serie-total.ipynb
jupyter notebook notebooks/lstm-serie-la-aurora.ipynb
```

Los cuadernos fijan las semillas de NumPy y TensorFlow para ser reproducibles.
Los datos de `data/processed/` son la salida exacta de la etapa 6 del pipeline
del Laboratorio 1; para regenerarlos desde el crudo, correr ese pipeline.
