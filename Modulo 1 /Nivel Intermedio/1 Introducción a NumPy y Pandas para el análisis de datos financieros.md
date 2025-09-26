 ¡Entras en el territorio de las herramientas pesadas\! 🚀

Con **NumPy** y **Pandas**, dejarás de trabajar con datos sueltos y empezarás a analizar grandes volúmenes de información como un profesional. Son las dos librerías más importantes para cualquier tarea de análisis de datos y finanzas en Python.

-----

### Módulo 1: Fundamentos de Matemáticas y Finanzas con Python

#### Nivel Intermedio: Introducción a NumPy y Pandas

### 1\. NumPy: El Músculo para los Números

**NumPy** (Numerical Python) es la librería que proporciona soporte para trabajar con **arreglos** (arrays) y matrices. Piensa en un arreglo como una lista de Python súper cargada y optimizada para la velocidad.

  * **¿Por qué es crucial en finanzas?**
      * **Velocidad:** NumPy es mucho más rápido que las listas de Python para hacer cálculos con grandes cantidades de datos. Esto es vital cuando analizas miles de precios de acciones o simulas escenarios.
      * **Operaciones Vectorizadas:** Permite aplicar una operación matemática (suma, multiplicación, potencia) a **todos** los elementos de un arreglo a la vez, sin usar bucles complejos.

#### La creación de Arreglos (Arrays)

Para usar NumPy, primero debemos "importarlo" con el apodo `np`.

```python
import numpy as np

# Lista de rendimientos diarios (porcentajes de ganancia/pérdida)
rendimientos_lista = [0.005, -0.012, 0.021, -0.008, 0.015]

# Convertir la lista normal de Python en un Arreglo de NumPy
rendimientos_array = np.array(rendimientos_lista)

print(rendimientos_array)
# Salida: [ 0.005 -0.012  0.021 -0.008  0.015]
```

#### Operaciones Mágicas (Vectorizadas)

Imagina que quieres duplicar todos los rendimientos. Con una lista normal sería complicado; con NumPy, es trivial:

```python
# Duplicamos todos los rendimientos a la vez
rendimientos_dobles = rendimientos_array * 2
print(rendimientos_dobles)
# Salida: [ 0.010 -0.024  0.042 -0.016  0.030]

# También puedes calcular la suma o el promedio de forma instantánea
suma_rendimientos = np.sum(rendimientos_array)
promedio_rendimientos = np.mean(rendimientos_array)

print(f"\nSuma de Rendimientos: {suma_rendimientos}")
print(f"Promedio de Rendimientos: {promedio_rendimientos}")
```

-----

### 2\. Pandas: La Herramienta de Tablas (DataFrames)

**Pandas** es la navaja suiza del análisis de datos. Su estructura principal es el **DataFrame**, que es esencialmente una tabla, como la que usarías en Excel o una base de datos. Tiene filas y columnas, y es perfecto para organizar y etiquetar datos.

  * **¿Por qué es crucial en finanzas?**
      * **Organización:** Los datos financieros (precios, fechas, volúmenes) son inherentemente tabulares. Pandas te permite cargarlos y visualizarlos de forma clara.
      * **Limpieza y Etiquetado:** Puedes nombrar las columnas (`"Precio de Cierre"`, `"Fecha"`) y manejar fechas y datos faltantes fácilmente.

#### La creación de un DataFrame

Para usar Pandas, lo importamos con el apodo `pd`.

```python
import pandas as pd

# Datos financieros que queremos organizar
data = {
    'Precio_Cierre': [150.00, 151.20, 149.80, 152.50],
    'Volumen': [120000, 150000, 95000, 180000]
}
# Las fechas serán las etiquetas (índice) de nuestras filas
fechas = ['2024-01-01', '2024-01-02', '2024-01-03', '2024-01-04']

# Creamos el DataFrame
df_acciones = pd.DataFrame(data, index=fechas)

print("--- Nuestro primer DataFrame de Precios ---")
print(df_acciones)
```

#### Accediendo y Analizando Datos

Con Pandas, las operaciones se vuelven muy intuitivas:

```python
# Ver solo los precios de cierre
precios = df_acciones['Precio_Cierre']
print("\nSolo la columna de precios:")
print(precios)

# Calcular estadísticas rápidas para una columna (el mínimo y el máximo)
precio_maximo = df_acciones['Precio_Cierre'].max()
precio_minimo = df_acciones['Precio_Cierre'].min()

print(f"\nPrecio Máximo: {precio_maximo}")
print(f"Precio Mínimo: {precio_minimo}")
```

**Resumen:**

  * **NumPy** es para cuando necesitas hacer **cálculos matemáticos rápidos** sobre grandes colecciones de números.
  * **Pandas** es para **organizar, etiquetar y manipular** datos financieros en formato de tabla (DataFrame).

Ambas librerías trabajan de la mano. A menudo, usarás Pandas para cargar y organizar los precios históricos, y luego NumPy para calcular la desviación estándar o la volatilidad sobre esos precios.
