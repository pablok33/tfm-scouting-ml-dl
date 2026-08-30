# TFM — Scouting de baloncesto europeo con ML y DL

Proyecto de ciencia de datos reproducible para predecir la producción individual de jugadores mediante `PIR_target` en ACB, EuroLeague y EuroCup 2017/18.

Esta carpeta es la versión preparada para publicación. Los notebooks serán autocontenidos: cada uno mostrará en orden la carga, transformación, controles, entrenamiento, evaluación y exportación de evidencias necesarias para comprender el experimento sin consultar código externo.

## Estado de construcción

La parte práctica publicable está implementada y validada mediante siete notebooks autocontenidos. También se ha completado una ejecución consecutiva `01 → 07` con el entorno definido en `environment.yml`. Las métricas y los rankings se reprodujeron sin cambios; únicamente variaron los tiempos de entrenamiento, que dependen de la carga del equipo. Los trece controles del cierre permanecen superados.

## Orden de ejecución disponible

1. `notebooks/01_eda_y_limpieza.ipynb`: integra los seis CSV, aplica controles de calidad, realiza el EDA y genera `data/processed/player_game_clean_1718.csv`.
2. `notebooks/02_construccion_datasets.ipynb`: construye los diseños temporales, valida la elegibilidad y exporta el dataset adoptado 3/5.
3. `notebooks/03_benchmark_ml_walkforward.ipynb`: compara nueve modelos ML y las variantes ajustadas de LightGBM/XGBoost mediante cinco folds temporales y tuning anidado.
4. `notebooks/04_mlp_tabular_walkforward.ipynb`: compara las tres variantes MLP iniciales y el MLP actual con tres semillas, selección temporal del número de `epochs` y registros de TensorBoard.
5. `notebooks/05_lstm_gru_secuencial.ipynb`: construye secuencias de diez partidos y compara LSTM y GRU con Lasso, ElasticNet y el mejor MLP.
6. `notebooks/06_interpretabilidad_elasticnet.ipynb`: compara coeficientes de Lasso/ElasticNet y calcula permutation importance temporal global y por competición.
7. `notebooks/07_resultados_y_cierre.ipynb`: consolida las cifras definitivas, compara los modelos y presenta las conclusiones finales.

Los siete notebooks contienen controles reproducibles sobre las cifras esperadas. Las tablas fuente para Python o Power BI se guardan por etapa en `reports/tables/` y las figuras generadas se conservan en `reports/figures/`. Los historiales de entrenamiento de las redes también se publican como CSV, mientras que los archivos binarios de TensorBoard se regeneran localmente al ejecutar los notebooks 04 y 05.

## Estructura

```text
data/raw/             Datos BAwiR originales incluidos para reproducción
data/processed/       Datasets generados por los notebooks
notebooks/            Proceso completo y explicado paso a paso
reports/tables/       CSV preparados para análisis, memoria o Power BI
reports/figures/      Figuras regenerables, incluidas las curvas de entrenamiento
reports/tensorboard/  Directorio local para logs regenerables de TensorBoard
```

## Entorno previsto

```powershell
conda env create -f environment.yml
conda activate tfm-scouting-ml-dl
python -m ipykernel install --user --name tfm-scouting-ml-dl --display-name "Python (tfm-scouting-ml-dl)"
jupyter lab
```

En VS Code o Jupyter debe seleccionarse explícitamente **Python (tfm-scouting-ml-dl)**. Un kernel genérico llamado `python3` puede resolver otro intérprete instalado y no debe utilizarse sin comprobar `sys.version`.

Para abrir los entrenamientos recurrentes en TensorBoard desde PowerShell:

```powershell
conda activate tfm-scouting-ml-dl
python -m tensorboard.main --logdir reports/tensorboard/05_sequence
```

El guion bajo de `05_sequence` se escribe directamente; no debe escaparse como `05\_sequence`.

El orden definitivo es `01 → 07`. Los notebooks 03–05 concentran el coste: tuning ML, 60 entrenamientos MLP y 30 entrenamientos LSTM/GRU. Los tiempos dependen del equipo; la ejecución validada utilizó CPU para TensorFlow.

Los logs binarios de TensorBoard no se incluyen porque se regeneran durante el entrenamiento. Sí se publican `training_history.csv`, los CSV fuente de las curvas y las figuras correspondientes en las carpetas `reports/tables/04_mlp`, `reports/tables/05_sequence`, `reports/figures/04_mlp` y `reports/figures/05_sequence`.

## Datos

Los seis CSV son exportaciones de los seis datasets 2017/18 incluidos en BAwiR 1.5.3, desarrollado por Guillermo Vinue. Sus nombres lógicos, dimensiones y competiciones coinciden con el manual oficial del paquete. BAwiR se distribuye bajo licencia GPL (≥ 2) y declara como fuentes originales las webs de ACB, EuroLeague y EuroCup.

Los datos se incluyen para permitir la reproducción del estudio, manteniendo la atribución a BAwiR y el aviso `GPL-2.0-or-later`. Esta indicación se limita a los datasets distribuidos dentro del paquete y no debe interpretarse como una licencia concedida directamente por ACB, EuroLeague o EuroCup ni como respaldo de dichas organizaciones. Referencias: [BAwiR en CRAN](https://CRAN.R-project.org/package=BAwiR) y [manual oficial del paquete](https://cran.r-project.org/web/packages/BAwiR/BAwiR.pdf).
