 Ya tienes la idea de que el Machine Learning es como enseñarle a un detective a encontrar patrones. Ahora, veremos las dos formas principales en que ese detective (nuestro algoritmo) hace predicciones: **Regresión** y **Clasificación**.

---

## 1. Regresión: Prediciendo un Número Exacto 📈

La **Regresión** es el modelo que usas cuando quieres predecir una **cantidad** o un **valor continuo**. Piensa en ella como una línea que intenta pasar lo más cerca posible de todos los puntos de datos.

### Analogía: El Precio de un Apartamento

Imagina que quieres predecir el precio exacto de un apartamento. La regresión considera varios factores (metros cuadrados, número de habitaciones, distancia al metro) y te da una **salida numérica específica**: "\$150,500.00".

| Componente | Explicación | Aplicación Financiera |
| :--- | :--- | :--- |
| **Objetivo** | Predecir un **valor continuo**. | El **precio exacto de una acción** de mañana, el monto de la deuda de un cliente, el VAN de un proyecto. |
| **Salida** | Un **número** (ej. 150.50). | El algoritmo traza una línea (o curva) para encontrar el mejor ajuste. |
| **Modelo Más Básico** | **Regresión Lineal Simple** (lo que veremos en el siguiente punto). | |

---

## 2. Clasificación: Prediciendo una Categoría o Etiqueta 🏷️

La **Clasificación** es el modelo que usas cuando quieres predecir una **categoría** o una **etiqueta**. Aquí, la respuesta no es un número, sino una de varias opciones fijas.

### Analogía: ¿Compro o Vendo?

En lugar de preguntar por el precio exacto de la acción (Regresión), preguntas: "¿El precio de la acción mañana va a **subir** o **bajar**?". La clasificación te da una de esas dos etiquetas.

| Componente | Explicación | Aplicación Financiera |
| :--- | :--- | :--- |
| **Objetivo** | Predecir una **etiqueta discreta**. | Si una acción subirá o bajará, si un cliente pagará o incumplirá un préstamo (fraude o no fraude). |
| **Salida** | Una **categoría** (ej. "Subir", "Bajar"). | El algoritmo dibuja límites para separar los datos en grupos. |
| **Modelo Más Básico** | **Regresión Logística** o **Árboles de Decisión**. | |

---

## El Chisme: Diferencia Clave en el Trading 🕵️‍♀️

En el mundo del trading, a menudo se usa la **Clasificación** en lugar de la **Regresión**, y hay una razón chismosa detrás:

* **Regresión es muy difícil:** Predecir que el precio exacto de una acción será **\$150.23** mañana es casi imposible. El error más pequeño te puede costar dinero.
* **Clasificación es más fácil:** Es mucho más sencillo predecir la **dirección** (sube o baja) que la magnitud exacta. Muchos *quants* prefieren saber la probabilidad de que una acción suba (ej. 75%) y luego apuestan por la etiqueta "Subir", en lugar de buscar el precio perfecto.

| Pregunta de Trading | Tipo de Modelo |
| :--- | :--- |
| ¿Cuál será el **valor** de la acción *X* en 3 días? | **Regresión** (Difícil) |
| ¿El precio de la acción *X* estará **por encima o por debajo** del precio de hoy en 3 días? | **Clasificación** (Más práctico) |

---

## Enfoque: La Regresión Lineal

Para el siguiente punto, nos centraremos en la **Regresión Lineal** porque es el cimiento de todos los modelos de Machine Learning. Es la forma más fácil de entender cómo una computadora aprende a predecir un valor.

¿Estás listo para programar y usar la **Regresión Lineal Simple para predecir precios**? 😊
