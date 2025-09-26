
-----

### Módulo 1: Fundamentos de Matemáticas y Finanzas con Python

#### Nivel Básico: Estructuras de datos para organizar información financiera

En finanzas, casi nunca trabajas con un solo número. Necesitas manejar series de precios, listas de empresas, o datos completos de un estado financiero. Aquí es donde entran las **listas** y los **diccionarios**.

### Listas: La "fila india" de los datos

Una **lista** es una colección ordenada de elementos. Imagina que es una fila de personas en el banco o una lista de precios de acciones a lo largo de los días. Los elementos pueden ser del mismo tipo (solo números) o de diferentes tipos. Se crean usando corchetes `[]` y los elementos se separan con comas.

  * **Aplicación financiera:** Puedes usar una lista para guardar una serie de precios de acciones diarios o semanales.

<!-- end list -->

```python
# Lista de precios de la acción de Apple durante una semana
precios_apple = [175.50, 178.25, 176.10, 179.80, 180.50]

# Puedes tener diferentes tipos de datos en la misma lista
info_empresa = ["Apple", "AAPL", 180.50, True]
```

#### Accediendo a los datos de una lista

Para encontrar un valor dentro de una lista, usas su **índice**, que es la posición que ocupa. En Python, los índices siempre empiezan en **cero (0)**, no en uno.

```python
# El primer precio de la lista (índice 0)
primer_precio = precios_apple[0]
print(primer_precio)
# Salida: 175.50

# El último precio (puedes usar índices negativos, -1 es el último)
ultimo_precio = precios_apple[-1]
print(ultimo_precio)
# Salida: 180.50
```

### Diccionarios: El "álbum de fotos" con etiquetas

Un **diccionario** es una colección de datos que no está ordenada por posición, sino por **pares de clave y valor**. Piensa en un diccionario de verdad, donde buscas una palabra (la clave) y te da su significado (el valor). En finanzas, es perfecto para guardar información de una empresa, donde la "clave" es el dato que buscas (`"nombre"`, `"precio"`) y el "valor" es la información (`"Apple"`, `180.50`). Se crean usando llaves `{}`.

  * **Aplicación financiera:** Ideal para guardar todos los datos de una sola entidad, como una empresa o un portafolio de inversión.

<!-- end list -->

```python
# Información detallada de una acción
datos_accion = {
    "empresa": "Apple Inc.",
    "ticker": "AAPL",
    "precio": 180.50,
    "capitalizacion_bursatil": 2.8e12, # 2.8 billones
    "mercado_abierto": True
}
```

#### Accediendo a los datos de un diccionario

Para encontrar un valor en un diccionario, no usas un índice numérico, sino la **clave** que le diste.

```python
# Obtener el precio de la acción usando su clave
precio_actual = datos_accion["precio"]
print(precio_actual)
# Salida: 180.50

# Cambiar un valor
datos_accion["mercado_abierto"] = False
print(datos_accion)
# Salida: {'empresa': 'Apple Inc.', 'ticker': 'AAPL', 'precio': 180.5, 'capitalizacion_bursatil': 2800000000000.0, 'mercado_abierto': False}
```

Las listas y los diccionarios son las herramientas básicas para organizar cualquier dato complejo. Un analista financiero que usa Python pasa gran parte de su tiempo trabajando con estas estructuras.

¿Entiendes la diferencia entre la lista (orden por posición) y el diccionario (orden por clave)? Una vez que te sientas cómodo con esto, podemos pasar al siguiente punto: **Implementación de fórmulas financieras básicas en Python: interés compuesto y simple**. 😊
