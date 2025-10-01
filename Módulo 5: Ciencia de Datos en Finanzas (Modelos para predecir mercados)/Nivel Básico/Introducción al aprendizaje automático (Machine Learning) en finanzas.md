
---

## Módulo 5: Aprendizaje Automático (Machine Learning) en Finanzas

#### Nivel Básico: Introducción al Machine Learning

### ¿Qué es el Aprendizaje Automático (ML)?

Imagina que eres un detective que intenta predecir si tu amigo Juan va a llegar tarde a la fiesta de mañana. Tú sabes que:

1.  **Si hay tráfico,** llega tarde.
2.  **Si es lunes,** casi siempre llega tarde.
3.  **Si tiene que pasar por el café nuevo,** se distrae y llega tarde.

El **Machine Learning** es exactamente ese proceso: tomas un montón de datos históricos (los "chismes" de las tardanzas de Juan), y en lugar de hacer la predicción tú mismo, **le enseñas a la computadora a encontrar las reglas y patrones**.

* **Tú (el Humano):** Tardas horas analizando los datos.
* **La Computadora (con ML):** Analiza **millones** de tardanzas en segundos y te dice con un 95% de precisión si Juan llegará a tiempo.

En finanzas, la computadora "aprende" a predecir si el precio de una acción va a subir, si un cliente va a pagar un préstamo, o qué inversión es la más riesgosa.

### El Proceso de "Aprendizaje" del ML

El aprendizaje automático sigue tres pasos sencillos:

1.  **Datos (El Chisme):** Le das a la computadora una tabla gigante con información del pasado.
    * *Ejemplo Financiero:* Precios históricos, volumen de negociación, informes de ganancias, noticias económicas.
2.  **Entrenamiento (La Escuela):** Usas un **algoritmo** (una receta matemática, como la Regresión Lineal) para que la computadora relacione los datos de entrada con la respuesta que buscas.
    * *Meta:* Que aprenda que "si la tasa de interés sube $\rightarrow$ las acciones bancarias caen".
3.  **Predicción (La Bola de Cristal):** Una vez "entrenada", le das datos nuevos y te da una predicción.
    * *Resultado:* "Basado en la historia, la probabilidad de que esta acción suba mañana es del 68%."

### ML en Finanzas: La Analogía del Noviazgo

El uso más popular del ML en finanzas es la **predicción**, y funciona como cuando intentas saber si una nueva relación funcionará.

| Elemento de ML | Analogía del Noviazgo | Tarea Financiera |
| :--- | :--- | :--- |
| **Datos de Entrada (Features)** | El historial de citas de tu amiga: "fue detallista", "respondió rápido", "se rieron mucho". | Precios de apertura, el RSI, el volumen de negociación, noticias. |
| **Etiqueta (Target)** | Si la relación **funcionó** o **no funcionó** (la respuesta correcta). | Si el precio de la acción **subió** o **bajó** (la respuesta histórica). |
| **Modelo (Algoritmo)** | La "regla" que tu amiga usa para elegir pareja. | **Regresión Lineal**, Árboles de Decisión, etc. |
| **Predicción** | "Con 80% de probabilidad, esta nueva persona será una buena pareja." | "Con 80% de probabilidad, el precio de cierre de mañana será de \$150." |

**¡El chiste es que el modelo de ML construye la regla por sí mismo!** No tienes que decirle las fórmulas; él las encuentra.

---

Ahora que entendemos la introducción y la gran idea detrás del Machine Learning, estamos listos para atacar el algoritmo más simple y fundamental de todos: la **Regresión Lineal**.

¿Listo para ver cómo la Regresión Lineal se convierte en tu primera herramienta de predicción de precios? 😊
