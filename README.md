# Proyecto de Clasificación de Riesgo Crediticio

##  Descripción general

Este proyecto construye y evalúa un modelo de clasificación binaria para predecir el **riesgo crediticio** de clientes (0 = no riesgoso, 1 = riesgoso). Se utilizó un modelo de **Regresión Logística** (`lr_1.pkl`) por su alta interpretabilidad, requisito fundamental en entornos financieros y regulatorios.

Además de la evaluación del desempeño, se aplicaron técnicas de **explicabilidad SHAP** y se realizaron validaciones robustas mediante **simulación Monte Carlo** y **casos manuales** con clientes ficticios.

---

##  Modelo utilizado

- **Algoritmo:** Regresión Logística.
- **Variables predictoras:**  
  `Perfil`, `Estado`, `Edad`, `Genero`, `ScoreCrediticio`, `PorcentajeFinanciacion`, `Plazo`, `IngresoEstimado`, `Gastos`, `CapacidadDePago`, `ValorCuotaMensual`.
- **Variable objetivo:** `Estado` (0: no incumple, 1: incumple / riesgo).
- **Preprocesamiento:** Normalización con `StandardScaler` (ajustado sobre `X_train`).

---

##  Interpretabilidad con SHAP

Se utilizó `shap.LinearExplainer` para analizar la influencia de cada variable en la predicción del modelo.

### Resultados clave:

- **`CapacidadDePago`** y **`ValorCuotaMensual`** son las variables más influyentes.
- Una capacidad de pago negativa (gastos > ingresos) aumenta drásticamente la probabilidad de riesgo.
- Un `ScoreCrediticio` alto reduce el riesgo.
- Los gráficos *beeswarm*, *dependence*, *waterfall* y *force plot* permiten explicar predicciones individuales y patrones globales.

---

##  Validación del modelo

###  Validación Monte Carlo

- Se muestrearon **10 observaciones** aleatorias del conjunto de prueba.
- Se repitió el proceso **1000 veces** para estimar la estabilidad del desempeño.
- **Métrica principal:** Recall (sensibilidad).

**Resultado promedio:**  
**Recall = 83.5%** (desviación estándar: 0.174)

Este resultado indica que el modelo detecta correctamente la mayoría de los clientes realmente riesgosos, aunque con cierta variabilidad esperable por el tamaño muestral.

###  Validación manual con clientes ficticios

Se crearon **10 perfiles hipotéticos** (archivo `clientes_manual.csv`) que cubren distintos niveles de ingresos, gastos, cuotas y scores.  
Se escalaron con el mismo `StandardScaler` y se aplicó el modelo.

**Ejemplo de resultados:**

| Cliente | Ingreso | Gastos | Cuota | Score | Prob. Riesgo | Clasificación |
|--------|--------|--------|-------|-------|--------------|----------------|
| 1      | 1.8M   | 1.9M   | 1.35M | 650   | 90.5%        | Riesgoso       |
| 2      | 4.0M   | 2.2M   | 1.1M  | 820   | 8.4%         | No Riesgoso    |
| 8      | 8.0M   | 3.5M   | 1.6M  | 940   | 2.8%         | No Riesgoso    |

**Análisis cualitativo:**  
El modelo clasifica correctamente según la lógica financiera: ingresos insuficientes o capacidad de pago negativa derivan en riesgo alto; ingresos altos, buena capacidad de pago y score elevado resultan en riesgo bajo.

---

##  Referencias metodológicas

1. Akil, S., Sekkate, S. & Adib, A. (2024). *Enhancing Credit Scoring Models with Monte Carlo Simulated Features*. 12th IEEE ISIVC, pp. 1–6.  
   → Respalda el uso de simulación Monte Carlo para evaluar estabilidad en modelos de scoring.

2. (2025). *Evaluating the stability of model explanations in instance-dependent cost-sensitive credit scoring*. European Journal of Operational Research.  
   → Valida el uso de SHAP para explicabilidad y transparencia en decisiones crediticias.

---

##  Autor
Yedison Cuervo
Estudiante de ingenieria industrial.




---


