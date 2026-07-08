# Optimizador de Portafolios: Teoría Moderna de Markowitz

## Descripción del Proyecto
Este proyecto implementa un modelo cuantitativo de optimización de portafolios de inversión basado en la **Teoría Moderna de Portafolios (MPT)** de Harry Markowitz. El objetivo principal es encontrar la asignación óptima de capital (*asset allocation*) entre un conjunto de activos financieros (SPY, BND, GLD, QQQ, VTI) para maximizar el rendimiento esperado ajustado por riesgo, utilizando el **Ratio de Sharpe** como métrica objetivo.

## Problema Financiero a Resolver
En la gestión de inversiones, el riesgo de un portafolio no es un simple promedio de las volatilidades individuales de sus activos, sino que depende de la correlación entre ellos. El problema consiste en hallar el vector de pesos que maximice la prima de riesgo del portafolio, sujeto a restricciones de diversificación obligatoria y la prohibición de ventas en corto (*short-selling*).

## Fundamentos Matemáticos

El modelo se construye sobre los siguientes pilares estadísticos y algebraicos:

* **Rendimientos Logarítmicos:** En lugar de retornos discretos, se utilizan retornos continuos para asegurar la simetría temporal.
    $$R_{i,t} = \ln\left(\frac{P_{i,t}}{P_{i,t-1}}\right)$$
* **Rendimiento Esperado del Portafolio:** Una combinación lineal de los pesos y las medias históricas anualizadas.
    $$\mathbb{E}[R_p] = \mathbf{w}^T \boldsymbol{\mu}$$
* **Riesgo del Portafolio (Volatilidad):** Calculado mediante la forma cuadrática de la matriz de varianza-covarianza ($\Sigma$).
    $$\sigma_p = \sqrt{\mathbf{w}^T \Sigma \mathbf{w}}$$
* **Función Objetivo (Ratio de Sharpe):** Mide el exceso de retorno por unidad de riesgo asumido. Se asume una tasa libre de riesgo ($R_F$) del 6.5%.
    $$SR = \frac{\mathbb{E}[R_p] - R_F}{\sigma_p}$$

## Metodología de Optimización

Para encontrar el máximo global del Ratio de Sharpe, el problema se modeló utilizando **Programación Cuadrática Secuencial (SLSQP)** a través de `scipy.optimize`. Dado que los optimizadores minimizan funciones, se minimizó el Ratio de Sharpe negativo.

Se aplicaron las siguientes restricciones algebraicas al vector de pesos $\mathbf{w}$:
1.  **Restricción Presupuestaria (Igualdad):** El 100% del capital debe ser invertido.
    $$\sum_{i=1}^N w_i = 1$$
2.  **Límites de Diversificación (Desigualdad):** No se permiten posiciones cortas y ningún activo puede concentrar más del 40% del capital.
    $$0 \leq w_i \leq 0.4 \quad \forall i$$

## Stack Tecnológico
* **Lenguaje:** Python 3
* **Extracción de Datos:** `yfinance` (precios de cierre ajustados, ventana de 5 años históricos).
* **Cálculo Matricial y Análisis de Datos:** `NumPy`, `Pandas`.
* **Optimización Matemática:** `SciPy` (módulo `optimize.minimize`).
* **Visualización:** `Matplotlib`.

## Resultados Generados
El *script* devuelve analíticamente la distribución óptima de pesos y calcula las métricas proyectadas del portafolio (Rendimiento Esperado, Volatilidad y Ratio de Sharpe Óptimo). Además, genera una visualización de barras que ilustra claramente el *asset allocation* resultante.

## Cómo ejecutar este proyecto
1. Clonar el repositorio.
2. Instalar las dependencias: `pip install -r requirements.txt`
3. Ejecutar el archivo `Optimizacion de Portafolio.ipynb` en un entorno de Jupyter (Jupyter Notebook, JupyterLab o VS Code).
