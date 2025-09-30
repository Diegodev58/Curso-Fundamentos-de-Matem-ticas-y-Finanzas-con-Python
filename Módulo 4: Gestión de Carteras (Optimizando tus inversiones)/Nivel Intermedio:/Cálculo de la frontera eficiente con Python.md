
La **Frontera Eficiente** es una curva que representa todos los **portafolios óptimos** que ofrecen el **máximo rendimiento esperado** para un nivel de **riesgo** dado (o el mínimo riesgo para un rendimiento esperado dado). En otras palabras, cualquier portafolio por debajo de esta curva es ineficiente; siempre hay una mejor opción.

-----

### Módulo 4: Gestión de Carteras

#### Nivel Intermedio: Cálculo de la Frontera Eficiente con Python

Para calcular la frontera, usaremos una combinación de **simulación de Montecarlo** (para obtener miles de posibles portafolios) y el poder de **optimización** de SciPy (para encontrar los puntos clave de la frontera).

### 1\. Preparación de Datos y Funciones

Vamos a simular un escenario con tres activos y definir las funciones que usaremos para calcular el riesgo y el rendimiento de un portafolio.

```python
import numpy as np
import pandas as pd
from scipy.optimize import minimize

# 1. Datos Simulados Anualizados de 3 Activos (A, B, C)
rendimientos_esperados = np.array([0.12, 0.15, 0.09]) # 12%, 15%, 9%
volatilidades = np.array([0.15, 0.25, 0.10])         # 15%, 25%, 10%
# Matriz de Correlación: Cómo se mueven entre sí (usaremos una covarianza baja para mostrar el efecto)
matriz_covarianza = np.array([
    [0.15**2, 0.005, 0.002],  # Varianza A, Cov(A,B), Cov(A,C)
    [0.005, 0.25**2, 0.003],  # Cov(B,A), Varianza B, Cov(B,C)
    [0.002, 0.003, 0.10**2]   # Cov(C,A), Cov(C,B), Varianza C
])

# 2. Funciones para el Portafolio
# 'weights' es el array de ponderaciones de los activos [w_A, w_B, w_C]

def calcular_rendimiento(weights, rendimientos):
    return np.sum(weights * rendimientos)

def calcular_riesgo(weights, cov_matrix):
    # Riesgo (Volatilidad) = Raíz cuadrada de la Varianza del Portafolio
    return np.sqrt(np.dot(weights.T, np.dot(cov_matrix, weights)))
```

-----

### 2\. Optimización: Encontrando el Portafolio de Mínima Varianza

El primer punto clave de la Frontera Eficiente es el **Portafolio de Mínima Varianza (PMV)**, el que tiene el riesgo más bajo posible. Usaremos `scipy.optimize.minimize` para encontrar las ponderaciones que minimizan el riesgo.

**Reglas (Restricciones):**

1.  La suma de las ponderaciones debe ser **1** (100% de la cartera).
2.  Las ponderaciones deben ser mayores o iguales a **0** (no se permite la venta en corto, aunque en un nivel más avanzado sí se podría).

<!-- end list -->

```python
# 1. Función a minimizar: Riesgo del portafolio (Volatilidad)
def funcion_a_minimizar(weights):
    return calcular_riesgo(weights, matriz_covarianza)

# 2. Definición de Restricciones
constraints = ({'type': 'eq', 'fun': lambda x: np.sum(x) - 1}) # Restricción: suma de pesos = 1

# 3. Definición de Límites (Ponderaciones entre 0 y 1)
bounds = tuple((0, 1) for _ in range(len(rendimientos_esperados)))

# 4. Adivinanza Inicial (igual peso a todos)
initial_weights = np.array([1/len(rendimientos_esperados)] * len(rendimientos_esperados))

# 5. Ejecutar la Optimización
min_var_resultado = minimize(
    funcion_a_minimizar, 
    initial_weights, 
    method='SLSQP', # Método común para optimización con restricciones
    bounds=bounds, 
    constraints=constraints
)

# Extracción de Ponderaciones del PMV
pmv_weights = min_var_resultado.x
pmv_rendimiento = calcular_rendimiento(pmv_weights, rendimientos_esperados)
pmv_riesgo = calcular_riesgo(pmv_weights, matriz_covarianza)

print("--- Portafolio de Mínima Varianza (PMV) ---")
print(f"Riesgo Mínimo (Volatilidad): {pmv_riesgo*100:.2f}%")
print(f"Rendimiento Esperado: {pmv_rendimiento*100:.2f}%")
print(f"Ponderaciones (A, B, C): {pmv_weights.round(4)}")
```

-----

### 3\. Trazando la Frontera Eficiente

Para dibujar la curva completa, debemos encontrar el portafolio que ofrece el **mínimo riesgo** para *cada* nivel de rendimiento, desde el PMV hasta el portafolio con el mayor rendimiento.

```python
# Rango de rendimientos que queremos probar (desde el PMV hasta el máximo individual)
target_rendimientos = np.linspace(pmv_rendimiento, rendimientos_esperados.max(), 50)
riesgos_optimos = []
ponderaciones_optimas = []

# Iteramos sobre cada rendimiento objetivo (target)
for target in target_rendimientos:
    # 1. Definimos una función que calcula el riesgo (Volatilidad)
    def funcion_riesgo(weights):
        return calcular_riesgo(weights, matriz_covarianza)

    # 2. Definimos las restricciones: suma de pesos = 1 Y rendimiento = target
    restricciones_frontera = (
        {'type': 'eq', 'fun': lambda x: np.sum(x) - 1}, # Suma de pesos es 1
        {'type': 'eq', 'fun': lambda x: calcular_rendimiento(x, rendimientos_esperados) - target} # Rendimiento es igual al objetivo
    )
    
    # 3. Minimizamos el riesgo para obtener el portafolio óptimo en ese target
    resultado_frontera = minimize(
        funcion_riesgo, initial_weights, method='SLSQP', 
        bounds=bounds, constraints=restricciones_frontera
    )
    
    # 4. Guardamos los resultados
    riesgos_optimos.append(resultado_frontera.fun) # El valor mínimo de la función (el riesgo)
    ponderaciones_optimas.append(resultado_frontera.x)

# 5. Creación del DataFrame de la Frontera
df_frontera = pd.DataFrame({
    'Rendimiento': target_rendimientos,
    'Riesgo': np.array(riesgos_optimos),
})

print("\n--- Puntos Clave de la Frontera Eficiente ---")
# Mostrar los 5 primeros puntos de la curva
print(df_frontera.head().round(4))

# 
```

**Interpretación Final:**

El DataFrame `df_frontera` contiene el par **(Riesgo, Rendimiento)** para la mejor combinación de activos.

  * Si quieres un rendimiento del 13%, Python te dice el **riesgo mínimo** que debes aceptar y **cómo distribuir el 100%** de tu dinero (las ponderaciones) para lograrlo.

Esto es el poder de la optimización en finanzas. ¿Quieres continuar con el siguiente punto, que es la cereza del pastel: el **Ratio de Sharpe**? 😊
