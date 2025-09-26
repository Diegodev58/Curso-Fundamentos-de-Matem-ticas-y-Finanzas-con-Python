
Una anualidad es simplemente una serie de **pagos fijos** realizados a intervalos regulares (mensuales, trimestrales, anuales) durante un período de tiempo.

-----

### Módulo 2: Matemáticas Financieras con Python

#### Nivel Básico: Cálculo de Anualidades para Pagos Fijos (PMT) y Valor Futuro (FV)

Usaremos nuevamente la librería `numpy_financial` para calcular los pagos y los montos futuros de estas anualidades.

### 1\. Cálculo del Pago Fijo de un Préstamo (PMT)

El **Pago Fijo (PMT)** te dice cuánto debes pagar periódicamente para liquidar un préstamo.

  * **La Pregunta:** "Si pido prestados $20,000 al 5% de interés, y tengo 4 años para pagar, ¿cuánto debo pagar cada mes?"

#### Implementación en Python con `npf.pmt()`

Para usar la función `npf.pmt()` (payment), necesitamos adaptar las variables a la frecuencia de pago (mensual en este caso).

**Ejemplo:** Préstamo de $20,000, 5% anual, a pagar en 4 años (48 meses).

```python
import numpy_financial as npf

# Variables del Préstamo
PV = 20000      # Valor Presente (monto del préstamo)
r_anual = 0.05  # Tasa Anual (5%)
n_años = 4      # Duración en años

# Adaptación a Pagos Mensuales (la clave en anualidades):
r_mensual = r_anual / 12  # Tasa mensual
n_periodos = n_años * 12  # Total de meses (48)

# El Valor Futuro (FV) de un préstamo es 0, porque queremos que quede liquidado.

# Cálculo del Pago Mensual (PMT)
# El PV debe ir negativo porque es dinero que recibes y que luego debes devolver.
pago_mensual = npf.pmt(rate=r_mensual, nper=n_periodos, pv=-PV, fv=0)

print("--- 1. Cálculo del Pago Fijo Mensual (PMT) ---")
print(f"Monto del Préstamo: ${PV:.2f}")
print(f"Número de Pagos (Meses): {n_periodos}")
print(f"Pago Mensual Requerido: ${pago_mensual:.2f}")

# Conclusión: Debes pagar $460.97 cada mes para liquidar el préstamo en 4 años.
```

### 2\. Cálculo del Valor Futuro de un Ahorro (Anualidad de Ahorro)

El **Valor Futuro (FV)** de una anualidad de ahorro te dice cuánto dinero acumularás si haces una contribución fija periódicamente.

  * **La Pregunta:** "Si ahorro $100 cada mes y mi inversión rinde 7% anual, ¿cuánto tendré en 15 años?"

#### Implementación en Python con `npf.fv()`

Usamos la misma función `npf.fv()` que ya conoces, pero esta vez, el argumento `pmt` (pago) es el que tiene el valor de la contribución mensual.

**Ejemplo:** Ahorro de $100 mensuales, 7% anual, durante 15 años.

```python
# Variables del Ahorro
PV = 0          # Valor Presente (Empezamos sin dinero inicial)
pmt = -100      # Aporte Mensual (negativo = salida de dinero para ahorrar)
r_anual = 0.07  # Tasa Anual (7%)
n_años = 15     # Duración en años

# Adaptación a Aportaciones Mensuales:
r_mensual = r_anual / 12
n_periodos = n_años * 12

# Cálculo del Valor Futuro (FV)
valor_futuro_ahorro = npf.fv(rate=r_mensual, nper=n_periodos, pmt=pmt, pv=PV)

print("\n--- 2. Cálculo del Valor Futuro del Ahorro ---")
print(f"Aporte Mensual: ${-pmt:.2f}")
print(f"Total Años de Ahorro: {n_años}")
print(f"Valor Acumulado al Final: ${valor_futuro_ahorro:.2f}")

# Conclusión: Al final de 15 años, habrías acumulado $30,958.82.
# (Solo aportaste $18,000 en total, el resto son intereses compuestos ¡Magia!)
```

Entender las anualidades y programar sus cálculos te permite evaluar préstamos, comparar opciones de financiación y planificar tu jubilación con precisión.

