# Trabajo final – Deep Learning: Predicción de NMHC (YDL Air Pollution)

Este repositorio contiene el trabajo final del curso **Deep Learning** (Módulo 4–5),
basado en la competición de Kaggle **YDL Air Pollution**.  
El objetivo principal es **predecir la concentración horaria de NMHC(GT)** a partir
de variables de calidad del aire y meteorológicas mediante modelos baseline y
modelos de deep learning (LSTM y GRU).

---

## 1. Video de presentación

👉 **Video en YouTube:** https://youtu.be/5Whr8Q-ZlTc

---

## 2. Datos

Los datos provienen de la competición:

> **YDL Air Pollution** – Kaggle  
> ((https://www.kaggle.com/competitions/ydl-air-pollution/data?select=ridge.csv))

Archivos necesarios:

- `train.csv`  
- `test.csv`  
- `sample.csv`

**Instrucciones:**

1. Descarga los tres archivos desde la página de la competición.
2. Colócalos en la **raíz del repositorio** (mismo nivel que los notebooks).
3. Al ejecutar en Google Colab, asegúrate de que esos archivos estén disponibles en el
   directorio de trabajo (subiéndolos al runtime o montando Google Drive).

---

## 3. Estructura del repositorio

La estructura sigue la recomendación oficial del curso:

- `01_exploracion_datos.ipynb`  
  - Carga de `train.csv`.  
  - Análisis exploratorio: estadísticas descriptivas, distribución de variables, detección
    de valores inválidos (`-200`), comportamiento temporal del NMHC(GT).

- `02_preprocesado.ipynb`  
  - Conversión de `Datetime` a tipo fecha-hora y orden temporal.  
  - Reemplazo de `-200` por `NaN` en gases (`CO(GT)`, `NMHC(GT)`, `NOx(GT)`, `NO2(GT)`).  
  - Eliminación de filas sin valor de `NMHC(GT)`.  
  - Imputación de valores faltantes (mediana) y escalado con `StandardScaler`.  
  - Construcción de:
    - Vista tabular (`X_tab`, `y_tab`) para modelos baseline.
    - Vista secuencial (`X_seq`, `y_seq`) mediante ventanas deslizantes de 24 horas
      para modelos LSTM/GRU (predicción 1 paso adelante).

- `03_modelo_baseline.ipynb`  
  - Entrenamiento de modelos clásicos de regresión sobre la vista tabular:
    - Regresión lineal múltiple.  
    - Random Forest Regressor.  
  - Evaluación con **MAE** y **RMSE** en el conjunto de test interno.  
  - Tabla de métricas `baseline_metrics.csv` en `results/`.

- `04_modelo_deep_learning.ipynb`  
  - Modelos recurrentes sobre la vista secuencial:
    - LSTM (target en escala original y con `log(1 + NMHC(GT))`).  
    - GRU (target con `log(1 + NMHC(GT))`).  
  - Uso de `EarlyStopping` y división temporal en train/val/test.  
  - Cálculo de **MAE** y **RMSE** en escala real (tras deshacer el log).  
  - Gráficas de:
    - Curvas de entrenamiento (loss vs val_loss).  
    - Real vs predicho (scatter).  
    - Serie temporal real vs predicha en el conjunto de test.  
  - El modelo final seleccionado es **GRU_log_target**, que obtiene las mejores
    métricas de MAE y RMSE.  
  - Tabla de métricas `dl_metrics.csv` en `results/`.

- `05_submission_kaggle.ipynb` *(opcional pero recomendado)*  
  - Reentrenamiento del modelo **GRU_log_target** usando todas las ventanas de train.  
  - Aplicación del mismo preprocesado a `test.csv`.  
  - Construcción de ventanas de test y predicción de NMHC(GT).  
  - Creación del archivo `submission_gru_log_target.csv` en `submissions/`
    siguiendo el formato de `sample_submission.csv`.

- `results/`  
  - `baseline_metrics.csv`: métricas de los modelos baseline.  
  - `dl_metrics.csv`: métricas de los modelos LSTM/GRU.

- `submissions/`  
  - `submission_gru_log_target.csv`: archivo listo para subir a Kaggle.

- `INFORME_PROYECTO.PDF`  
  - Informe final (≈5–10 páginas) con:
    - Descripción detallada del problema.  
    - Preprocesado y construcción de ventanas.  
    - Arquitectura de los modelos (baseline y deep learning).  
    - Resultados cuantitativos.  
    - Discusión.

---

## 4. Cómo ejecutar los notebooks en Google Colab

1. Abre el notebook desde GitHub y selecciona **“Open in Colab”**  
   (o sube el `.ipynb` manualmente a Colab).
2. Sube `train.csv`, `test.csv` y `sample_submission.csv` al entorno de Colab  
   o monta Google Drive de forma que queden en el directorio de trabajo.
3. Ejecuta las celdas **en orden** de arriba abajo:
   - Primero `01_exploracion_datos`.  
   - Luego `02_preprocesado`.  
   - Después `03_modelo_baseline`.  
   - Finalmente `04_modelo_deep_learning` (y `05_submission_kaggle` si se usa).
4. Las métricas finales se guardan en la carpeta `results/` y, en su caso,
   el archivo de submission en `submissions/`.

---

## 5. Modelo final y resultados (resumen)

Tras comparar varios modelos:

- Baselines: Regresión lineal y Random Forest.  
- Deep learning: LSTM y GRU sobre ventanas de 24 horas, con y sin
  transformación logarítmica del objetivo.

El modelo que obtiene el mejor compromiso entre MAE y RMSE en el conjunto de test
es una **GRU con target transformado** `log(1 + NMHC(GT))` (`GRU_log_target`),
entrenada con pérdida MSE y evaluada en la escala real de NMHC(GT).

Los detalles numéricos y las gráficas de ajuste se describen en
`INFORME_PROYECTO.PDF` y en el notebook `04_modelo_deep_learning.ipynb`.

---

## 6. Dependencias principales

Las dependencias estándar usadas en los notebooks son:

- Python 3.x  
- `pandas`, `numpy`, `matplotlib`, `seaborn`  
- `scikit-learn`  
- `tensorflow` / `keras`

En Google Colab estas librerías vienen preinstaladas (solo puede ser necesario
actualizar TensorFlow en algunos casos).

---
