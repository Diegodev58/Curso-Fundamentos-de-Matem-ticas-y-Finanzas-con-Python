 Hemos sentado las bases de la predicción. Ahora vamos a construir tu primer modelo de Machine Learning en finanzas: la **Regresión Lineal**, que es la forma más sencilla de predecir un valor.

-----

## Módulo 5: Aprendizaje Automático (Machine Learning) en Finanzas

#### Nivel Intermedio: Modelos de Predicción de Precios de Acciones

### 1\. Regresión Lineal: El Modelo Más Básico

La **Regresión Lineal** asume que hay una **relación de línea recta** entre la variable que queremos predecir (la **salida**, como el precio de mañana, $Y$) y la variable que usamos para predecir (la **entrada**, como el precio de hoy, $X$).

Piensa en esto como la fórmula de una línea recta que aprendiste en la escuela:

$$Y = mX + b$$

  * $Y$: El precio que queremos predecir (la variable dependiente).
  * $X$: El precio que ya conocemos (la variable independiente).
  * $m$: La **pendiente** (o el "peso"). Te dice cuánto sube $Y$ por cada unidad que sube $X$.
  * $b$: El **punto de corte**.

El trabajo del algoritmo de Regresión Lineal es encontrar la mejor línea (la $m$ y la $b$ perfectas) que minimice el error entre la línea y los puntos de datos históricos.

### La Analogía del Chisme: "Si hoy hace sol, mañana también"

Imagina que quieres predecir el precio de una acción mañana ($Y$), usando solo el precio de cierre de hoy ($X$). Si históricamente, la acción sube $\$1.00$ por cada $\$1.00$ que sube hoy (pendiente $m=1$), y tiene un precio base $b=0$, la fórmula sería $Y = 1X$. ¡Súper simple\!

-----

### 2\. Implementación con Python: `scikit-learn`

Para el Machine Learning, usaremos la librería estándar de Python: **`scikit-learn`** (a menudo llamada `sklearn`). Usaremos un modelo de **Regresión Lineal Simple** (una sola variable $X$) para predecir el precio futuro.

Para este ejemplo, usaremos el **precio de cierre de ayer** para predecir el **precio de cierre de hoy**.

```python
import pandas as pd
import numpy as np
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split

# --- 1. Preparación de Datos (El Chisme Histórico) ---
# Creamos datos simulados de 10 días
precios = np.array([100, 101, 103, 102, 104, 106, 105, 107, 109, 110])
df = pd.DataFrame({'Precio_Cierre': precios})

# Variable de Entrada (X): Precio de CIERRE de AYER (Shifting the data)
# Usamos .shift(1) para que el precio de hoy sea el precio de AYER en la fila de hoy
df['Precio_Ayer'] = df['Precio_Cierre'].shift(1)

# Variable de Salida (Y): Precio de CIERRE de HOY
df['Precio_Hoy'] = df['Precio_Cierre']

# Eliminamos la primera fila (NaN) porque no tiene precio de ayer
df = df.dropna()

print("--- DataFrame preparado para el ML ---")
print(df)
```

**Análisis del DataFrame:**

| Precio\_Cierre | Precio\_Ayer (X) | Precio\_Hoy (Y) |
| :---: | :---: | :---: |
| 100.0 | NaN | 100.0 |
| **101.0** | **100.0** | 101.0 | \<- Usaremos $X=100$ para predecir $Y=101$
| **103.0** | **101.0** | 103.0 | \<- Usaremos $X=101$ para predecir $Y=103$

### 3\. Entrenamiento del Modelo (La Escuela)

Dividimos los datos en dos grupos: **entrenamiento** (para que el modelo aprenda) y **prueba** (para ver qué tan bien predice).

```python
# Definimos X y Y. Usamos .values.reshape(-1, 1) para que scikit-learn lo acepte
X = df[['Precio_Ayer']].values
Y = df['Precio_Hoy'].values

# Dividimos 80% para entrenar, 20% para probar
X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.2, random_state=42)

# 1. Creamos la instancia del modelo de Regresión Lineal
modelo_regresion = LinearRegression()

# 2. Entrenamos el modelo: la computadora encuentra la mejor 'm' y 'b'
modelo_regresion.fit(X_train, Y_train)

# Coeficientes encontrados por el modelo (m y b)
pendiente_m = modelo_regresion.coef_[0]
punto_corte_b = modelo_regresion.intercept_

print("\n--- Resultados del Entrenamiento ---")
print(f"Pendiente (m): {pendiente_m:.4f}")
print(f"Punto de Corte (b): {punto_corte_b:.4f}")
# El modelo ha encontrado la mejor fórmula para la línea recta!
```

### 4\. Predicción y Conclusión

Ahora que tenemos la fórmula, podemos hacer predicciones con los datos que el modelo nunca ha visto ($X_{test}$) y compararlos con la respuesta correcta ($Y_{test}$).

```python
# Hacemos predicciones con los datos de prueba
Y_pred = modelo_regresion.predict(X_test)

print("\n--- Predicciones (Modelo vs. Real) ---")
for real, pred in zip(Y_test, Y_pred):
    print(f"Precio Real: {real:.2f} | Predicción: {pred:.2f} | Error: {abs(real - pred):.2f}")

# Ejemplo de Predicción Futura: Si el precio de cierre de hoy es $110.00:
proxima_prediccion = modelo_regresion.predict(np.array([[110.00]]))
print(f"\nSi hoy cierra en $110.00, mañana predice: ${proxima_prediccion[0]:.2f}")
```

¡Acabas de construir tu primer modelo de predicción de precios\! Este es un modelo simple, pero sienta las bases para modelos mucho más complejos.

