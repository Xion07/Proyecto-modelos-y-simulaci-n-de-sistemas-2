# BlackLegend: predicción del repago de préstamos

Proyecto de aula del curso **Modelos y Simulación de Sistemas II**, Departamento de Ingeniería de Sistemas, Universidad de Antioquia.

**Integrantes:** Bryan David Molina, David Julian Penagos y Xiomara Giraldo Pérez

El objetivo del proyecto es predecir si un solicitante pagará su préstamo en su totalidad (`loan_paid_back`) a partir de su información demográfica, financiera y crediticia y de las condiciones del préstamo. Es un problema de aprendizaje supervisado de clasificación binaria.

Este repositorio contiene el reporte y el notebook del **Entregable I**: descripción del problema, análisis exploratorio de los datos y estado del arte.

---

## Contenido del repositorio

```
.
├── README.md                    # este archivo
├── Reporte_Entregable_I.pdf     # reporte en formato IEEE
├── EDA_BlackLegend.ipynb        # notebook con el análisis exploratorio
├── loan_dataset_20000.csv       # base de datos (Kaggle)
```

El notebook se guardó con sus salidas, así que se puede revisar directamente en GitHub sin necesidad de ejecutarlo.

## Base de datos

|          |                                                                                                         |
| -------- | ------------------------------------------------------------------------------------------------------- |
| Nombre   | Loan Prediction Dataset 2025                                                                            |
| Fuente   | [Kaggle, usuario nabihazahid](https://www.kaggle.com/datasets/nabihazahid/loan-prediction-dataset-2025) |
| Licencia | Apache 2.0                                                                                              |
| Archivo  | `loan_dataset_20000.csv` (2,6 MB)                                                                       |
| Tamaño   | 20.000 registros y 22 columnas (21 predictoras y la variable objetivo)                                  |
| Tipo     | Sintético, sin valores faltantes                                                                        |

El archivo ya está incluido en el repositorio. Si por alguna razón no aparece, se puede descargar desde el enlace de Kaggle y guardarlo en la carpeta principal del repositorio, **con el mismo nombre** y al lado del notebook.

## Cómo ejecutar el notebook


### En Google Colab (sin instalar nada)

1. Abrir el notebook en Colab con este enlace (cambiando `<usuario>` y `<repositorio>`; el repositorio debe ser público):
   `https://colab.research.google.com/github/<usuario>/<repositorio>/blob/main/EDA_BlackLegend.ipynb`
2. Insertar una celda al inicio del notebook y ejecutarla para traer el repositorio con el archivo CSV:

   ```python
   !git clone https://github.com/<usuario>/<repositorio>.git
   %cd <repositorio>
   ```

   Otra opción es subir `loan_dataset_20000.csv` manualmente desde el panel **Archivos** (ícono de carpeta a la izquierda).

3. Ejecutar todo desde **Entorno de ejecución → Ejecutar todas**.

Colab ya trae instaladas todas las librerías necesarias. Las figuras quedan en la carpeta `<repositorio>/figs/` del panel **Archivos**, pero se borran al cerrar la sesión, así que hay que descargarlas si se necesitan.

## Qué hace el notebook

Cada sección del notebook corresponde a una parte de la Sección I del reporte:

| Sección del notebook                      | Qué se calcula                                           | Dónde aparece en el reporte |
| ----------------------------------------- | -------------------------------------------------------- | --------------------------- |
| 1. Carga y vista general                  | Dimensiones, tipos de variables y valores únicos         | I-B                         |
| 2. Calidad de los datos                   | Faltantes, duplicados y redundancia de `monthly_income`  | I-B                         |
| 3. Balance de clases                      | Distribución de `loan_paid_back`                         | Fig. 1                      |
| 4. Variables numéricas                    | Estadísticos descriptivos y asimetría                    | Tabla I                     |
| 5. Correlación con la variable objetivo   | Correlación de Pearson de cada variable numérica         | Fig. 2                      |
| 6. Variables categóricas                  | Tasa de pago por categoría                               | Fig. 3                      |
| 7. `grade_subgrade` y fuga de información | Tasa de pago por grado y correlación con `interest_rate` | Fig. 4                      |
| 8. Codificación propuesta                 | Número de variables después de One-Hot Encoding          | I-B                         |
| 9. Resumen de hallazgos                   | Conclusiones del análisis                                | I-B                         |

## Resultados esperados

Al ejecutar el notebook se deben obtener los mismos valores que aparecen en el reporte, entre ellos:

- 20.000 registros, 22 columnas, 0 valores faltantes y 0 filas duplicadas.
- 15.998 préstamos pagados (80 %) y 4.002 incumplidos (20 %).
- Correlación de Spearman entre `grade_subgrade` e `interest_rate` de 0,51.
- 37 variables de entrada después de la codificación (escenario A).

El notebook no usa procesos aleatorios, así que los resultados son iguales en cada ejecución.

Además, en la carpeta `figs/` se generan estas imágenes (se sobrescriben cada vez que se ejecuta):

| Archivo                                                                     | Contenido                                                |
| --------------------------------------------------------------------------- | -------------------------------------------------------- |
| `fig1_balance.png`                                                          | Fig. 1 del reporte: balance de clases                    |
| `fig2_correlacion.png`                                                      | Fig. 2 del reporte: correlación con la variable objetivo |
| `fig3_categoricas.png`                                                      | Fig. 3 del reporte: tasa de pago por categoría           |
| `fig4_grade.png`                                                            | Fig. 4 del reporte: tasa de pago por `grade_subgrade`    |
| `extra_histogramas.png`, `extra_heatmap.png`, `extra_interes_por_grade.png` | Figuras complementarias que no están en el reporte       |

## Versiones probadas

El notebook se ejecutó sin errores con estas combinaciones:

| Python | pandas | numpy  | matplotlib | seaborn | scikit-learn |
| ------ | ------ | ------ | ---------- | ------- | ------------ |
| 3.10   | 2.0.3  | 1.24.4 | 3.7.5      | 0.12.2  | 1.2.2        |
| 3.11   | 3.0.2  | 2.4.4  | 3.10.9     | 0.13.2  | 1.8.0        |
| 3.12   | 2.2.2  | 2.0.2  | 3.10.0     | 0.13.2  | 1.6.1        |
| 3.13   | 3.0.6  | 2.5.3  | 3.11.2     | 0.13.2  | 1.9.1        |

Con versiones muy recientes de matplotlib las figuras pueden verse con pequeñas diferencias de dibujo, pero las cifras no cambian.

## Problemas comunes

- **`FileNotFoundError: loan_dataset_20000.csv`**: el CSV no está en la misma carpeta que el notebook o tiene otro nombre. Hay que moverlo al lado de `EDA_BlackLegend.ipynb` o cambiar la ruta en la primera celda de la sección 1.
- **`ModuleNotFoundError` (por ejemplo, `No module named 'seaborn'`)**: el entorno virtual no está activado o el notebook está usando otro kernel. Activar el entorno, correr de nuevo `pip install -r requirements.txt` y abrir Jupyter desde esa misma terminal.
- **`jupyter: command not found`**: lo mismo que el punto anterior; también se puede abrir con `python -m notebook`.
- **En Windows, PowerShell no deja activar el entorno**: ejecutar una vez `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned` y volver a intentar.

## Próximas entregas

En el Entregable II se agregarán los notebooks de entrenamiento y evaluación de modelos (regresión logística, k-NN, Random Forest o XGBoost, SVM y red neuronal), el análisis de selección de características y la reducción de dimensión con PCA y UMAP.
