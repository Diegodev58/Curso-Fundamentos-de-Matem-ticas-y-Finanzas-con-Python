¡Genial\! Ya conoces el concepto del **VaR**; ahora vamos a la práctica. En el mundo real, los gestores de riesgo no confían solo en el método paramétrico (que asume la campana de Gauss); usan el **Método Histórico** y el **Método de Monte Carlo** para obtener una imagen más robusta del riesgo.

-----

## Módulo 6: Riesgo Financiero

#### Nivel Intermedio: Cálculo del VaR con Python

### La Analogía del Chisme: La Experiencia vs. La Simulación 🧙

  * **Método Histórico:** Es como predecir el futuro de una relación basándote **únicamente** en todo el historial de peleas y reconciliaciones que ya tuvieron. Es simple y se basa en la realidad.
  * **Método de Monte Carlo:** Es como predecir el futuro de esa relación creando **miles de escenarios** de peleas y reconciliaciones *posibles*, incluso algunos que nunca han ocurrido. Es más complejo, pero abarca más posibilidades.

-----

## 1\. VaR por el Método Histórico

El **Método Histórico** es el más sencillo y no asume ninguna distribución estadística (como la normal). Simplemente toma los **rendimientos reales** de un portafolio durante un período (ej. los últimos 250 días) y ordena las pérdidas de la peor a la menor.

### El Proceso:

1.  Calcula los rendimientos diarios del portafolio.
2.  Ordena los rendimientos de forma ascendente.
3.  Encuentra el percentil que corresponde al nivel de confianza.

**Ejemplo:** Si tienes 100 rendimientos y quieres el VaR al 99% (el 1% peor), buscas el rendimiento que está en la posición 1 (el 1% más bajo).

#### Implementación en Python con Datos Simulados Realistas

```python
import numpy as np
import pandas as pd
# Simularemos rendimientos que se parecen a los reales (con una "cola" más pesada)
np.random.seed(42)

# Simulamos 252 días (aproximadamente 1 año) de rendimientos
rendimientos = np.random.normal(loc=0.0005, scale=0.015, size=252) # Media 0.05%, Vol 1.5%

# Valor del portafolio hoy
valor_portafolio = 100000.00 # $100,000

# 1. Parámetros
nivel_confianza = 0.99  # 99%
percentil = 1 - nivel_confianza # El 1% peor

# 2. Ordenar los rendimientos (de menor a mayor)
rendimientos_ordenados = np.sort(rendimientos)

# 3. Encontrar el VaR (el percentil 1)
# np.quantile encuentra el valor en ese percentil (ej. 0.01)
var_percentual_historico = np.quantile(rendimientos_ordenados, percentil)

# 4. Calcular el VaR en monto (multiplicar el % por el valor del portafolio)
var_monto_historico = abs(var_percentual_historico * valor_portafolio)

print("--- 1. VaR por el Método Histórico (1 Día, 99%) ---")
print(f"Rendimiento en el 1% (VaR %): {var_percentual_historico*100:.2f}%")
print(f"VaR en Monto: ${var_monto_historico:,.2f}")
```

**Conclusión:** Este VaR Histórico te dice que, según la peor pérdida registrada en el **1% de los días en tu historia**, tu portafolio no perderá más del monto calculado.

-----

## 2\. VaR por el Método de Monte Carlo

El **Método de Monte Carlo** no usa el historial directo, sino que genera **miles de simulaciones** de lo que podría suceder en el futuro, basándose en el **riesgo y rendimiento estimados** de tu portafolio.

### El Proceso:

1.  Define el rendimiento promedio ($\mu$) y la volatilidad ($\sigma$) de tu portafolio.
2.  Genera un gran número (ej. 10,000) de **escenarios aleatorios** de rendimientos futuros.
3.  Calcula el valor del portafolio en cada escenario.
4.  Ordena los resultados y busca el peor percentil (igual que en el método histórico).

#### Implementación en Python (Simulación de 10,000 Escenarios)

```python
# Mantenemos los parámetros de riesgo del portafolio
media_rend = 0.0005  # Rendimiento promedio diario (0.05%)
volatilidad = 0.015  # Desviación estándar diaria (1.5%)
valor_portafolio = 100000.00 # $100,000

# 1. Parámetros de Monte Carlo
num_simulaciones = 10000 # Número de escenarios
nivel_confianza = 0.99   # 99%
percentil = 1 - nivel_confianza # El 1% peor

# 2. Generar 10,000 rendimientos aleatorios (usando la distribución normal)
# np.random.normal(localización, escala, tamaño)
simulaciones_rendimiento = np.random.normal(loc=media_rend, scale=volatilidad, size=num_simulaciones)

# 3. Calcular la PÉRDIDA/GANANCIA en cada escenario
resultados_pnl = simulaciones_rendimiento * valor_portafolio

# 4. Ordenar las pérdidas de menor (peor) a mayor (mejor)
resultados_ordenados = np.sort(resultados_pnl)

# 5. Encontrar el VaR (el percentil 1)
var_monto_montecarlo = abs(np.quantile(resultados_ordenados, percentil))

print("\n--- 2. VaR por el Método de Monte Carlo (1 Día, 99%) ---")
print(f"Número de Escenarios Simulados: {num_simulaciones}")
print(f"VaR en Monto: ${var_monto_montecarlo:,.2f}")
```

### Comparación Final:

| Característica | Histórico | Monte Carlo |
| :--- | :--- | :--- |
| **Ventaja** | Simple, no asume distribución. | Flexible, permite modelar escenarios complejos (ej. volatilidad agrupada con GARCH). |
| **Desventaja** | **"El pasado no garantiza el futuro"**. Ignora eventos que nunca han ocurrido. | Depende de la **calidad de las estimaciones** ($\mu$ y $\sigma$). Puede ser lento. |

Ambos métodos son valiosos, y el uso de Python te permite calcularlos rápidamente.

-----

¡Excelente\! Con esto, has dominado el cálculo del VaR. El siguiente paso en el Nivel Avanzado es ir más allá del VaR para medir la "cola" del riesgo.

**¿Listo para continuar con el Nivel Avanzado: Modelos de simulación para medir el riesgo de una cartera completa?** 😊
