
-----

### Módulo 2: Matemáticas Financieras con Python

#### Nivel Intermedio: Valoración de Bonos y Acciones

### 1\. Valoración de Bonos (Deuda a Descuento)

Un **bono** es un préstamo que tú le das a una entidad (un gobierno o una empresa). Ellos te prometen devolverte el principal (el valor nominal) en una fecha futura y pagarte intereses fijos periódicos, llamados **cupones**.

El precio justo de un bono es la suma de los **valores presentes** de todos los flujos de caja futuros (los cupones y el valor nominal). Esto se llama **Valoración por Descuento de Flujos de Caja (DCF)**.

  * **Caso de Estudio:** Un bono con un valor nominal de $1,000, paga un cupón anual del 5%, y madura en 5 años. Si los inversores en el mercado exigen un rendimiento del 6% (tasa de descuento), ¿cuál es el precio justo del bono hoy?

#### Implementación con Python

Usaremos la función `npf.pv()` (Valor Presente) para cada flujo de caja y luego sumaremos los resultados.

```python
import numpy as np
import numpy_financial as npf

# Variables del Bono
Valor_Nominal = 1000  # Lo que te devolverán al final (FV)
Tasa_Cupon = 0.05     # 5% de cupón anual
Tasa_Descuento = 0.06 # 6% de rendimiento exigido por el mercado (r)
Años_Maturacion = 5   # Períodos (n)

# 1. Cálculo del pago anual del cupón
Pago_Cupon = Valor_Nominal * Tasa_Cupon # $50 al año

# 2. Valoración de los Cupones (una anualidad)
# Usamos npf.pv() donde FV es 0 y PMT es el cupón
PV_Cupones = npf.pv(rate=Tasa_Descuento, nper=Años_Maturacion, pmt=Pago_Cupon, fv=0)

# 3. Valoración del Valor Nominal (un solo pago futuro)
# Usamos npf.pv() donde PMT es 0 y FV es el Valor Nominal
PV_Nominal = npf.pv(rate=Tasa_Descuento, nper=Años_Maturacion, pmt=0, fv=Valor_Nominal)

# 4. Precio Justo del Bono (suma de los valores presentes)
Precio_Justo_Bono = - (PV_Cupones + PV_Nominal) # Multiplicamos por -1 para obtener un precio positivo

print("--- 1. Valoración de un Bono ---")
print(f"PV de los Cupones: ${-PV_Cupones:.2f}")
print(f"PV del Valor Nominal: ${-PV_Nominal:.2f}")
print(f"Precio Justo del Bono Hoy: ${Precio_Justo_Bono:.2f}")

# Conclusión: Como el mercado exige más rendimiento (6%) que lo que paga el bono (5%),
# el precio justo es menor al valor nominal de $1,000. El bono se vende "con descuento".
```

-----

### 2\. Valoración de Acciones (Flujo de Caja a Descuento - DCF)

Valorar una **acción** es más complejo porque el flujo de caja (dividendos) no es fijo y la vida de la empresa es, teóricamente, infinita. El método más robusto es el **Modelo de Descuento de Dividendos (DDM)**, un tipo de DCF.

Asumiremos el modelo más simple, el **Modelo de Crecimiento Constante de Gordon**.

  * **La Fórmula de Gordon:** El precio justo de una acción es el dividendo del próximo año ($D_1$) dividido por la diferencia entre la tasa de rendimiento requerida ($r$) y la tasa de crecimiento esperada de los dividendos ($g$).

$$P_0 = \frac{D_1}{r - g}$$

  * **Caso de Estudio:** Una acción paga un dividendo de $2.00 este año ($D\_0$). Se espera que crezca al 4% ($g$). Los inversores requieren un rendimiento del 10% ($r$).

#### Implementación con Python

```python
# Variables de la Acción
D_cero = 2.00  # Dividendo pagado este año (D0)
r = 0.10      # Tasa de rendimiento requerida (10%)
g = 0.04      # Tasa de crecimiento constante (4%)

# 1. Calcular el Dividendo del Próximo Año (D1)
D_uno = D_cero * (1 + g)

# 2. Aplicar la Fórmula de Gordon
Precio_Justo_Accion = D_uno / (r - g)

print("\n--- 2. Valoración de una Acción (Modelo de Gordon) ---")
print(f"Dividendo Proyectado (D1): ${D_uno:.2f}")
print(f"Tasa Requerida (r): {r*100:.0f}%")
print(f"Tasa de Crecimiento (g): {g*100:.0f}%")
print(f"Precio Justo de la Acción Hoy: ${Precio_Justo_Accion:.2f}")

# Conclusión: Si la acción se vende en el mercado a un precio menor a $33.33,
# la consideraríamos subvaluada y una buena compra.
```

Dominar estas técnicas de valoración es esencial para cualquier analista. Te permite comparar el **precio de mercado** con el **valor intrínseco** (el precio justo).

