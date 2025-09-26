

### Módulo 1: Fundamentos de Matemáticas y Finanzas con Python

#### Nivel Básico: Variables y Tipos de Datos en Finanzas

Imagina que las **variables** son como "cajitas" o "etiquetas" en las que puedes guardar información. En lugar de escribir un número una y otra vez, le das un nombre, lo guardas en una variable, y luego llamas a ese nombre cada vez que lo necesites. Esto hace que tu código sea mucho más fácil de leer, de escribir y de modificar.

En Python, crear una variable es tan simple como darle un nombre y un valor:

```python
# El precio de una acción
precio_de_la_accion = 150.75

# La cantidad de acciones que tienes
cantidad_de_acciones = 10

# Tu nombre, que es un texto
nombre_inversionista = "Juan"
```

-----

#### Tipos de datos en el mundo financiero

En el mundo real, no todo son números. A veces, la información es texto, a veces son números con decimales, o números enteros. Los **tipos de datos** le dicen a Python qué tipo de información está guardando en una variable.

1.  **Enteros (`int`)**: Son números redondos, sin decimales. Los usarás para cosas como el número de acciones que tienes, la cantidad de años para un préstamo o el número de transacciones en un día.

      * **Ejemplo:** `cantidad_de_acciones = 100`

2.  **Flotantes (`float`)**: Son números con decimales. Los usarás para precios de acciones, tasas de interés o montos de dinero que no son exactos.

      * **Ejemplo:** `precio_de_bitcoin = 65000.50`

3.  **Cadenas de texto (`str`)**: Son textos. Los usarás para nombres de empresas, el tipo de activo (por ejemplo, "bono", "acción") o cualquier descripción. Deben ir entre comillas, ya sean simples (`'`) o dobles (`"`).

      * **Ejemplo:** `nombre_de_la_empresa = "Apple"`

4.  **Booleanos (`bool`)**: Son un tipo de dato que solo puede ser `True` (verdadero) o `False` (falso). Son útiles para preguntas de "sí o no", como si una inversión fue rentable o si el mercado está abierto.

      * **Ejemplo:** `es_rentable = True`

-----

#### ¿Por qué son tan importantes?

Las variables son esenciales porque te permiten crear un código flexible. Imagina que quieres calcular el valor total de tus acciones. En lugar de hacer esto:

```python
valor_total = 150.75 * 10
print(valor_total)
```

Puedes usar variables para que tu código se adapte a cualquier cambio. Si el precio de la acción o la cantidad que tienes cambia, solo necesitas modificar una línea de código.

```python
precio_de_la_accion = 150.75
cantidad_de_acciones = 10

valor_total = precio_de_la_accion * cantidad_de_acciones
print(valor_total)

# Ahora, si el precio cambia, ¡solo modificas una línea!
precio_de_la_accion = 160.00
valor_total = precio_de_la_accion * cantidad_de_acciones
print(valor_total)
```

En este ejemplo, Python entiende que `valor_total` debe ser un `float` porque está multiplicando un `float` por un `int`. La mayoría del tiempo, Python es muy inteligente y deduce el tipo de dato por sí mismo.
