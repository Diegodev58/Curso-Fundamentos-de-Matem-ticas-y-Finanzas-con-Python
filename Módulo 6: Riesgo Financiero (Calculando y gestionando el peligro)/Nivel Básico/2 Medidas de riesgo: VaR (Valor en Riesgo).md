
-----

## Módulo 6: Riesgo Financiero

#### Nivel Básico: Medidas de Riesgo: Valor en Riesgo (VaR)

### 1\. ¿Qué es el Valor en Riesgo (VaR)?

El **Valor en Riesgo (VaR)** no es otra cosa que la **pérdida máxima estimada** que una inversión o portafolio podría sufrir en un **horizonte temporal** específico, con una **probabilidad** o nivel de confianza determinado.

Piensa en el VaR como el **límite de peligro**.

| Componente del VaR | Significado | Ejemplo |
| :--- | :--- | :--- |
| **Pérdida Máxima Estimada** | La cantidad de dinero que esperas no perder. | **$1,000** |
| **Horizonte Temporal** | El período que analizas. | **En el próximo día** |
| **Nivel de Confianza (Probabilidad)** | La certeza que tienes de que la pérdida no será mayor. | Con una certeza del **99%** |

**Lectura Completa del Ejemplo:** "Hay solo un **1%** de probabilidad de que el portafolio pierda más de **$1,000** en el transcurso del **próximo día**." (O, con un 99% de confianza, la pérdida máxima no excederá los $1,000).

### La Analogía del Chisme: El Pronóstico del Dinero 💰

Imagina que estás planeando un viaje y tienes un presupuesto de emergencia.

  * Si tu VaR a 1 semana al 95% es de **$500**, significa que, en el 95% de las semanas, no perderás más de $500.
  * En el restante **5%** de las semanas (los días de **crisis o eventos extremos**), la pérdida **podría exceder los $500**. Es tu advertencia de turbulencia.

-----

### 2\. Cálculo Básico del VaR (Método Paramétrico o Delta-Normal)

El método más simple para calcular el VaR asume que los rendimientos de tu portafolio siguen una **distribución normal** (la curva de campana que vimos en el Módulo 1).

**Fórmula (Básica):**

$$\text{VaR} = \text{Valor del Portafolio} \times \text{Volatilidad} \times \text{Factor Z}$$

Donde:

  * **Volatilidad ($\sigma$):** La desviación estándar de los rendimientos del portafolio (Módulo 4).
  * **Factor Z:** Un valor estadístico que depende del nivel de confianza (se obtiene de la tabla de la distribución normal).

| Nivel de Confianza | Factor Z (Aprox.) |
| :---: | :---: |
| **95%** | **1.645** |
| **99%** | **2.33** |

#### Implementación en Python con NumPy

Usaremos NumPy para simular un portafolio y calcular el VaR al 99% en un horizonte de 1 día.

```python
import numpy as np
from scipy.stats import norm # Para obtener el Factor Z (Z-score)

# --- 1. Datos del Portafolio Simulados (Basados en el Módulo 4) ---
valor_portafolio = 1000000.00  # $1,000,000.00
rendimiento_diario_promedio = 0.0003 # 0.03% (Cercano a cero en VaR, a veces se ignora)
volatilidad_diaria = 0.015       # 1.5% de Desviación Estándar Diaria

# --- 2. Definición de Parámetros de Riesgo ---
nivel_confianza = 0.99  # 99%
factor_Z = norm.ppf(nivel_confianza) # Función que encuentra el Z para el 99% (aprox. 2.33)

# --- 3. Cálculo del VaR (Método Delta-Normal) ---
# Usamos el concepto de pérdida (la Volatilidad * Factor Z)
var_porcentaje = volatilidad_diaria * factor_Z
var_monto = valor_portafolio * var_porcentaje

print("--- Cálculo del VaR al 99% (1 Día) ---")
print(f"Factor Z para el 99%: {factor_Z:.3f}")
print(f"VaR Porcentual: {var_porcentaje*100:.2f}%")
print(f"VaR en Monto: ${var_monto:,.2f}")
```

**Interpretación del Resultado (Ejemplo):**

Si el cálculo da un VaR de **$34,950**, significa que solo hay un **1%** de probabilidad de que tu portafolio de $1 millón pierda más de **$34,950\*\* en el próximo día.

### Limitación del VaR

El VaR es fácil de entender, pero tiene una gran limitación: **No te dice CUÁNTO podrías perder si la pérdida excede el VaR**. Solo te da el límite. Por eso, en el Nivel Avanzado, conocerás el *Expected Shortfall*, que es una métrica complementaria.

-----

¡Excelente\! Has aprendido a cuantificar el riesgo de mercado con el VaR paramétrico.
