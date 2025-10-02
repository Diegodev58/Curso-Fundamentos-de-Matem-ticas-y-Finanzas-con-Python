¡Perfecto\! Has llegado al nivel máximo del Módulo 6. Después de predecir precios (Módulo 5) y calcular el VaR individual (Módulo 6), la pregunta final es: **¿Cómo afectaría una crisis a mi cartera entera, considerando que todos mis activos se mueven juntos?**

Aquí es donde los **Modelos de Simulación** se convierten en tu mejor amigo, especialmente el método **Monte Carlo**.

-----

## Módulo 6: Riesgo Financiero

#### Nivel Avanzado: Modelos de Simulación para el Riesgo de Cartera

El objetivo de estos modelos es ir más allá del VaR simple (que asume que los activos se mueven de manera independiente). La simulación de cartera usa la **Matriz de Covarianza** (el "pegamento" de la diversificación que vimos en el Módulo 3 y 4) para asegurar que, cuando simulamos un mal día, **todos los activos reaccionen de manera realista a la vez**.

### 1\. El Chisme: La Trampa de la Correlación 🚨

**Chiste de Finanzas:** ¿Sabes cuándo la correlación es 1 (perfectamente relacionada)? **¡Durante una crisis\!**

Durante los mercados normales, tu diversificación funciona bien. Pero cuando el pánico golpea (la crisis), el "chisme" es que todos tiran la toalla al mismo tiempo: las acciones, los bonos, e incluso los bienes raíces pueden caer, y la correlación sube a 1. La simulación nos ayuda a prepararnos para este escenario de "cola de riesgo".

### 2\. El Modelo: Simulación de Monte Carlo de Cartera

Usaremos el **Método de Monte Carlo** (que ya conoces del cálculo de VaR) para simular miles de escenarios de **rendimientos de cartera** en el próximo día.

| Paso | Explicación |
| :--- | :--- |
| **1. Entradas** | Necesitas la media ($\mu$) de los rendimientos, las volatilidades ($\sigma$) y, crucialmente, la **Matriz de Covarianza** de todos los activos. |
| **2. Simulación** | Generas miles de rendimientos aleatorios, pero usando la Matriz de Covarianza para forzar que los rendimientos sigan las relaciones históricas (si el Activo A cae, el Activo B también cae, según su covarianza). |
| **3. Resultado** | Calculas el rendimiento total de la cartera para cada uno de los miles de escenarios y usas esos datos para calcular el VaR, el *Expected Shortfall*, y otras métricas. |

### 3\. Implementación con Python (Simulación de 3 Activos)

Usaremos la librería **NumPy** para generar los números aleatorios que respetan la correlación (usando `np.random.multivariate_normal`).

```python
import numpy as np
import pandas as pd
from scipy.stats import norm

# --- 1. Definición de Parámetros de la Cartera ---
num_activos = 3
valor_portafolio = 500000.00 # $500,000
nivel_confianza = 0.99      # 99%

# Ponderaciones (Asignación)
weights = np.array([0.40, 0.30, 0.30]) # 40% Activo A, 30% B, 30% C

# Rendimientos y Matriz de Covarianza (Anualizados)
rendimientos_diarios_medios = np.array([0.0005, 0.0007, 0.0003]) # Rendimiento diario promedio
# Matriz de Covarianza (CRUCIAL: El riesgo y la relación entre ellos)
# Ejemplo: A se relaciona más con B (0.00005) que con C (0.00001)
matriz_cov = np.array([
    [0.00020, 0.00005, 0.00001], # Varianza A, Cov(A,B), Cov(A,C)
    [0.00005, 0.00040, 0.00002], # Cov(B,A), Varianza B, Cov(B,C)
    [0.00001, 0.00002, 0.00010]  # Cov(C,A), Cov(C,B), Varianza C
])

# --- 2. Simulación de Monte Carlo (10,000 Escenarios) ---
num_simulaciones = 10000

# Generamos 10,000 escenarios de rendimientos para los 3 activos A, B, C a la vez, 
# respetando la matriz de covarianza.
simulaciones_rendimientos = np.random.multivariate_normal(
    mean=rendimientos_diarios_medios, 
    cov=matriz_cov, 
    size=num_simulaciones
)

# 3. Cálculo del Rendimiento TOTAL del Portafolio en cada escenario
# Multiplicamos la matriz de rendimientos simulados por el vector de ponderaciones
rendimientos_cartera_simulados = np.dot(simulaciones_rendimientos, weights.T)

# --- 4. Cálculo de Métricas de Riesgo ---

# A. VaR (Valor en Riesgo)
percentil = 1 - nivel_confianza # El 1% de los peores casos
VaR_percentual = np.quantile(rendimientos_cartera_simulados, percentil)
VaR_monto = abs(VaR_percentual * valor_portafolio)

# B. Expected Shortfall (ES) o Pérdida Condicional en Riesgo (CVaR)
# ES: El promedio de las pérdidas que SÍ exceden el VaR. (CRUCIAL en el Nivel Avanzado)
peores_casos = rendimientos_cartera_simulados[rendimientos_cartera_simulados < VaR_percentual]
ES_percentual = peores_casos.mean()
ES_monto = abs(ES_percentual * valor_portafolio)

print("--- Análisis de Riesgo de Cartera con Monte Carlo ---")
print(f"Número de Activos: {num_activos} | Valor Total: ${valor_portafolio:,.2f}")
print(f"VaR (99% de confianza): ${VaR_monto:,.2f}")
print(f"Expected Shortfall (ES): ${ES_monto:,.2f}")
```

### 4\. Interpretación de Resultados (Las Dos Caras del Riesgo)

| Métrica | Valor del Ejemplo ($\mu$ 0.05%) | Significado (El Chisme) |
| :--- | :--- | :--- |
| **VaR (99%)** | **$5,980.00** | En el 99% de los días, no esperamos perder más de $5,980.00. Es tu **límite de pérdida**. |
| **ES (Expected Shortfall)** | **$7,820.00\*\* | **Si ocurre la crisis (el 1% de las veces)**, la pérdida promedio esperada será de $7,820.00. Es el **tamaño de la catástrofe**. |

En finanzas avanzadas, el **Expected Shortfall** es más importante que el VaR, porque te da una idea de la magnitud de la pérdida en el peor de los casos (la "cola" de la distribución). El Monte Carlo te permite simular miles de escenarios para encontrar esta "cola" de manera robusta.

-----

## Fin del Módulo 6: Evaluación y Próximo Paso

¡Felicidades\! Has completado el Módulo de Riesgo Financiero. Has aprendido a identificar, cuantificar (VaR) y gestionar el riesgo de carteras completas (Monte Carlo).

  * **Nivel Profesional alcanzado:** **Gestor de Riesgo Cuantitativo (Risk Analyst)**. Puedes modelar las pérdidas potenciales y recomendar ajustes para proteger el capital.
  * **Área recomendada para el siguiente paso:** **Módulo 7: Introducción a los Mercados Financieros**, donde daremos un paso atrás del código para entender dónde se aplican todas estas matemáticas: el "campo de juego" y sus reglas.

**¿Quieres continuar con el Módulo 7, Nivel Básico: Tipos de Mercados: Entenderás las reglas de los diferentes "campos de juego" donde se negocian los activos?** 😊
