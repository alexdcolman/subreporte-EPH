# Subreporte y no reporte de ingresos en la EPH durante 2024: análisis comparativo con fuentes administrativas

Este proyecto analiza el subreporte y no reporte de ingresos en los hogares encuestados por la Encuesta Permanente de Hogares (EPH, INDEC, Argentina) durante los cuatro trimestres de 2024. A través de técnicas de aprendizaje automático y ajustes basados en fuentes administrativas (SIPA/ANSES), se busca evaluar la precisión de la medición de ingresos y sus implicancias sobre los indicadores oficiales de pobreza en la Argentina.

## Autores

Este proyecto fue desarrollado por:

- **Alex Colman** – Diseño metodológico, modelado y análisis
- **Alejo Gárdenes** – Diseño metodológico, modelado y análisis

Para dudas o colaboraciones, podés contactarnos vía GitHub o correo:

alexdcolman@gmail.com | alejomgarderes@gmail.com

## Objetivos

- Detectar y caracterizar casos de subreporte y no reporte de ingresos.
- Estimar la magnitud del subreporte mediante simulaciones ajustadas.
- Modelar patrones de subdeclaración con técnicas supervisadas y no supervisadas.

## Herramientas utilizadas

- Python 3.x
- Jupyter Notebooks
- Pandas, NumPy, Scikit-learn, Matplotlib, Seaborn
- Git y GitHub para control de versiones
- Binder para visualización interactiva

## Resultados principales

### Clustering no supervisado (PCA + KMeans / Ward)

Los métodos no supervisados (PCA seguido de KMeans y Ward) no lograron recuperar las tres clases de ingreso (declaración precisa, subreporte, no reporte) en el espacio latente.

- **Adjusted Rand Index (KMeans)**: 0.005  
- **Adjusted Rand Index (Ward)**: -0.006

Esto indica una escasa correspondencia entre clases reales y clusters emergentes. La distribución proporcional por clase refuerza esta conclusión:

| Cluster | Clase 0 | Clase 1 | Clase 2 |
|---------|---------|---------|---------|
|   0     |  41.1%  |  33.8%  |  48.5%  |
|   1     |  33.7%  |  47.1%  |  32.6%  |
|   2     |  25.2%  |  19.2%  |  18.9%  |

![Distribución Clases por Cluster](https://github.com/alexdcolman/subreporte-EPH/blob/img/tabla_clusters.png)  
![PCA Clusters](https://github.com/alexdcolman/subreporte-EPH/blob/img/pca_clusters.png)  
![Dendrograma](https://github.com/alexdcolman/subreporte-EPH/blob/img/dendrograma.png)

---

### Modelos supervisados: Random Forest vs. XGBoost

Se compararon los modelos bajo dos esquemas: entrenamiento sobre todo el año vs. entrenamiento en Trimestres 1 a 3 (T1-T3) y test en Trimestre 4 (T4). XGBoost obtuvo mejor rendimiento en todas las métricas principales, aunque con diferencias moderadas.

| Modelo         | División temporal | Accuracy | F1 Macro | AUC-ROC |
|----------------|-------------------|----------|----------|---------|
| Random Forest  | T1-T3 / T4        | 57.3%    | 0.550    | –       |
| XGBoost        | T1-T3 / T4        | 57.9%    | 0.554    | 0.763   |
| Random Forest  | Aglomerado total  | 58.2%    | 0.535    | –       |
| XGBoost        | Aglomerado total  | 59.1%    | 0.540    | 0.753   |
  
![Comparación Métricas](https://github.com/alexdcolman/subreporte-EPH/blob/img/metrics_rf_xgb.png)

---

### Variables más importantes

Las variables con mayor importancia para ambos modelos incluyen:

- `AGLOMERADO`
- `TRIMESTRE`
- `IX_MAYEQ10`, `IX_MEN10`
- Variables socioeconómicas específicas: `V1`, `V2`, `II8`, `IV2`
  
![Importancia de Variables](https://github.com/alexdcolman/subreporte-EPH/blob/img/feature_importance2.png)
![Importancia de Variables](https://github.com/alexdcolman/subreporte-EPH/blob/img/feature_importance.png)
---

### Conclusiones

El subreporte de ingresos **no se manifiesta como una estructura latente clara** que pueda detectarse con clustering. Aunque **XGBoost mejora marginalmente a Random Forest**, las dificultades persisten para predecir correctamente los casos de subreporte y no reporte.

Estos resultados abren nuevas preguntas sobre la complejidad del fenómeno, las limitaciones de los datos disponibles y la necesidad de estrategias de modelado más sofisticadas, junto con una mejor ingeniería de variables para capturar patrones ocultos.

---

## Cómo reproducir este proyecto

1. Cloná el repositorio:

```bash
git clone https://github.com/tu_usuario/subreporte-ingresos-EPH.git
cd subreporte-ingresos-EPH
```

2. Instalá las dependencias necesarias (podés usar un entorno virtual):

```bash
pip install -r requirements.txt
```
3. Abrí los notebooks en JupyterLab y ejecutalos en orden.


## Recursos interactivos

[    Versión interactiva vía Binder](https://hub.gesis.mybinder.org/user/alexdcolman-subreporte-eph-u8vtrmbd/doc/tree/analisis_subreporte.ipynb)

## Licencia

Este proyecto se publica bajo la licencia MIT. Podés reutilizar y modificar el código citando adecuadamente.

---

## Agradecimientos

Este proyecto se benefició del acompañamiento y las observaciones de **Julián Ansaldo**, a quien agradecemos especialmente por su orientación y generosidad durante todo el proceso de trabajo.
