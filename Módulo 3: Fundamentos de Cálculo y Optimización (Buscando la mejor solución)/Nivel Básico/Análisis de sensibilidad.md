
En nuestro curso, la función $P(t) = 0.5t^2 - 4t + 10$ es un **modelo matemático simplificado** que usamos con fines **didácticos** para ilustrar el concepto de la derivada.

-----

## Origen de los Datos en la Vida Real

En un análisis financiero real, esta función $P(t)$ no se inventa; se **deriva** de los datos históricos del mercado.

### 1\. Modelado de Series de Tiempo

En la práctica, los datos de los precios de las acciones ($P$) a lo largo del tiempo ($t$) son una **serie de tiempo**. La relación no suele ser una fórmula cuadrática perfecta, sino una línea dentada y compleja.

Para poder aplicar el cálculo y predecir o analizar tendencias, los analistas y científicos de datos hacen lo siguiente:

1.  **Recopilan Datos Históricos:** Obtienen miles de puntos de datos de precios.
2.  **Ajuste de Curva (Curve Fitting):** Usan técnicas estadísticas como la **Regresión** (que verás en el Módulo 5) para encontrar una función matemática que se "ajuste" lo mejor posible a los datos históricos.
      * **Ejemplo:** Si los precios han seguido una tendencia en forma de "U" (cayeron y luego subieron), una función cuadrática como $P(t) = at^2 + bt + c$ podría ser un buen **modelo de aproximación**.

### 2\. El Propósito del Ejemplo Didáctico

En nuestro caso, usamos $P(t) = 0.5t^2 - 4t + 10$ porque tiene una forma perfecta (una parábola) para ilustrar cómo la derivada ($\frac{dP}{dt}$) nos da la pendiente y, lo más importante, cómo encontrar ese punto donde la pendiente es **cero** ($t=4$):

  * **Punto Mínimo:** El punto donde la caída del precio se detiene y comienza el rebote.

**En resumen:** Piensa en $P(t) = 0.5t^2 - 4t + 10$ como un experimento de laboratorio. En el laboratorio real, tú mismo usarás Python (Módulo 5) para **crear** estas funciones matemáticas a partir de los datos brutos del mercado.

-----

## Vamos con Análisis de Sensibilidad

Ahora que aclaramos de dónde vienen las funciones, continuemos con el siguiente tema. El **Análisis de Sensibilidad** es una aplicación directa del concepto de la tasa de cambio que acabamos de ver.

### Módulo 3: Fundamentos de Cálculo y Optimización

#### Nivel Básico: Análisis de Sensibilidad

El **Análisis de Sensibilidad** es una herramienta de gestión de riesgos que te permite entender cómo pequeños cambios en una variable de entrada (por ejemplo, el costo de las materias primas o la tasa de interés) afectan a la variable de salida más importante (por ejemplo, la ganancia o el VAN de un proyecto).

**¿Qué pregunta responde?** "Si mi costo de capital aumenta del 7% al 8%, ¿qué tan sensible es mi ganancia a ese cambio?"

### 1\. El Papel de la Derivada (Matemáticas)

En este contexto, la derivada se llama **sensibilidad** o **elasticidad**.

Si tenemos una Ganancia ($G$) que depende de la Tasa de Interés ($r$), la derivada $\frac{dG}{dr}$ te dice:

$$\text{Sensibilidad} = \frac{\text{Cambio en la Ganancia}}{\text{Cambio en la Tasa de Interés}}$$

  * **Valor Alto:** La ganancia es muy sensible; un pequeño cambio en la tasa tiene un gran impacto.
  * **Valor Bajo:** La ganancia es poco sensible; la tasa podría cambiar mucho sin afectar gravemente el resultado.

### 2\. Aplicación a un Proyecto de Inversión (VAN)

Recordemos el cálculo del **Valor Actual Neto (VAN)** (Módulo 2). El VAN de un proyecto depende de la **Tasa de Descuento ($r$)**.

$$VAN = \sum_{t=1}^{n} \frac{FC_t}{(1 + r)^t} - I_0$$

Vamos a usar la librería **SciPy** (Scientific Python) para calcular numéricamente la sensibilidad del VAN a los cambios en la tasa de descuento.

**Caso de Estudio:** Usaremos los flujos de caja del Módulo 2 para ver qué tan sensible es el VAN a la tasa de descuento.

```python
import numpy_financial as npf
from scipy.optimize import approx_fprime # Herramienta para calcular la derivada numérica

# Datos del proyecto (tomados del Módulo 2)
inversion_inicial = -50000
flujos_futuros = [10000, 15000, 20000, 30000]
flujos_de_caja = [inversion_inicial] + flujos_futuros

# 1. Definimos una función que calcula el VAN (NPV)
def calcular_van(tasa_descuento):
    # La función npf.npv requiere la tasa y los flujos
    return npf.npv(rate=tasa_descuento, values=flujos_de_caja)

# 2. Definimos el punto inicial para el cálculo (la tasa actual)
tasa_actual = 0.07 # 7%

# 3. Calculamos la Sensibilidad (la derivada)
# approx_fprime(x, f, epsilon)
#   x: el punto donde queremos calcular (la tasa actual)
#   f: la función que queremos derivar (calcular_van)
#   epsilon: el "pequeño cambio" (h) para calcular la pendiente
sensibilidad_van = approx_fprime(np.array([tasa_actual]), calcular_van, 1e-6)

print("--- Análisis de Sensibilidad del VAN ---")
print(f"VAN original a 7%: ${calcular_van(tasa_actual):.2f}")
print(f"Sensibilidad del VAN a la Tasa de Descuento (dVAN/dr): {sensibilidad_van[0]:.2f}")
```

**Interpretación del Resultado (Ejemplo de Salida):**

Si la sensibilidad calculada es, por ejemplo, **-120,000**, significa que:

  * Por cada **1%** de **aumento** en la tasa de descuento ($r$), el **VAN** del proyecto **caerá** aproximadamente **$1,200** (es decir, $-120,000 \times 0.01$).

Este número te dice qué tan riesgoso es el proyecto a cambios en la tasa de interés.

