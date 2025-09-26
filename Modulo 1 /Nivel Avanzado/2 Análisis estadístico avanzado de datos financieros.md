
El **análisis estadístico** nos permite ir más allá del precio obvio y entender el **riesgo** y la **relación** entre diferentes activos. Usaremos **NumPy** y **Pandas** para calcular tres métricas esenciales: **Volatilidad**, **Correlación** y la **Distribución de Rendimientos**.

-----

### Módulo 1: Fundamentos de Matemáticas y Finanzas con Python

#### Nivel Avanzado: Análisis Estadístico Avanzado de Datos Financieros

Para este ejercicio, simularemos datos de rendimientos diarios (el porcentaje de ganancia o pérdida en un día) para dos acciones: **Acción A** y **Acción B**.

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

# 1. Creamos datos simulados de rendimientos diarios (porcentajes)
np.random.seed(42) # Para que nuestros resultados sean siempre los mismos
rendimientos_A = np.random.normal(0.0005, 0.01, 100) # Media de 0.05%, Desviación Estándar de 1%
rendimientos_B = np.random.normal(0.0010, 0.02, 100) # Media de 0.10%, Desviación Estándar de 2%

# 2. Creamos un DataFrame con los rendimientos
df_rendimientos = pd.DataFrame({
    'Rendimientos_A': rendimientos_A,
    'Rendimientos_B': rendimientos_B
})
```

### 1\. Volatilidad: Midiendo el Riesgo (Desviación Estándar)

La **Volatilidad** es la medida principal de **riesgo** en finanzas. Te dice cuánto se desvía el precio (o el rendimiento) del promedio. En estadística, esto se llama **Desviación Estándar ($\sigma$)**. Un valor más alto significa que el activo es más arriesgado.

  * **Fórmula:** La Desviación Estándar ($\sigma$) se calcula como la raíz cuadrada de la varianza. NumPy lo calcula automáticamente.
  * **Interpretación:** La volatilidad anualizada te dice cuánto riesgo (en porcentaje) tiene la inversión en un año.

<!-- end list -->

```python
# 1. Calculamos la Desviación Estándar (Volatilidad diaria)
volatilidad_diaria_A = df_rendimientos['Rendimientos_A'].std()
volatilidad_diaria_B = df_rendimientos['Rendimientos_B'].std()

# 2. Anualizamos la volatilidad (multiplicamos por la raíz cuadrada de los días de trading)
# En finanzas, se usan 252 días de trading por año.
dias_de_trading = 252
volatilidad_anual_A = volatilidad_diaria_A * np.sqrt(dias_de_trading)
volatilidad_anual_B = volatilidad_diaria_B * np.sqrt(dias_de_trading)

print("--- 1. Cálculo de la Volatilidad ---")
print(f"Volatilidad Anual (Riesgo) de la Acción A: {volatilidad_anual_A:.2%}")
print(f"Volatilidad Anual (Riesgo) de la Acción B: {volatilidad_anual_B:.2%}")

# Conclusión: La Acción B es el doble de volátil (riesgosa) que la Acción A.
```

-----

### 2\. Correlación: Midiendo la Relación (Diversificación)

La **Correlación** mide cómo se mueven dos activos entre sí. Este valor está siempre entre **-1** y **+1**. Es la clave para la **diversificación** de carteras (Módulo 4).

  * **Correlación de +1:** Los activos se mueven **perfectamente juntos**. Si uno sube un 1%, el otro sube un 1%. (Mala diversificación).
  * **Correlación de -1:** Los activos se mueven en **direcciones opuestas**. Si uno sube, el otro baja. (Buena diversificación).
  * **Correlación de 0:** Los activos **no tienen relación**. Se mueven de forma independiente.

<!-- end list -->

```python
# La función .corr() de Pandas calcula la matriz de correlación completa
matriz_correlacion = df_rendimientos.corr()

# Extraemos el valor entre A y B
correlacion_A_B = matriz_correlacion.loc['Rendimientos_A', 'Rendimientos_B']

print("\n--- 2. Cálculo de la Correlación ---")
print("Matriz de Correlación Completa:")
print(matriz_correlacion)
print(f"\nCorrelación entre A y B: {correlacion_A_B:.4f}")

# Conclusión: Nuestro valor (cercano a 0) indica que estas acciones se mueven de forma independiente.
# ¡Esto es bueno para la diversificación!
```

-----

### 3\. Distribución de Rendimientos: ¿Es Normal el Riesgo?

Para entender el riesgo de forma completa, graficamos la **Distribución de Rendimientos** usando un **Histograma**. En finanzas, se asume a menudo que los rendimientos siguen una **distribución normal** (forma de campana).

```python
# Usamos Matplotlib para visualizar la distribución
plt.figure(figsize=(10, 5))

# Histograma para la Acción A
plt.hist(df_rendimientos['Rendimientos_A'], bins=20, alpha=0.6, label='Acción A')

# Histograma para la Acción B
plt.hist(df_rendimientos['Rendimientos_B'], bins=20, alpha=0.6, label='Acción B')

plt.title('Distribución de Rendimientos Diarios')
plt.xlabel('Rendimiento Diario')
plt.ylabel('Frecuencia')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
# 
```

**Análisis Visual:**

  * Verás que la distribución de la **Acción A** es más **estrecha** y alta (menos volatilidad).
  * La distribución de la **Acción B** es más **ancha** y baja (más dispersión de resultados, lo que significa más volatilidad).

Este análisis estadístico es lo que usan los gestores de carteras y analistas de riesgo para cuantificar el peligro y construir portafolios inteligentes.

