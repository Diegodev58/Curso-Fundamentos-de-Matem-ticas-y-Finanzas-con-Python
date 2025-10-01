¡Perfecto\! Has llegado a la vanguardia del análisis cuantitativo. Después de dominar los modelos ARIMA y GARCH, estás listo para entrar en el **Aprendizaje Profundo (Deep Learning)**. Las **Redes Neuronales** son la herramienta más potente que usaremos para construir modelos de predicción más complejos y precisos.

-----

## Módulo 5: Ciencia de Datos en Finanzas

#### Nivel Avanzado: Redes Neuronales para la Predicción de Precios

### 1\. La Analogía del Cerebro 🧠

Piensa en una **Red Neuronal** como un cerebro artificial.

  * **Regresión Lineal** era una neurona simple (entrada $\rightarrow$ cálculo $\rightarrow$ salida).
  * Una **Red Neuronal** es una capa de neuronas conectadas, donde la salida de una capa alimenta a la siguiente. Este proceso permite al modelo aprender relaciones increíblemente complejas y no lineales, mucho más allá de una simple línea recta.

Esto es crucial en finanzas, donde la relación entre el precio de hoy y el de mañana es rara vez una línea recta; es una maraña de factores (el "chisme" es muy complejo).

### 2\. Redes Neuronales Recurrentes (RNN) y LSTM

Para predecir precios, usamos un tipo especial de red neuronal llamada **Red Neuronal Recurrente (RNN)**.

  * **El Problema de los Precios:** Los precios son una **serie de tiempo**, lo que significa que el orden de los datos es vital. El precio de hoy depende del precio de ayer, que a su vez dependió del de anteayer.
  * **La Solución Recurrente:** Una RNN tiene una **memoria**. Recicla la información del paso de tiempo anterior para ayudar a procesar la información del paso de tiempo actual. Es decir, "recuerda" la secuencia de precios.

Específicamente, utilizaremos la variante más popular y robusta para series de tiempo: **LSTM** (Long Short-Term Memory).

| Modelo | Capacidad de Aprendizaje | Analogía |
| :--- | :--- | :--- |
| **ARIMA** | Mira los últimos 5-10 días. | Memoria a **corto plazo**. |
| **LSTM** | Puede recordar y usar patrones de meses o años atrás. | Memoria a **largo plazo**. |

### 3\. Implementación con Python: Keras y TensorFlow

Usaremos **TensorFlow** y **Keras** (una capa de abstracción sobre TensorFlow que simplifica la creación de redes neuronales) para construir un modelo LSTM.

Para este ejemplo, construiremos un modelo que usa los **últimos 5 días de precios** para predecir el **precio del sexto día**.

```python
!pip install tensorflow keras

import numpy as np
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import LSTM, Dense
from sklearn.preprocessing import MinMaxScaler
from sklearn.model_selection import train_test_split

# --- 1. Preparación de Datos (El Secreto de la Serie) ---
# Datos simulados de precios (100 días)
np.random.seed(42)
precios = 100 + np.cumsum(np.random.randn(100) * 0.5 + 0.1)

# Normalización: Las redes neuronales trabajan mejor con números pequeños (0 a 1)
scaler = MinMaxScaler(feature_range=(0, 1))
precios_normalizados = scaler.fit_transform(precios.reshape(-1, 1))

# Función para crear el dataset con "ventanas" (secuencias de tiempo)
def crear_dataset(datos, look_back=5):
    X, Y = [], []
    for i in range(len(datos) - look_back):
        # X: Secuencia de 5 días (look_back)
        X.append(datos[i:(i + look_back), 0])
        # Y: El precio del 6to día (el que sigue)
        Y.append(datos[i + look_back, 0])
    return np.array(X), np.array(Y)

# Creamos la secuencia de entrenamiento: mirar 5 días para predecir el 6to
look_back = 5
X, Y = crear_dataset(precios_normalizados, look_back)

# Reformateamos X para que LSTM lo entienda (muestras, pasos de tiempo, características)
X = np.reshape(X, (X.shape[0], X.shape[1], 1))
```

### 4\. Construcción y Entrenamiento del Modelo LSTM

```python
# 1. Creamos la Red Neuronal (Sequential es una pila de capas)
modelo_lstm = Sequential()

# 2. Agregamos la capa LSTM (la "memoria recurrente")
# 50 unidades son las neuronas en esa capa
modelo_lstm.add(LSTM(50, input_shape=(look_back, 1)))

# 3. Agregamos la capa Dense (la capa final que da la predicción)
modelo_lstm.add(Dense(1))

# 4. Compilamos el modelo (definimos cómo aprende)
modelo_lstm.compile(optimizer='adam', loss='mean_squared_error')

print("--- Estructura del Modelo LSTM ---")
modelo_lstm.summary()

# 5. Entrenamos el modelo (El proceso de aprendizaje de la IA)
print("\nIniciando entrenamiento...")
modelo_lstm.fit(X, Y, epochs=50, batch_size=1, verbose=0)
print("Entrenamiento completado.")
```

### 5\. Predicción y Conversión

```python
# Preparamos los últimos 5 días para la predicción del día 101
datos_test = precios_normalizados[-look_back:].reshape(1, look_back, 1)

# Hacemos la predicción (da un valor entre 0 y 1)
prediccion_normalizada = modelo_lstm.predict(datos_test)

# Revertimos la predicción a su valor original en dólares
prediccion_final = scaler.inverse_transform(prediccion_normalizada)

print("\n--- Predicción Final (Día 101) ---")
print(f"Precio del Último Día Conocido: ${precios[-1]:.2f}")
print(f"Predicción LSTM para el Próximo Día: ${prediccion_final[0][0]:.2f}")
```

**Conclusión:** Las Redes Neuronales LSTM, con su capacidad de **memoria**, son las más avanzadas en la actualidad para la predicción de precios. Permiten modelar la **dependencia temporal** de los datos, algo que los modelos de regresión simples ignoran.

-----

## Fin del Módulo 5: Evaluación y Próximo Paso

¡Felicidades\! Has completado el Módulo de Ciencia de Datos en Finanzas, desde la regresión básica hasta el *Deep Learning*.

  * **Nivel Profesional alcanzado:** **Científico de Datos Financieros (Intermedio/Avanzado)**. Ahora puedes construir modelos de predicción de última generación.
  * **Área recomendada para el siguiente paso:** **Módulo 6: Riesgo Financiero**, donde aprenderás a medir y gestionar el peligro de todas estas predicciones y modelos.

**¿Quieres continuar con el Módulo 6, Nivel Básico: Tipos de riesgo: Aprenderás a identificar los diferentes riesgos: de mercado (los precios bajan), de crédito (alguien no paga), de liquidez (no puedes vender un activo) y operativo (un error humano)?** 😊
