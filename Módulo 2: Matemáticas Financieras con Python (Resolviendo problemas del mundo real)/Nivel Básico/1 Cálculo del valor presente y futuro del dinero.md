
Este concepto es vital: **un euro hoy vale más que un euro mañana**. ¿Por qué? Por dos razones principales: puedes invertir el euro de hoy y ganar intereses, y por la inflación (los precios suben).

Aquí aprenderás a calcular dos métricas clave: el **Valor Futuro (FV)** y el **Valor Presente (PV)**.

-----

### Módulo 2: Matemáticas Financieras con Python

#### Nivel Básico: Cálculo del Valor Presente y Futuro del Dinero

Para todos estos cálculos, usaremos una herramienta muy útil de **NumPy** llamada `numpy_financial`. Si no la tienes, se instala así: `pip install numpy-financial`. Esta librería contiene funciones financieras preconstruidas que nos ahorran escribir fórmulas complejas.

### 1\. Valor Futuro (Future Value, FV)

El **Valor Futuro (FV)** te dice cuánto valdrá una inversión hoy después de un período de tiempo, asumiendo una tasa de interés constante. Es la pregunta: "Si invierto $1,000 hoy, ¿cuánto tendré en 5 años?"

#### La Fórmula Matemática

$$FV = PV \times (1 + r)^n$$

Donde:

  * $FV$: Valor Futuro (lo que quieres calcular).
  * $PV$: Valor Presente (la inversión inicial).
  * $r$: Tasa de Interés por período.
  * $n$: Número de períodos (años).

#### Implementación en Python con `np.fv()`

Usaremos la función `np.fv()` (future value). Los argumentos clave son:

1.  `rate` (tasa de interés).
2.  `nper` (número de períodos).
3.  `pmt` (pago periódico; en este caso, es 0 porque solo hacemos una inversión inicial).
4.  `pv` (valor presente; debe ser un **número negativo** porque representa una salida de dinero o una inversión).

**Ejemplo:** Inviertes $5,000 a una tasa anual del 8% durante 10 años.

```python
import numpy_financial as npf

# Variables
PV = -5000  # Inversión inicial (negativo = salida de dinero)
r = 0.08    # Tasa anual (8%)
n = 10      # Número de años

# Cálculo del Valor Futuro
FV = npf.fv(rate=r, nper=n, pmt=0, pv=PV)

print("--- 1. Cálculo del Valor Futuro (FV) ---")
print(f"Inversión Inicial: ${-PV:.2f}")
print(f"Valor Futuro después de {n} años: ${FV:.2f}")

# Conclusión: Tus $5,000 se convertirán en $10,794.62.
```

-----

### 2\. Valor Presente (Present Value, PV)

El **Valor Presente (PV)** te dice cuánto tienes que invertir hoy para alcanzar una meta futura. Es la pregunta: "¿Cuánto debo invertir hoy para tener $10,000 en 5 años?"

#### La Fórmula Matemática

El Valor Presente es simplemente la fórmula anterior despejada:

$$PV = \frac{FV}{(1 + r)^n}$$

#### Implementación en Python con `np.pv()`

Usaremos la función `np.pv()` (present value). Los argumentos clave son:

1.  `rate` (tasa de interés).
2.  `nper` (número de períodos).
3.  `pmt` (pago periódico; en este caso, es 0).
4.  `fv` (valor futuro; el dinero que queremos alcanzar).

**Ejemplo:** Quieres tener $10,000 en 5 años. La tasa de interés que esperas ganar es del 6% anual.

```python
# Variables
FV = 10000  # Meta de dinero futura
r = 0.06    # Tasa anual esperada (6%)
n = 5       # Número de años

# Cálculo del Valor Presente
PV = npf.pv(rate=r, nper=n, pmt=0, fv=FV)

print("\n--- 2. Cálculo del Valor Presente (PV) ---")
print(f"Meta en el Futuro: ${FV:.2f}")
print(f"Cantidad a Invertir Hoy: ${-PV:.2f}") # El resultado es negativo, lo mostramos positivo.

# Conclusión: Necesitas invertir $7,472.58 hoy para tener $10,000 en 5 años al 6%.
```

Entender y calcular el VDT es el primer paso para valorar proyectos, activos y tomar decisiones de inversión.

