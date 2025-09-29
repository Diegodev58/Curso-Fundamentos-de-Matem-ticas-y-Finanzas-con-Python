
-----

### Módulo 3: Fundamentos de Cálculo y Optimización

#### Nivel Intermedio: Integrales y Acumulación de Rendimientos

La **Integral** es, esencialmente, una suma. Se utiliza para calcular el **área bajo una curva**. En términos financieros, esto es muy útil porque nos permite:

1.  **Calcular el Capital Acumulado:** Sumar pequeños pagos a lo largo del tiempo.
2.  **Determinar el Rendimiento Total:** Sumar las pequeñas ganancias (o pérdidas) instantáneas para obtener el rendimiento total o acumulado de un activo.

### 1\. El Concepto Básico: Suma vs. Integral

Imagina que estás invirtiendo. Si haces un aporte mensual, sumas los 12 aportes. Pero, ¿qué pasa si tu inversión crece de manera **continua** e **instantánea** (como el interés compuesto continuo)? Ahí es donde entra la integral.

| Herramienta | Función | Concepto Financiero |
| :--- | :--- | :--- |
| **Suma ($\Sigma$)** | Suma de valores **discretos** (separados). | Cálculo de intereses simples anuales o pagos mensuales fijos. |
| **Integral ($\int$)** | Suma de valores **continuos** (instantáneos). | Cálculo de **rendimiento acumulado** o interés compuesto continuo. |

### 2\. Aplicación Financiera: Rendimiento Acumulado Continuo

En el mundo real, los mercados se mueven cada segundo. Si tienes una función que describe la tasa de rendimiento instantánea de una inversión ($R(t)$), la integral te dirá cuál es el rendimiento total acumulado entre dos puntos de tiempo.

$$Rendimiento\ Acumulado = \int_{t_1}^{t_2} R(t) dt$$

Usaremos **SymPy** para el cálculo simbólico y **SciPy** para el cálculo numérico (que es más útil con datos reales).

#### Implementación con SymPy (Cálculo Simbólico)

Vamos a definir una función simple de rendimiento continuo, por ejemplo, que el rendimiento de tu inversión crece linealmente con el tiempo:

$$R(t) = 0.10 + 0.02t$$

Esto significa que empiezas con un rendimiento del 10% y aumenta un 2% cada año. Queremos saber el rendimiento total acumulado en los primeros 5 años (de $t=0$ a $t=5$).

```python
import sympy as sp

# 1. Definimos la variable simbólica 't' (tiempo)
t = sp.symbols('t')

# 2. Definimos la función de Rendimiento Instantáneo R(t)
R_t = 0.10 + 0.02 * t

# 3. Calculamos la Integral (Antiderivada)
integral_R_t = sp.integrate(R_t, t)
# Salida: 0.01*t**2 + 0.1*t

# 4. Calculamos la Integral Definida (Rendimiento Acumulado de 0 a 5)
rendimiento_acumulado = sp.integrate(R_t, (t, 0, 5))

print("--- Cálculo Simbólico del Rendimiento Acumulado ---")
print(f"La Integral Indefinida es: {integral_R_t}")
print(f"El Rendimiento Acumulado en los primeros 5 años es: {rendimiento_acumulado:.4f}")
# El resultado es 0.75 (o 75% de rendimiento acumulado)
```

### 3\. Aplicación con SciPy (Cálculo Numérico)

En la práctica, no siempre tienes una fórmula $R(t)$ perfecta. Simplemente tienes una serie de datos (por ejemplo, rendimientos diarios). Para estos casos, usamos la **Integración Numérica** con SciPy, que es mucho más robusta.

**Caso de Estudio:** Queremos calcular el rendimiento total acumulado durante 10 días, donde la tasa de rendimiento diario fue variando.

```python
import numpy as np
from scipy.integrate import trapz # Usamos la regla del trapecio para sumar el área

# Simulamos la tasa de rendimiento instantánea R(t) a lo largo de 10 días
tiempo = np.arange(1, 11) # Días 1 a 10
tasa_rendimiento = 0.005 + 0.001 * tiempo # Una tasa que crece un poco cada día

# La función trapz() calcula el área bajo la curva uniendo los puntos con trapecios
rendimiento_acumulado_numerico = trapz(tasa_rendimiento, tiempo)

print("\n--- Cálculo Numérico del Rendimiento Acumulado (SciPy) ---")
print(f"Tasa de Rendimiento en 10 Días: {tasa_rendimiento.round(4)}")
print(f"Rendimiento Acumulado Total (Integral Numérica): {rendimiento_acumulado_numerico:.4f}")

# Conclusión: La integral numérica nos da la suma total de las pequeñas ganancias diarias.
```

La integral te permite modelar sistemas donde el cambio es constante, como la valoración continua de derivados o el cálculo de la acumulación de riesgos.

¿Entendido cómo la integral "suma" las pequeñas partes continuas? Podemos pasar al siguiente nivel y ver la joya de la corona del Módulo 3: **Optimización básica con Python para encontrar el mejor valor en una función financiera**. 😊
