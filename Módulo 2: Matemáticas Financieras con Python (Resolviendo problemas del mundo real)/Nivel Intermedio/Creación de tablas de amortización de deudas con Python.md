
Con Python, podemos generar esta tabla de forma automática, lo que es mucho más rápido y menos propenso a errores que usar una hoja de cálculo manual.

-----

### Módulo 2: Matemáticas Financieras con Python

#### Nivel Intermedio: Creación de Tablas de Amortización de Deudas

Para este ejemplo, usaremos el préstamo que calculamos en el punto anterior ($20,000, 5% anual, 4 años) y la función `npf.ipmt()` (intereses) y `npf.ppmt()` (capital principal) de la librería `numpy_financial`.

### 1\. Preparación de Variables y el Pago Fijo (PMT)

Primero, necesitamos definir todas las variables y calcular el pago fijo mensual, tal como lo hicimos en el tema anterior.

```python
import numpy_financial as npf
import pandas as pd

# Variables del Préstamo
PV = 20000      # Monto total del préstamo
r_anual = 0.05  # Tasa Anual (5%)
n_años = 4      # Duración en años

# Adaptación Mensual
r_mensual = r_anual / 12     # Tasa mensual
n_periodos = n_años * 12     # Total de pagos (48 meses)

# Cálculo del Pago Fijo Mensual (PMT)
# Usamos -PV para que el PMT de la función salga positivo
pago_mensual = npf.pmt(rate=r_mensual, nper=n_periodos, pv=-PV, fv=0)
print(f"Pago Fijo Mensual: ${pago_mensual:.2f}") # $460.97
```

### 2\. Generando los Componentes del Préstamo con NumPy

Ahora, el "secreto" de la automatización en Python está en usar el poder de NumPy para calcular los 48 valores de golpe.

  * `npf.ipmt()`: Calcula el componente de **intereses** para cada período.
  * `npf.ppmt()`: Calcula el componente de **capital principal** para cada período.

Necesitamos crear un arreglo de períodos (del 1 al 48) para decirle a las funciones qué períodos queremos calcular:

```python
# Creamos una lista de períodos (desde 1 hasta n_periodos)
periodos = np.arange(1, n_periodos + 1)

# Cálculo de Intereses Pagados en CADA Período
# La función requiere que el PV esté en negativo
interes_cada_pago = npf.ipmt(rate=r_mensual, per=periodos, nper=n_periodos, pv=-PV, fv=0)

# Cálculo de Capital Pagado en CADA Período
capital_cada_pago = npf.ppmt(rate=r_mensual, per=periodos, nper=n_periodos, pv=-PV, fv=0)
```

### 3\. Construcción y Análisis de la Tabla de Amortización

Usaremos **Pandas** para organizar estos números en una tabla clara. Necesitamos una columna adicional para el **Saldo Pendiente** (la deuda que queda).

```python
# 1. Creamos el DataFrame
tabla_amortizacion = pd.DataFrame({
    'Pago': periodos,
    'Monto_Pago': pago_mensual,
    'Intereses_Pagados': interes_cada_pago,
    'Capital_Pagado': capital_cada_pago
})

# 2. Corregimos los signos (los valores de NumPy salen negativos, los queremos positivos)
tabla_amortizacion['Intereses_Pagados'] = tabla_amortizacion['Intereses_Pagados'] * -1
tabla_amortizacion['Capital_Pagado'] = tabla_amortizacion['Capital_Pagado'] * -1

# 3. Calculamos el Saldo Pendiente (el más complejo)
# El saldo es el monto inicial menos el capital acumulado que se ha pagado hasta ese punto.
tabla_amortizacion['Capital_Acumulado'] = tabla_amortizacion['Capital_Pagado'].cumsum()
tabla_amortizacion['Saldo_Pendiente'] = PV - tabla_amortizacion['Capital_Acumulado']

# 4. Ajuste final (limpiamos el DataFrame para mostrar)
tabla_final = tabla_amortizacion[['Pago', 'Monto_Pago', 'Capital_Pagado', 'Intereses_Pagados', 'Saldo_Pendiente']]
tabla_final = tabla_final.round(2) # Redondeamos a 2 decimales para la moneda
```

**Análisis de las Primeras Filas:**

| Pago | Monto\_Pago | Capital\_Pagado | Intereses\_Pagados | Saldo\_Pendiente |
| :---: | :---: | :---: | :---: | :---: |
| 1 | 460.97 | 377.64 | 83.33 | 19622.36 |
| 2 | 460.97 | 379.21 | 81.76 | 19243.15 |
| 3 | 460.97 | 380.79 | 80.18 | 18862.36 |

**La Revelación:**

Al observar la tabla, entiendes el concepto clave de la amortización:

  * **Al principio del préstamo (Pago 1):** Una gran parte de tu pago de $460.97 se destina a **Intereses ($83.33)\*\*, y una parte menor al **Capital ($377.64)**.
  * **A medida que avanzas:** Tu **Saldo Pendiente** disminuye, por lo que la porción de **Intereses** que pagas también disminuye, mientras que la porción de **Capital** aumenta.

Este es un análisis esencial para cualquier préstamo, ¡y lo has programado en Python\!

