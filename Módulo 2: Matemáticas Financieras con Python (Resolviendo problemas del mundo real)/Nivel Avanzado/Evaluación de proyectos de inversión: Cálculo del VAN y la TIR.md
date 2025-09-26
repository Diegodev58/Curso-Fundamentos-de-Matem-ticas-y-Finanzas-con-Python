¡Estupendo\! Estás a punto de dominar las herramientas más importantes para la toma de decisiones financieras en el ámbito corporativo: el **Valor Actual Neto (VAN)** y la **Tasa Interna de Retorno (TIR)**. Estos indicadores te dicen si un proyecto de inversión es viable y rentable.

-----

### Módulo 2: Matemáticas Financieras con Python

#### Nivel Avanzado: Evaluación de Proyectos de Inversión (VAN y TIR)

Ambos métodos se basan en el concepto de Valor del Dinero en el Tiempo (VDT). Para esto, usaremos la librería `numpy_financial` (`npf`).

### 1\. El Valor Actual Neto (VAN o NPV)

El **VAN (Net Present Value, NPV)** te dice, en dólares de hoy, si un proyecto generará más valor que el costo de inversión. Es la suma de todos los flujos de caja futuros (ingresos y gastos) del proyecto, descontados a valor presente, menos la inversión inicial.

  * **La Regla de Decisión:**
      * **VAN \> 0:** Acepta el proyecto. Generará valor.
      * **VAN = 0:** Indiferente. El proyecto solo cubre el costo del capital.
      * **VAN \< 0:** Rechaza el proyecto. Destruirá valor.

#### La Fórmula Matemática

$$VAN = \sum_{t=1}^{n} \frac{FC_t}{(1 + r)^t} - I_0$$

Donde:

  * $FC_t$: Flujo de Caja en el período $t$.
  * $r$: Tasa de descuento o costo de capital.
  * $I_0$: Inversión inicial (el flujo de caja del momento 0).

#### Implementación en Python con `npf.npv()`

La función `npf.npv()` hace el trabajo pesado por ti. Solo necesita la **tasa de descuento** y una **lista de los flujos de caja**.

**Caso de Estudio:** Una inversión de $50,000. Los flujos de caja esperados para los próximos 4 años son: Año 1: $10,000; Año 2: $15,000; Año 3: $20,000; Año 4: $30,000. El costo de capital (tasa de descuento) es del 7%.

```python
import numpy_financial as npf

# Variables
tasa_descuento = 0.07 # Costo de capital (7%)
inversion_inicial = -50000 # La inversión es una salida de dinero (negativo)
flujos_futuros = [10000, 15000, 20000, 30000]

# 1. Creamos la lista completa de flujos de caja, incluyendo la inversión inicial (período 0)
flujos_de_caja = [inversion_inicial] + flujos_futuros

# 2. Cálculo del VAN (NPV)
VAN = npf.npv(rate=tasa_descuento, values=flujos_de_caja)

print("--- 1. Cálculo del Valor Actual Neto (VAN) ---")
print(f"Flujos de Caja (incl. Inversión): {flujos_de_caja}")
print(f"Costo de Capital: {tasa_descuento*100:.0f}%")
print(f"Valor Actual Neto (VAN): ${VAN:.2f}")

# Conclusión: Como el VAN es $8,771.51 (mayor que 0), el proyecto es rentable.
```

-----

### 2\. La Tasa Interna de Retorno (TIR o IRR)

La **TIR (Internal Rate of Return, IRR)** es la tasa de rendimiento que genera el propio proyecto. Es la tasa de descuento a la que el **VAN se hace exactamente cero**. Te da la rentabilidad del proyecto como un porcentaje.

  * **La Regla de Decisión:**
      * **TIR \> Costo de Capital:** Acepta el proyecto. El proyecto rinde más de lo que nos cuesta financiarlo.
      * **TIR \< Costo de Capital:** Rechaza el proyecto.

#### La Fórmula Matemática

Para encontrar la TIR, tienes que resolver $0 = \sum_{t=1}^{n} \frac{FC_t}{(1 + TIR)^t} - I_0$. Esto requiere una iteración matemática, pero Python lo hace por ti.

#### Implementación en Python con `npf.irr()`

La función `npf.irr()` (internal rate of return) requiere solo la **lista completa de los flujos de caja**, incluyendo la inversión inicial.

```python
# Usamos la misma lista de flujos de caja:
# flujos_de_caja = [-50000, 10000, 15000, 20000, 30000]

# Cálculo de la TIR (IRR)
TIR = npf.irr(values=flujos_de_caja)

print("\n--- 2. Cálculo de la Tasa Interna de Retorno (TIR) ---")
print(f"Tasa Interna de Retorno (TIR): {TIR*100:.2f}%")
print(f"Costo de Capital (r): 7.00%")

# Conclusión: La TIR (12.44%) es mucho mayor que el costo de capital (7%),
# lo que confirma la decisión: el proyecto es muy rentable.
```

Ambos indicadores te dan una visión completa para evaluar la viabilidad de cualquier proyecto, desde la compra de una máquina nueva hasta la expansión de un negocio.

