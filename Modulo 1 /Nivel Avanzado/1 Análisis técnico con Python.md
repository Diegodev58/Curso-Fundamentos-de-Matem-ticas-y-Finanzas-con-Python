
-----

### Módulo 1: Fundamentos de Matemáticas y Finanzas con Python

#### Nivel Avanzado: Análisis Técnico con Python

### 1\. Promedio Móvil (MA): Suavizando la Tendencia

El **Promedio Móvil (MA)** es el indicador más básico y potente. Es simplemente el promedio de los precios de un activo durante un número específico de períodos (días, semanas, etc.). Su propósito es **suavizar** las fluctuaciones diarias para mostrar la **verdadera tendencia** subyacente.

  * **Aplicación financiera:** Un MA de 50 días muestra la tendencia a mediano plazo; uno de 200 días muestra la tendencia a largo plazo.

#### Cálculo con Pandas

Gracias a Pandas, calcular un promedio móvil es sorprendentemente fácil usando la función `rolling()`.

```python
import pandas as pd
# Creamos un DataFrame de ejemplo (usamos los precios del punto anterior)
data = {
    'Precio_Cierre': [100.0, 101.5, 102.0, 100.5, 103.0, 105.0, 106.0, 104.5, 107.0, 108.5]
}
df_precios = pd.DataFrame(data)

# 1. Definimos el periodo del promedio (ventana)
ventana = 3  # Un Promedio Móvil de 3 días

# 2. Usamos .rolling() para tomar una "ventana móvil" de 3 días
# y luego calculamos la media (.mean()) de esos 3 días.
df_precios['MA_3_dias'] = df_precios['Precio_Cierre'].rolling(window=ventana).mean()

print("DataFrame con Promedio Móvil (MA):")
print(df_precios)
```

**Interpretación del Resultado:**

  * Verás que los primeros dos días tienen un valor **NaN** (Not a Number), porque no hay suficientes datos para calcular un promedio de 3 días.
  * El valor en el día 3 (índice 2) es el promedio de `100.0`, `101.5` y `102.0`.

#### Señales de Trading con MA

Una señal de compra o venta muy común ocurre cuando el precio cruza el MA:

  * **Señal de Compra (Tendencia Alcista):** Cuando el precio de cierre cruza *por encima* del Promedio Móvil.
  * **Señal de Venta (Tendencia Bajista):** Cuando el precio de cierre cruza *por debajo* del Promedio Móvil.

-----

### 2\. Índice de Fuerza Relativa (RSI): Midiendo el Impulso

El **RSI** es un oscilador que mide la **velocidad y el cambio de los movimientos de los precios**. Te indica si un activo está **sobrecomprado** (demasiada gente comprando, el precio podría bajar pronto) o **sobrevendido** (demasiada gente vendiendo, el precio podría subir pronto).

  * **Rango:** El RSI se mueve entre 0 y 100.
  * **Zona de Sobrecompra:** Generalmente, si el RSI está **por encima de 70**.
  * **Zona de Sobrevendido:** Generalmente, si el RSI está **por debajo de 30**.

#### Cálculo con Python (Librería Técnica)

Calcular el RSI es matemáticamente más complejo que el MA (implica promedios de ganancias y pérdidas), por lo que en lugar de escribir toda la fórmula desde cero, usamos librerías especializadas como **TA-Lib** o, de forma más sencilla, la librería **`pandas_ta`** para el propósito de nuestro curso.

```python
# ¡Asumiendo que ya tienes instalado 'pandas_ta' (pip install pandas-ta)!
import pandas_ta as ta

# Calculamos el RSI de 14 períodos (el estándar)
# La función ta.rsi() toma la columna de precios y la ventana de cálculo
df_precios.ta.rsi(close='Precio_Cierre', length=14, append=True)

# Mostramos el DataFrame con la nueva columna 'RSI_14'
print("\nDataFrame con Índice de Fuerza Relativa (RSI):")
print(df_precios.tail()) # Mostramos solo las últimas filas, donde el RSI ya se calculó
```

#### Interpretación de Resultados

Una vez que tienes la columna del RSI, puedes analizar las señales:

| Valor del RSI | Interpretación | Implicación de Trading |
| :--- | :--- | :--- |
| **\> 70** | **Sobrecomprado** | El precio puede estar *demasiado* alto; podría haber un retroceso. |
| **\< 30** | **Sobrevendido** | El precio puede estar *demasiado* bajo; podría haber un rebote. |

**Conclusión:** Usando la potencia de Python, puedes integrar estos indicadores directamente en tus modelos de datos, en lugar de solo mirarlos en una plataforma de trading. Esto te prepara para el *trading* algorítmico.

