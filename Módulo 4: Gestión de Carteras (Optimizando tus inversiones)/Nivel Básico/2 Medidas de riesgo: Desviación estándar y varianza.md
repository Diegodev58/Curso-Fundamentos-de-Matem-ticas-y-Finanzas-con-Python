
La **Varianza ($\sigma^2$)** y la **Desviación Estándar ($\sigma$)** son las dos medidas estadísticas que nos dicen qué tan "dispersos" están los rendimientos de un portafolio respecto a su promedio. Es decir, nos dicen qué tan **volátil (riesgosa)** es tu inversión.

-----

### Módulo 4: Gestión de Carteras

#### Nivel Intermedio: Medidas de Riesgo (Desviación Estándar y Varianza)

### 1\. Varianza ($\sigma^2$): El Origen del Riesgo

La **Varianza** es el promedio de las diferencias al cuadrado de cada rendimiento respecto al rendimiento promedio. Si un activo tiene una varianza alta, sus rendimientos son muy inestables (pueden ser muy altos o muy bajos).

  * **Problema:** La varianza está expresada en unidades al cuadrado (por ejemplo, $\$^2$), lo que la hace difícil de interpretar en términos de dinero real.

### 2\. Desviación Estándar ($\sigma$): El Riesgo en Dinero

La **Desviación Estándar** es simplemente la raíz cuadrada de la Varianza. Al hacer esto, volvemos a la unidad original (porcentaje), lo que la convierte en la medida de riesgo más utilizada en finanzas.

  * **Interpretación:** Si tu portafolio tiene un rendimiento promedio del 10% y una desviación estándar del 5%, es muy probable (alrededor del 68% de las veces) que tu rendimiento real se ubique entre el 5% (10% - 5%) y el 15% (10% + 5%).

### 3\. Cálculo del Riesgo de un Portafolio

Para calcular el riesgo de un portafolio de dos activos (A y B) con ponderaciones ($w_A$ y $w_B$), la fórmula de la varianza del portafolio ($\sigma_p^2$) es la siguiente. ¡Fíjate cómo incluye la correlación\!

$$\sigma_p^2 = w_A^2\sigma_A^2 + w_B^2\sigma_B^2 + 2w_A w_B \rho_{AB} \sigma_A \sigma_B$$

Donde:

  * $\sigma_A^2, \sigma_B^2$: Varianza de los Activos A y B.
  * $\rho_{AB}$: Correlación entre los Activos A y B.

**Esta fórmula es la razón matemática de la diversificación.** Si la $\rho_{AB}$ es baja (o negativa), el término final se reduce o resta, **disminuyendo el riesgo total del portafolio** sin sacrificar tanto rendimiento.

#### Implementación en Python con NumPy

Vamos a usar NumPy para calcular el riesgo de un portafolio compuesto por:

  * **Activo A:** 50% de la cartera.
  * **Activo B:** 50% de la cartera.

<!-- end list -->

```python
import numpy as np

# Variables de Entrada (Valores Anualizados Simulados)
w_A = 0.50          # Ponderación del Activo A (50%)
w_B = 0.50          # Ponderación del Activo B (50%)
sigma_A = 0.10      # Volatilidad (Riesgo) de A (10%)
sigma_B = 0.20      # Volatilidad (Riesgo) de B (20%)
rho_AB = 0.30       # Correlación entre A y B (0.30)

# 1. Cálculo de la Varianza del Portafolio (la fórmula completa)
var_p = (w_A**2 * sigma_A**2) + \
        (w_B**2 * sigma_B**2) + \
        (2 * w_A * w_B * rho_AB * sigma_A * sigma_B)

# 2. Cálculo de la Desviación Estándar del Portafolio (Riesgo)
sigma_p = np.sqrt(var_p)

# 3. Cálculo del Riesgo Ponderado Simple (SUMA DE RIESGOS INDIVIDUALES)
# Esto es lo que NO pasa en un portafolio diversificado
riesgo_simple = (w_A * sigma_A) + (w_B * sigma_B)

print("--- 1. Cálculo del Riesgo del Portafolio ---")
print(f"Riesgo de A: {sigma_A*100:.2f}% | Riesgo de B: {sigma_B*100:.2f}%")
print(f"Riesgo (Desviación Estándar) del Portafolio: {sigma_p*100:.2f}%")
print(f"Suma Simple de Riesgos (No diversificado): {riesgo_simple*100:.2f}%")
```

**Conclusión:**

  * Si solo hubieras promediado los riesgos individuales (riesgo simple), habrías esperado un riesgo del 15% (0.5\*10% + 0.5\*20%).
  * Sin embargo, gracias a la **diversificación** (la baja correlación de 0.30), el riesgo real del portafolio ($\sigma_p$) es de **12.45%**.

Acabas de probar matemáticamente que la diversificación reduce el riesgo total. Este concepto es la base para el siguiente paso: encontrar las ponderaciones perfectas en la **Frontera Eficiente**.

¿Listo para usar la optimización que aprendiste en el Módulo 3 para construir la **Frontera Eficiente** de verdad? 😊
