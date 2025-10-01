
-----

## Módulo 5: Ciencia de Datos en Finanzas

#### Nivel Intermedio: Análisis de Sentimiento con Python

### ¿Qué es el Análisis de Sentimiento?

El **Análisis de Sentimiento** es una rama del Machine Learning (específicamente, Procesamiento de Lenguaje Natural o **NLP**). Su objetivo es determinar la **actitud emocional** detrás de un texto: ¿Es **positivo**, **negativo** o **neutral**?

  * **Analogía del Chisme:** Es como tener un detector de mentiras para las noticias. No leemos solo el titular ("Compañía X lanza nuevo producto"), sino que cuantificamos la reacción: ¿la gente lo ama (positivo) o lo odia (negativo)?

En finanzas, la hipótesis es simple: **el sentimiento positivo impulsa los precios al alza, y el sentimiento negativo los impulsa a la baja**.

### 1\. Herramienta Principal: VADER

Para un análisis de sentimiento inicial y rápido, usaremos una herramienta muy popular y sencilla llamada **VADER** (Valence Aware Dictionary and sEntiment Reasoner). VADER es especialmente buena con textos cortos y de redes sociales.

Para usar VADER, solo necesitas instalar la librería `vaderSentiment`: `pip install vaderSentiment`.

#### Implementación en Python con VADER

Vamos a analizar el sentimiento de tres "noticias" simuladas sobre la empresa ficticia "TechCorp".

```python
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
import pandas as pd

# Inicializamos el analizador de sentimiento
analizador = SentimentIntensityAnalyzer()

# El chisme que vamos a analizar (textos de ejemplo)
noticias = [
    "TechCorp reportó ganancias récord, superando todas las expectativas del mercado. ¡Un crecimiento increíble!",
    "La nueva línea de productos de TechCorp es decepcionante; la calidad no justifica el alto precio.",
    "TechCorp anunció una alianza menor con un proveedor de servicios, sin impacto significativo en sus acciones."
]

sentimientos_analizados = []

for texto in noticias:
    # 1. Obtenemos el puntaje del sentimiento
    puntaje = analizador.polarity_scores(texto)
    
    # 2. VADER devuelve varios puntajes, pero el 'compound' (compuesto) es el más útil:
    #   - Cerca de +1.0: Extremadamente Positivo
    #   - Cerca de 0.0: Neutral
    #   - Cerca de -1.0: Extremadamente Negativo
    
    sentimientos_analizados.append({
        'Noticia': texto[:40] + '...', # Acortamos la noticia para la tabla
        'Positivo': puntaje['pos'],
        'Negativo': puntaje['neg'],
        'Neutral': puntaje['neu'],
        'Compuesto': puntaje['compound']
    })

df_sentimiento = pd.DataFrame(sentimientos_analizados)

print("--- Análisis de Sentimiento de Noticias ---")
print(df_sentimiento)
```

### 2\. Interpretación de Resultados

| Noticia | Positivo | Negativo | Neutral | Compuesto |
| :--- | :--- | :--- | :--- | :--- |
| TechCorp reportó ganancias récord... | **0.421** | 0.000 | 0.579 | **0.8690** |
| La nueva línea de productos de Tec... | 0.000 | **0.312** | 0.688 | **-0.5719** |
| TechCorp anunció una alianza meno... | 0.000 | 0.000 | **1.000** | **0.0000** |

**Conclusión Financiera:**

1.  **Noticia 1 (Compuesto: +0.8690):** El sentimiento es fuertemente positivo. Esto podría ser un **predictor de que el precio de la acción subirá**.
2.  **Noticia 2 (Compuesto: -0.5719):** El sentimiento es negativo. El mercado probablemente **reaccionará vendiendo la acción**.
3.  **Noticia 3 (Compuesto: 0.0000):** El sentimiento es neutral. Es probable que **el precio no se mueva** por esta noticia.

### 3\. El Paso de Predicción (La Conexión Final)

En un modelo avanzado, tomarías el puntaje **Compuesto** (la columna clave) y lo usarías como una **variable de entrada ($X$)** adicional en tu modelo de Regresión Lineal o Clasificación (Módulo 5, Nivel Básico).

$$\text{Precio Futuro} = m_1(\text{Precio Ayer}) + m_2(\text{Volumen}) + \mathbf{m_3(\text{Sentimiento})} + b$$

Al incluir el factor humano (el sentimiento) en tu modelo, estás haciendo que la computadora sea un detective mucho más completo que solo mira los números fríos.

-----

¡Excelente trabajo\! Has analizado el sentimiento de noticias con Machine Learning.
