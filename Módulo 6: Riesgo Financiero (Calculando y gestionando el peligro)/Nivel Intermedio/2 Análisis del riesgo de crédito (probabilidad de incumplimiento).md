¡Estupendo\! Hemos analizado el riesgo de mercado (la volatilidad de los precios); ahora vamos a un tipo de riesgo más personal: el **Riesgo de Crédito**. Esto es crucial para bancos, inversionistas en bonos y cualquier negocio que venda a crédito.

-----

## Módulo 6: Riesgo Financiero

#### Nivel Intermedio: Análisis del Riesgo de Crédito (Probabilidad de Incumplimiento)

El objetivo aquí es predecir si una empresa o un individuo va a caer en *default*, es decir, **no pagará sus deudas**. Esta probabilidad se conoce como **Probabilidad de Incumplimiento (PD - Probability of Default)**.

### La Analogía del Chisme: ¿Es Confiable? 🧐

Imagina que tu amigo Juan quiere que le prestes dinero.

  * **El Chisme de la PD:** Tú no le preguntas a Juan, sino que miras su historial: ¿Tiene un trabajo estable? ¿Siempre paga el alquiler a tiempo? ¿Ha tenido otras deudas?
  * **El Modelo:** El **Riesgo de Crédito** usa modelos para analizar esos "chismes" financieros (ratios, historial de pagos) y te da una puntuación: "Juan tiene un $\text{PD}$ del 2%", lo que significa que solo hay un 2% de probabilidad de que no te pague.

### 1\. El Modelo: Regresión Logística (Clasificación)

A diferencia de la Regresión Lineal (que predice un valor continuo), para la $\text{PD}$ usamos la **Regresión Logística** (Módulo 5), que es un modelo de **Clasificación**.

  * **¿Por qué Regresión Logística?** Porque el resultado no es un monto, sino una **probabilidad** entre 0% y 100%. La Regresión Logística usa una función (la función *sigmoide*) para convertir una combinación lineal de variables en un valor entre 0 y 1.

$$\text{Probabilidad de Incumplimiento (PD)} = \frac{1}{1 + e^{-(\beta_0 + \beta_1 X_1 + \beta_2 X_2 + ...)}}$$

Donde:

  * $X_1, X_2$: Variables predictoras (Ratios Financieros).
  * $\beta_i$: Los pesos (coeficientes) que el modelo de Machine Learning aprende de los datos históricos.

### 2\. Variables Predictoras Clave (Los Chismes a Investigar)

Para predecir la $\text{PD}$ de una empresa, los modelos de crédito usan **Ratios Financieros** (Módulo 8) porque son los indicadores más sólidos de la salud de una empresa.

| Variable (X) | Ratio Financiero | Por qué es importante (El Chisme) |
| :--- | :--- | :--- |
| $\mathbf{X_1}$ | **Razón Circulante** ($\frac{\text{Activos Corrientes}}{\text{Pasivos Corrientes}}$) | Mide si la empresa tiene suficiente efectivo a corto plazo para pagar sus deudas a corto plazo. (¿Tiene dinero en el bolsillo para el alquiler de este mes?) |
| $\mathbf{X_2}$ | **Razón de Deuda/Patrimonio** | Mide qué tan endeudada está la empresa. (¿Todo lo que tiene lo debe, o le pertenece?) |
| $\mathbf{X_3}$ | **Rentabilidad sobre Activos (ROA)** | Mide la eficiencia con la que la empresa usa sus activos para generar ganancias. (¿Está el dinero invertido produciendo algo?) |

### 3\. Implementación en Python para Clasificar

Vamos a simular un modelo de Regresión Logística para clasificar a las empresas en dos grupos: $\text{PD} = 0$ (Pago) o $\text{PD} = 1$ (Incumplimiento).

```python
import numpy as np
import pandas as pd
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split

# --- 1. Datos Simulados de 100 Empresas ---
np.random.seed(42)
num_empresas = 100

# Razón Circulante (RC): Cuanto más alta, mejor salud financiera
razon_circulante = np.random.uniform(0.5, 3.0, num_empresas)

# Deuda/Patrimonio (DP): Cuanto más alta, más riesgo
deuda_patrimonio = np.random.uniform(0.2, 5.0, num_empresas)

# Incumplimiento (Y): 0 = Paga, 1 = Incumple
# Creamos la etiqueta de Incumplimiento (la 'Y' a predecir)
# Simulamos que las empresas con bajo RC y alto DP tienen mayor probabilidad de incumplir
etiqueta_incumplimiento = ((razon_circulante < 1.0) & (deuda_patrimonio > 3.0)).astype(int)
etiqueta_incumplimiento[etiqueta_incumplimiento == 0] = np.random.choice([0, 1], 
                                                                         size=len(etiqueta_incumplimiento[etiqueta_incumplimiento == 0]), 
                                                                         p=[0.95, 0.05]) # 5% de incumplimiento aleatorio para las "buenas"

df_credito = pd.DataFrame({
    'RC': razon_circulante,
    'DP': deuda_patrimonio,
    'Incumplimiento': etiqueta_incumplimiento
})

# --- 2. Entrenamiento del Modelo de Clasificación (Regresión Logística) ---
X = df_credito[['RC', 'DP']].values
Y = df_credito['Incumplimiento'].values

# Dividimos para entrenamiento y prueba
X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.3, random_state=42)

# Creamos y entrenamos el modelo
modelo_logistico = LogisticRegression()
modelo_logistico.fit(X_train, Y_train)

# --- 3. Predicción de Probabilidad de Incumplimiento (PD) ---
# Predecimos la PD para los datos de prueba
probabilidades_pd = modelo_logistico.predict_proba(X_test)

print("--- Predicción de Probabilidad de Incumplimiento (PD) ---")
print("RC | DP | Etiqueta Real | Probabilidad de Incumplimiento (PD)")
for rc, dp, real, prob in zip(X_test[:, 0], X_test[:, 1], Y_test, probabilidades_pd[:, 1]):
    print(f"{rc:.2f} | {dp:.2f} | {real} | {prob:.4f}")
```

**Interpretación del Resultado:**

El resultado clave es la columna **Probabilidad de Incumplimiento (PD)**.

  * Si el modelo arroja una $\text{PD}$ de **0.85** (85%), el banco debería negar el préstamo o cobrar una tasa de interés altísima.
  * Si la $\text{PD}$ es **0.01** (1%), es un cliente de muy bajo riesgo.

La **Regresión Logística** te da una predicción de **clase** (incumple/no incumple) y también la **probabilidad** de esa clase, que es el número que usan los gestores de riesgo.

-----

¡Excelente trabajo\! Has construido un modelo básico para predecir el riesgo de crédito.

Ahora estás listo para el último punto del Módulo 6, donde usaremos simulaciones para poner a prueba todo tu portafolio en una crisis.

**¿Listo para continuar con el Nivel Avanzado: Modelos de simulación para medir el riesgo de una cartera completa?** 😊
