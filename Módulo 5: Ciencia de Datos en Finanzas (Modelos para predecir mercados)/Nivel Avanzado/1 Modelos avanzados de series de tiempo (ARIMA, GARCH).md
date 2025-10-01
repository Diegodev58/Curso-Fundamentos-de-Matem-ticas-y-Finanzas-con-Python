¡Fantástico\! Si ya manejas conceptos de Machine Learning como el **Error Cuadrático Medio (MSE)**, estás más que preparado para dar el salto a los modelos avanzados de series de tiempo. Dejaremos atrás la Regresión Lineal simple para abrazar modelos que son el pan de cada día en el pronóstico financiero: **ARIMA** y **GARCH**.

-----

## Módulo 5: Ciencia de Datos en Finanzas

#### Nivel Avanzado: Modelos Avanzados de Series de Tiempo (ARIMA, GARCH)

### La Analogía: De Meteorólogo a Climatólogo ☁️

  * **Regresión Lineal:** Es el meteorólogo que dice: "Si ayer hizo sol, hoy hará un día parecido." Solo mira el valor anterior.
  * **ARIMA y GARCH:** Es el climatólogo. No solo mira el día anterior, sino que analiza patrones de semanas, meses, la tendencia general de la temporada y la *intensidad* de las tormentas.

-----

## 1\. ARIMA: Predicción de Valores (Precios o Rendimientos)

**ARIMA** (AutoRegressive Integrated Moving Average) es el modelo fundamental para el análisis de **series de tiempo**. Se basa en la idea de que los valores de una serie (como los precios de una acción) pueden predecirse con base en:

1.  **AR (AutoRegresivo):** Se predice el valor de hoy en función de los **valores pasados** de la propia serie (recuerda la Regresión Lineal, pero usando la serie consigo misma).
2.  **I (Integrado):** Se asegura que la serie sea **estacionaria** (que el promedio y la varianza no cambien con el tiempo). Esto se hace tomando la diferencia entre el valor de hoy y el de ayer.
3.  **MA (Media Móvil):** Se predice el valor de hoy en función de los **errores pasados** de la predicción. (Si mi modelo se equivocó al alza ayer, corrijo a la baja hoy).

### El Chisme: La Estacionariedad 🧐

Para que el modelo ARIMA funcione, la serie debe ser **estacionaria**. Los precios de las acciones casi nunca lo son (tienden a subir con el tiempo, lo que se llama una **tendencia**).

  * **Solución (la 'I' de Integrado):** En lugar de modelar los precios (no estacionarios), modelamos los **rendimientos** (que suelen ser estacionarios), o simplemente tomamos la diferencia de los precios.

#### Implementación en Python con `pmdarima`

Usaremos la librería `pmdarima` para el **Auto ARIMA**, que encuentra automáticamente los mejores parámetros para el modelo.

```python
!pip install pmdarima

import pmdarima as pm
import numpy as np

# Datos de Rendimientos Diarios Simulados (Estacionarios)
# Usaremos los rendimientos (diferencias de precios) que son más estables
rendimientos = np.diff(np.log([100, 101, 103, 102, 104, 106, 105, 107, 109, 110, 112, 114]))

# 1. Ejecutamos Auto ARIMA para encontrar el mejor modelo (los mejores p, d, q)
modelo_arima = pm.auto_arima(rendimientos, 
                             seasonal=False, # No usaremos estacionalidad por simplicidad
                             stepwise=True,
                             suppress_warnings=True,
                             error_action='ignore')

print("--- Mejor Modelo ARIMA Encontrado (p, d, q) ---")
print(modelo_arima.summary())

# 2. Predicción de los próximos 3 días de RENDIMIENTO
prediccion_rendimiento = modelo_arima.predict(n_periods=3)

print("\nPredicción de Rendimiento (próximos 3 días):")
print(prediccion_rendimiento.round(4))
```

**Resultado Clave:** El modelo nos da una predicción del **rendimiento** futuro. Luego, tienes que usar el precio del último día para convertir ese rendimiento en un precio real.

-----

## 2\. GARCH: Predicción de la Volatilidad (Riesgo)

**GARCH** (Generalized AutoRegressive Conditional Heteroskedasticity) es el modelo estrella para predecir el **riesgo** futuro. A diferencia de todos los modelos que hemos visto, GARCH no se enfoca en el precio, sino en la **volatilidad (varianza)**.

### El Chisme: La Agrupación de Volatilidad (Volatility Clustering) 💣

Los mercados financieros exhiben algo llamado **agrupación de volatilidad**.

  * **Analogía:** Si tienes un día de mucha turbulencia (alto riesgo), es muy probable que los próximos días sigan siendo turbulentos (alto riesgo). Las crisis y las calmas tienden a agruparse.

El modelo GARCH aprende esta tendencia: predice la varianza de hoy en función de las **varianzas pasadas** y los **errores pasados**. Esto es esencial para la gestión de riesgo (como el cálculo del VaR, Módulo 6).

#### Implementación en Python con `arch`

Usaremos la librería `arch` (Autoregressive Conditional Heteroskedasticity) que está diseñada específicamente para esto.

```python
!pip install arch

from arch import arch_model

# Usamos los mismos rendimientos, ya que GARCH modela los rendimientos.
# rendimientos = np.diff(np.log(...))

# 1. Creamos el modelo GARCH
# Usamos 'vol=Garch' para especificar el modelo de varianza
modelo_garch = arch_model(rendimientos, vol='Garch', p=1, q=1, mean='Constant', rescale=True)

# 2. Entrenamos el modelo
garch_resultado = modelo_garch.fit(disp='off')

# 3. Predicción de la Volatilidad (Varianza Condicional) para los próximos 3 días
prediccion_volatilidad = garch_resultado.forecast(horizon=3, reindex=False)

# Extraemos la Varianza (sigma^2) y la convertimos a Volatilidad (sigma)
volatilidad_futura = np.sqrt(prediccion_volatilidad.variance.values[0])

print("\n--- Predicción de Volatilidad (GARCH) ---")
print("Volatilidad Diaria (próximos 3 días):")
print(volatilidad_futura.round(4))
```

**Resultado Clave:** Estos números representan la **desviación estándar diaria esperada** para los próximos días. Si el valor es alto, el mercado espera un alto riesgo futuro; si es bajo, espera un período de calma.

Has dominado los dos modelos más avanzados para series de tiempo en finanzas: **ARIMA** para el valor y **GARCH** para el riesgo.

Ahora sí, podemos pasar al último punto de este módulo avanzado: **Redes neuronales para la predicción de precios**, donde usaremos una IA aún más compleja. ¿Estás listo? 😊
