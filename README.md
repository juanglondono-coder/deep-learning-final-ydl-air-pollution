# deep-learning-final-ydl-air-pollution
# Trabajo final – Deep Learning: YDL Air Pollution

Este repositorio contiene el trabajo final del curso de Deep Learning, basado en la competición
**YDL Air Pollution** de Kaggle. El objetivo es predecir la concentración horaria de NMHC(GT)
a partir de variables de calidad del aire y meteorológicas.

## Video de presentación

👉 [Ver video en YouTube]

## Estructura del repositorio

- `01_exploracion_datos.ipynb`  
  Análisis exploratorio del dataset (`train.csv`).

- `02_preprocesado.ipynb`  
  Limpieza de datos, tratamiento de valores -200, imputación y escalado.
  Construcción de las vistas tabular y secuencial (ventanas de 24h).

- `03_modelo_baseline.ipynb`  
  Modelos baseline (Regresión lineal y Random Forest) sobre la vista tabular.

- `04_modelo_deep_learning.ipynb`  
  Modelos LSTM y GRU sobre ventanas temporales.
  Uso de transformación log(1 + NMHC) y comparación con los baselines.

- `05_submission_kaggle.ipynb`  
  Entrenamiento final del modelo GRU_log_target y generación de `submission.csv`
  para la competición de Kaggle.

- `INFORME_PROYECTO.PDF`  
  Informe final (5–10 páginas) con descripción completa del proyecto y resultados.

- `results/`  
  Métricas en CSV (`baseline_metrics.csv`, `dl_metrics.csv`).

- `submissions/`  
  Archivos de envío a Kaggle (`submission_gru_log_target.csv`).

## Datos

Los datos provienen de la competición **YDL Air Pollution** en Kaggle.

1. Descargar `train.csv`, `test.csv` y `sample.csv` desde la página de la competición.  
2. Colocar estos archivos en la raíz del repositorio (mismo nivel que los notebooks).  

## Cómo ejecutar los notebooks en Google Colab

1. Abrir el notebook deseado en GitHub y hacer clic en **“Open in Colab”** (o subir el `.ipynb` a Colab).  
2. Asegurarse de tener los CSV (`train.csv`, `test.csv`, `sample.csv`) en el entorno de ejecución
   (subidos al runtime o montando Google Drive).  
3. Ejecutar todas las celdas de arriba abajo.

El modelo final utilizado para la predicción es una **GRU con target log-transformado**
(`GRU_log_target`), que obtuvo las mejores métricas de MAE y RMSE en el conjunto de test.
