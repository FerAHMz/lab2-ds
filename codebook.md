# Codebook

## `data/raw/Base_Migracion_2009-2026jun.xlsx`

El crudo original del Laboratorio 1: ingreso de viajeros internacionales a
Guatemala de enero 2009 a junio 2026, en formato largo (una fila por
combinación de mes, vía, frontera, país, agrupación de residencia y tipo de
viajero). Se incluye sin modificar para que el proyecto sea reproducible de
punta a punta; los CSV de abajo se regeneran corriendo el pipeline del
Laboratorio 1 sobre este archivo.

## `data/processed/series_train.csv` y `data/processed/series_test.csv`

Salida de la etapa 6 (`06_split.py`) del pipeline del
[Laboratorio 1](https://github.com/Fercho1118/Laboratorio-1-Series-de-Tiempo).
Son las siete series mensuales de visitantes (Turista + Excursionista) a
Guatemala, ya particionadas temporalmente. Se copian aquí sin ninguna
modificación para que el Laboratorio 2 use exactamente los mismos conjuntos de
entrenamiento y prueba.

| Columna | Tipo | Descripción |
|---------|------|-------------|
| `fecha` | fecha (YYYY-MM-01) | Mes de la observación |
| `total` | numérica | Total de visitantes del mes |
| `region: América Del Centro` | numérica | Visitantes de América del Centro (`region_dos`) |
| `region: América Del Norte` | numérica | Visitantes de América del Norte (`region_dos`) |
| `region: Europa` | numérica | Visitantes de Europa (`region_dos`) |
| `frontera: La Aurora` | numérica | Visitantes por el aeropuerto La Aurora |
| `frontera: Valle Nuevo` | numérica | Visitantes por la frontera Valle Nuevo |
| `frontera: San Cristóbal` | numérica | Visitantes por la frontera San Cristóbal |

Partición temporal (70% / 30%, corte único para todas las series, nunca
aleatoria):

- `series_train.csv`: 147 meses, 2009-01 a 2021-03.
- `series_test.csv`: 63 meses, 2021-04 a 2026-06.

Los valores no enteros provienen del prorrateo hecho en la limpieza del
Laboratorio 1. Los meses en cero de abril a agosto de 2020 (dentro del
entrenamiento) son ceros reales por el cierre de fronteras de la pandemia.
