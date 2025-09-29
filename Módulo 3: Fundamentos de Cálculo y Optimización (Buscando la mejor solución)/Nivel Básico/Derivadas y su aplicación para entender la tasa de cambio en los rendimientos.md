

### Módulo 3: Fundamentos de Cálculo y Optimización

#### Nivel Básico: Derivadas y Tasa de Cambio

La **Derivada** es la herramienta clave del cálculo. En términos sencillos, la derivada te dice la **tasa de cambio instantánea** de una función. Es la **pendiente** de la curva en un punto específico.

Piensa en esto:

  * Si la función es la **distancia** que recorre un coche, la derivada es la **velocidad** del coche.
  * Si la función es el **precio de una acción ($P$ en función del tiempo $t$ )**, la derivada ($\frac{dP}{dt}$) es la **velocidad** a la que el precio sube o baja.

### 1\. Concepto Básico: La Pendiente y el Crecimiento

Una pendiente te indica si algo está subiendo, bajando o manteniéndose estable:

| Derivada (Pendiente) | Significado | Implicación Financiera |
| :--- | :--- | :--- |
| **Positiva (\> 0)** | La función está subiendo. | El precio (o rendimiento) está **creciendo**. |
| **Cero (= 0)** | La función está plana. | El precio ha alcanzado un **máximo o mínimo** temporal. |
| **Negativa (\< 0)** | La función está bajando. | El precio (o rendimiento) está **decayendo**. |

### 2\. Aplicación Financiera: Tasa de Cambio en los Rendimientos

En finanzas, usamos la derivada para medir:

1.  **Velocidad del Cambio de Precio:** ¿La acción está subiendo lentamente o se está disparando?
2.  **Puntos Críticos:** Identificar el punto máximo de un precio antes de que comience a caer (pico) o el punto mínimo antes de que comience a subir (valle).

Para programar derivadas, usamos la librería **SymPy**, que permite hacer **Cálculo Simbólico** (trabajar con ecuaciones y letras, no solo números).

#### Implementación con SymPy

Vamos a definir una función simple que simule el precio de una acción a lo largo del tiempo:

$$P(t) = 0.5t^2 - 4t + 10$$

```python
import sympy as sp

# 1. Definimos la variable simbólica 't' (tiempo)
t = sp.symbols('t')

# 2. Definimos la función de precio P(t)
P_t = 0.5 * t**2 - 4 * t + 10

# 3. Calculamos la Derivada de P(t) con respecto a 't'
# La derivada nos da la TASA DE CAMBIO del precio
dP_dt = sp.diff(P_t, t)

print("--- 1. La Función de Precio P(t) ---")
print(P_t)
print("\n--- 2. La Derivada (Tasa de Cambio) dP/dt ---")
print(dP_dt)
# Salida: 1.0*t - 4
```

### 3\. Usando la Derivada para Análisis

Ahora, la derivada es $1.0t - 4$. Podemos evaluarla en diferentes momentos del tiempo ($t$):

| Momento ($t$) | Tasa de Cambio (Sustituir $t$ en $1.0t - 4$) | Interpretación |
| :---: | :---: | :--- |
| **t = 1** | $1(1) - 4 = \mathbf{-3.0}$ | El precio está cayendo rápidamente. |
| **t = 4** | $1(4) - 4 = \mathbf{0.0}$ | **Punto Crítico\!** El precio ha tocado fondo (mínimo) y está a punto de empezar a subir. |
| **t = 7** | $1(7) - 4 = \mathbf{3.0}$ | El precio está subiendo rápidamente. |

En el momento $t=4$, la derivada es cero. Esto significa que la pendiente es plana y el precio está en un punto de **inflexión** (en este caso, un mínimo). **Este es el poder de la derivada: te ayuda a encontrar los puntos exactos de máximo o mínimo rendimiento.**

¿Te parece claro cómo la derivada nos da la "velocidad" y nos ayuda a encontrar los puntos críticos en los precios? Si es así, podemos pasar a **Análisis de sensibilidad**. 😊
