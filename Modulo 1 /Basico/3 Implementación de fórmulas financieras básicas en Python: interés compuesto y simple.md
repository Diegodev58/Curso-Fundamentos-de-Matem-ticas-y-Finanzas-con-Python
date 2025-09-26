
-----

### Módulo 1: Fundamentos de Matemáticas y Finanzas con Python

#### Nivel Básico: Implementación de Interés Simple y Compuesto

### 1\. Interés Simple (El crecimiento más básico)

El **interés simple** es el más fácil de entender. Es una ganancia que se calcula solo sobre la cantidad original que invertiste o prestaste. El interés no se suma al capital para generar más interés; es una ganancia fija.

#### La Fórmula Matemática

La fórmula para calcular el interés simple es:

$$I = P \times r \times t$$

Donde:

  * $I$: Intereses ganados (lo que quieres calcular).
  * $P$: **Capital Principal** (el monto inicial invertido).
  * $r$: **Tasa de interés** (en formato decimal, por ejemplo, 5% es 0.05).
  * $t$: **Tiempo** (en años).

#### Implementación en Python

Vamos a calcular el interés simple que ganas al invertir $1,000 al 5% anual durante 3 años.

```python
# 1. Definimos las variables financieras
P = 1000      # Capital Principal (float, para permitir decimales)
r = 0.05      # Tasa de Interés Anual (5% en decimal)
t = 3         # Tiempo en años (int)

# 2. Implementamos la fórmula
interes_ganado = P * r * t

# 3. Calculamos el valor futuro (Capital + Intereses)
valor_futuro = P + interes_ganado

# 4. Imprimimos los resultados de forma clara
print("--- Cálculo de Interés Simple ---")
print(f"Capital Inicial (P): ${P}")
print(f"Tasa Anual (r): {r*100}%")
print(f"Interés Ganado (I): ${interes_ganado}")
print(f"Valor Futuro al final de {t} años: ${valor_futuro}")
# El interés es de $50 por año, por 3 años son $150. El total es $1,150.
```

-----

### 2\. Interés Compuesto (La "Octava Maravilla del Mundo")

El **interés compuesto** es la magia de las finanzas. Aquí, el interés que ganas se *suma* a tu capital principal, y luego, en el siguiente período, generas interés sobre la nueva cantidad (capital + intereses previos). ¡Tu dinero trabaja para ti\!

#### La Fórmula Matemática

La fórmula para calcular el **Valor Futuro (FV)** con interés compuesto es:

$$FV = P \times (1 + r)^t$$

Donde:

  * $FV$: **Valor Futuro** (el monto total que tendrás al final).
  * $P$: **Capital Principal** (el monto inicial invertido).
  * $r$: **Tasa de interés** (en formato decimal).
  * $t$: **Tiempo** (en años).
  * El operador `**` en Python se usa para las potencias (como el "elevado a").

#### Implementación en Python

Usemos los mismos datos: $1,000 al 5% anual durante 3 años.

```python
# 1. Usamos las mismas variables
P = 1000
r = 0.05
t = 3

# 2. Implementamos la fórmula de Valor Futuro (FV)
# Usamos el operador ** para la potencia
valor_futuro_compuesto = P * (1 + r)**t

# 3. Calculamos solo el interés ganado (opcional)
interes_ganado_compuesto = valor_futuro_compuesto - P

# 4. Imprimimos los resultados
print("\n--- Cálculo de Interés Compuesto ---")
print(f"Capital Inicial (P): ${P}")
print(f"Tasa Anual (r): {r*100}%")
print(f"Valor Futuro al final de {t} años: ${round(valor_futuro_compuesto, 2)}") # Redondeamos a 2 decimales
print(f"Interés Ganado (I): ${round(interes_ganado_compuesto, 2)}")
# El total es $1157.63, que es más que los $1150 del interés simple.
```

**Conclusión:** Viste que con el interés compuesto ganaste $7.63 más que con el simple. ¡Esto se conoce como el poder de la capitalización\! Saber programar estas fórmulas te permite automatizar decisiones de inversión.

