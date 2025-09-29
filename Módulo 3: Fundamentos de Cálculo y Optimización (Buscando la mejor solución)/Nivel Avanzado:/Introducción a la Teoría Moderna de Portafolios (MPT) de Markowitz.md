¡Estás listo para el Nivel Avanzado del Módulo 3\! La **Teoría Moderna de Portafolios (MPT)**, desarrollada por Harry Markowitz, es la piedra angular de la inversión institucional. Es la herramienta matemática que responde a la pregunta fundamental de las finanzas: **¿Cómo elegir el mejor conjunto de activos para obtener el máximo rendimiento con el menor riesgo posible?**

En lugar de elegir la mejor acción, la MPT te enseña a elegir el **mejor portafolio** (o cartera).

-----

### Módulo 3: Fundamentos de Cálculo y Optimización

#### Nivel Avanzado: Teoría Moderna de Portafolios (MPT) de Markowitz

### 1\. Los Tres Pilares de la MPT

La MPT se basa en tres conceptos clave que ya hemos cubierto:

1.  **Rendimiento Esperado:** La ganancia promedio que se espera de un activo (o portafolio).
2.  **Riesgo (Volatilidad):** Medido por la **Desviación Estándar** (Módulo 1).
3.  **Correlación:** Cómo se mueven dos activos entre sí (Módulo 1). ¡Esta es la clave de la diversificación\!

La gran idea de Markowitz es que el **riesgo de un portafolio NO es solo la suma de los riesgos individuales de los activos**. Si juntas activos que no se mueven de la misma manera (baja correlación), el riesgo total del portafolio se reduce.

### 2\. El Problema de Optimización de MPT

La MPT se formula como un problema de optimización donde necesitamos encontrar las **ponderaciones** (los porcentajes) de cada activo en el portafolio que logren uno de estos objetivos:

  * **Minimizar el Riesgo** para un Nivel de Rendimiento *Deseado*.
  * **Maximizar el Rendimiento** para un Nivel de Riesgo *Aceptable*.

Para simplificar, vamos a simular el proceso de optimización para encontrar el portafolio con el **mínimo riesgo** (la cartera de mínima varianza).

### 3\. Implementación Práctica con Python

Usaremos Python para simular miles de portafolios con dos activos (**Acción X** y **Acción Y**) y luego encontrar el mejor entre ellos.

```python
import numpy as np
import pandas as pd

# Datos de Rendimiento y Riesgo Simulados (Anualizados)
rendimientos_esperados = np.array([0.10, 0.15]) # X rinde 10%, Y rinde 15%
volatilidades = np.array([0.08, 0.12])       # X riesgo 8%, Y riesgo 12%
correlacion_XY = 0.50                      # Se mueven en la misma dirección, pero no perfectamente

# Calculamos la Matriz de Covarianza (necesaria para el riesgo del portafolio)
# La covarianza es (Correlación * Volatilidad_X * Volatilidad_Y)
covarianza = correlacion_XY * volatilidades[0] * volatilidades[1]

# Matriz que muestra la covarianza entre los activos
matriz_covarianza = np.array([
    [volatilidades[0]**2, covarianza],
    [covarianza, volatilidades[1]**2]
])

print("--- Matriz de Covarianza (Riesgo Relativo) ---")
print(matriz_covarianza.round(4))
```

-----

### 4\. Simulación y Creación de la Frontera Eficiente

Ahora, simularemos 10,000 portafolios diferentes variando la ponderación de la Acción X (de 0% a 100%) y la Acción Y (el restante).

Para un portafolio con ponderaciones $w$ (un vector), el riesgo y el rendimiento se calculan así:

  * **Rendimiento del Portafolio:** $R_p = w_X R_X + w_Y R_Y$
  * **Riesgo del Portafolio:** $\sigma_p = \sqrt{w^T \cdot \text{Cov} \cdot w}$ (donde $w^T$ es la transpuesta de las ponderaciones y $\text{Cov}$ es la matriz de covarianza).

<!-- end list -->

```python
num_portfolios = 10000
resultados = np.zeros((3, num_portfolios)) # Array para guardar [Rendimiento, Riesgo, Ratio de Sharpe]

for i in range(num_portfolios):
    # Ponderaciones aleatorias para la Acción X (wx)
    w = np.random.random(2)
    w /= np.sum(w) # Aseguramos que la suma de las ponderaciones sea 1 (100%)
    
    # Cálculo del Rendimiento del Portafolio
    rendimiento_portafolio = np.dot(w, rendimientos_esperados)
    
    # Cálculo del Riesgo del Portafolio (Volatilidad)
    riesgo_portafolio = np.sqrt(np.dot(w.T, np.dot(matriz_covarianza, w)))
    
    # Guardamos los resultados (Riesgo, Rendimiento)
    resultados[0, i] = rendimiento_portafolio
    resultados[1, i] = riesgo_portafolio

# Convertimos los resultados a un DataFrame para fácil visualización
df_resultados = pd.DataFrame({
    'Rendimiento': resultados[0],
    'Riesgo': resultados[1],
})
```

### 5\. Identificando la Frontera Eficiente

La **Frontera Eficiente** es la curva formada por los mejores portafolios posibles. Para cualquier nivel de riesgo, es el punto que ofrece el **mayor rendimiento**.

  * **Portafolio de Mínima Varianza:** El portafolio con el menor riesgo posible.

<!-- end list -->

```python
# Encontramos el Portafolio de Mínima Varianza (el que tiene el menor riesgo)
min_riesgo = df_resultados['Riesgo'].min()
portafolio_min_varianza = df_resultados[df_resultados['Riesgo'] == min_riesgo].iloc[0]

print("\n--- Portafolio de Mínima Varianza ---")
print(f"Riesgo Mínimo: {portafolio_min_varianza['Riesgo']:.2%}")
print(f"Rendimiento para ese Riesgo: {portafolio_min_varianza['Rendimiento']:.2%}")
# 
```

**Conclusión:** La MPT te permite usar tus conocimientos de cálculo (optimización) y estadística (volatilidad y correlación) para encontrar el punto óptimo. La **Frontera Eficiente** es la curva que todo gestor de portafolios busca: cualquier punto por debajo de esa curva significa que estás tomando demasiado riesgo para el rendimiento que recibes.

¡Has completado el Módulo 3\! El próximo paso es aplicar esto directamente a la **Gestión de Carteras** real, donde profundizaremos en la diversificación.

¿Listo para iniciar el **Módulo 4, Nivel Básico: Fundamentos de la diversificación y la relación entre riesgo y rendimiento**? 😊
