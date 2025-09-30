
El fundamento de este módulo es la **Regla de Oro de la Inversión**: **No poner todos los huevos en la misma canasta**. Esto se llama **Diversificación**.

-----

### Módulo 4: Gestión de Carteras

#### Nivel Básico: Fundamentos de la Diversificación y Riesgo

### 1\. La Diversificación: Reduciendo el Riesgo Específico

La diversificación es el arte de combinar diferentes activos en una cartera. Su objetivo principal es reducir el **Riesgo No Sistemático** (o riesgo específico), que es el riesgo único de una sola empresa o sector.

  * **Analogía:** Si solo inviertes en una compañía de helados, te va muy bien en verano, pero muy mal en invierno o si hay una escasez de azúcar. Si inviertes en helados *y* en paraguas, una ganancia compensa la otra, y tu rendimiento general es más estable.

La clave no es solo tener muchos activos, sino tener activos con **baja correlación** (como vimos en el Módulo 1), es decir, activos que no se mueven de la misma manera al mismo tiempo.

### 2\. La Relación Riesgo y Rendimiento

En finanzas, existe una verdad fundamental: **Para obtener un rendimiento potencialmente más alto, debes aceptar un riesgo más alto.**

  * **Activo sin Riesgo ($R_f$):** Una inversión muy segura (como un bono de EE. UU.) tiene un rendimiento bajo pero predecible.
  * **Activo con Riesgo:** Una acción volátil o una criptomoneda puede ofrecer un rendimiento enorme, pero también puede llevar a grandes pérdidas.

La meta de un inversor es ubicarse en la **Frontera Eficiente** (Módulo 3), que es la curva donde obtienes el **máximo rendimiento posible** por cada unidad de riesgo que estás dispuesto a tolerar.

### 3\. Midiendo el Riesgo de una Inversión: La Volatilidad

En la práctica, la forma más común de medir el riesgo de un activo es su **Volatilidad**, que se cuantifica con la **Desviación Estándar ($\sigma$)**.

  * La Desviación Estándar te dice cuánto se desvían los rendimientos diarios del rendimiento promedio.
  * Una $\sigma$ **alta** indica que el precio tiene cambios bruscos (alto riesgo).
  * Una $\sigma$ **baja** indica que el precio es estable (bajo riesgo).

#### Implementación en Python: Cálculo del Rendimiento y Riesgo

Usaremos una simulación simple de dos activos para ver cómo calcular su rendimiento y riesgo individuales.

```python
import numpy as np
import pandas as pd

# Datos simulados de rendimientos diarios (porcentajes de cambio)
rendimientos_A = np.array([0.01, -0.005, 0.02, 0.001, -0.015])
rendimientos_B = np.array([0.002, 0.003, 0.001, 0.004, 0.005])

# Creamos un DataFrame
df_activos = pd.DataFrame({
    'Rendimientos_A': rendimientos_A,
    'Rendimientos_B': rendimientos_B
})

# 1. Cálculo del Rendimiento Esperado (Promedio)
rendimiento_promedio_A = df_activos['Rendimientos_A'].mean()
rendimiento_promedio_B = df_activos['Rendimientos_B'].mean()

# 2. Cálculo del Riesgo (Desviación Estándar o Volatilidad)
riesgo_volatilidad_A = df_activos['Rendimientos_A'].std()
riesgo_volatilidad_B = df_activos['Rendimientos_B'].std()

print("--- Análisis de Riesgo y Rendimiento (Individual) ---")
print(f"Rendimiento Promedio de Activo A: {rendimiento_promedio_A:.4f}")
print(f"Volatilidad (Riesgo) de Activo A: {riesgo_volatilidad_A:.4f}")
print(f"\nRendimiento Promedio de Activo B: {rendimiento_promedio_B:.4f}")
print(f"Volatilidad (Riesgo) de Activo B: {riesgo_volatilidad_B:.4f}")

# Conclusión:
# El Activo A tiene un riesgo y un rendimiento mucho mayor que el Activo B, que es muy estable.
```

El siguiente paso, el Nivel Intermedio, nos mostrará cómo esta regla se aplica a un portafolio completo, es decir, el riesgo se mide en conjunto: **Medidas de riesgo: Desviación estándar y varianza**.

¿Estás listo para aprender a medir el riesgo cuando combinas varios activos? 😊
