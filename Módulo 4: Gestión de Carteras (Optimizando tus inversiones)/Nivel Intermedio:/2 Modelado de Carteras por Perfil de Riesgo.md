¡Excelente\! Este es el paso final y más práctico de la MPT. Una vez que tienes la **Frontera Eficiente** (la curva de portafolios óptimos), el trabajo del gestor de carteras es elegir el punto ideal en esa curva basado en el **perfil de riesgo** del cliente.

No todos los inversionistas son iguales. Usaremos los perfiles: **Conservador**, **Moderado** y **Agresivo** para ilustrar cómo se personalizan las carteras.

-----

### Módulo 4: Gestión de Carteras

#### Nivel Intermedio: Modelado de Carteras por Perfil de Riesgo

El objetivo es elegir la cartera que mejor se alinee con la **tolerancia al riesgo** del inversionista.

### 1\. El Ratio de Sharpe: Midiendo el Retorno Ajustado al Riesgo

Antes de personalizar, necesitamos una métrica para comparar la "calidad" de un portafolio: el **Ratio de Sharpe (SR)**.

El SR es el indicador más importante para medir el rendimiento de una inversión *en relación con el riesgo que tomaste*. Te dice cuánto rendimiento extra (por encima de la tasa libre de riesgo, $R_f$) obtuviste por cada unidad de volatilidad que asumiste.

$$\text{Ratio de Sharpe} = \frac{E(R_p) - R_f}{\sigma_p}$$

Donde:

  * $E(R_p)$: Rendimiento Esperado del Portafolio.
  * $R_f$: Tasa Libre de Riesgo (lo que ganarías sin riesgo, ej. 3%).
  * $\sigma_p$: Riesgo del Portafolio (Desviación Estándar).

**Interpretación:** Un Ratio de Sharpe **más alto es mejor**, ya que significa que el portafolio está generando más rendimiento por el mismo nivel de riesgo.

#### Implementación: Encontrando el Portafolio de Máximo Sharpe

El **Portafolio Tangente (o de Máximo Sharpe)** es crucial, ya que es el portafolio diversificado que, para su nivel de riesgo, ofrece el mejor rendimiento.

```python
# Tasa Libre de Riesgo Simulada (Rf)
R_f = 0.03 # 3%

# Función que queremos MAXIMIZAR: el Ratio de Sharpe
# Como la función minimize de SciPy solo minimiza, usamos la versión NEGATIVA
def funcion_a_maximizar_sharpe(weights, rendimientos, cov_matrix, R_f):
    rendimiento = calcular_rendimiento(weights, rendimientos)
    riesgo = calcular_riesgo(weights, cov_matrix)
    sharpe_ratio = (rendimiento - R_f) / riesgo
    return -sharpe_ratio # Devolvemos el negativo para que minimize lo encuentre

# Ejecutamos la optimización para encontrar el MÁXIMO SHARPE
sharpe_resultado = minimize(
    funcion_a_maximizar_sharpe, 
    initial_weights, 
    args=(rendimientos_esperados, matriz_covarianza, R_f),
    method='SLSQP', 
    bounds=bounds, 
    constraints=constraints
)

# Extracción de Ponderaciones del Portafolio de Máximo Sharpe
max_sharpe_weights = sharpe_resultado.x
max_sharpe_rendimiento = calcular_rendimiento(max_sharpe_weights, rendimientos_esperados)
max_sharpe_riesgo = calcular_riesgo(max_sharpe_weights, matriz_covarianza)
max_sharpe_ratio = (max_sharpe_rendimiento - R_f) / max_sharpe_riesgo

print("--- Portafolio de Máximo Ratio de Sharpe ---")
print(f"Ratio de Sharpe: {max_sharpe_ratio:.2f}")
print(f"Rendimiento: {max_sharpe_rendimiento*100:.2f}% | Riesgo: {max_sharpe_riesgo*100:.2f}%")
print(f"Ponderaciones (A, B, C): {max_sharpe_weights.round(4)}")
```

### 2\. Personalización por Perfil de Riesgo

Una vez que tenemos la Frontera Eficiente, podemos elegir los portafolios que se ajustan a cada perfil.

| Perfil de Riesgo | Meta de la Cartera | Posición en la Frontera | Estrategia (Basada en la Optimización) |
| :--- | :--- | :--- | :--- |
| **Conservador** | **Minimizar el riesgo** (Proteger el capital). | **Cercano al Portafolio de Mínima Varianza (PMV)**. | Ponderaciones más altas en activos con baja volatilidad (ej. Activo C) y baja correlación con los otros. |
| **Moderado** | **Máximo rendimiento ajustado al riesgo**. | **Portafolio de Máximo Sharpe**. | El equilibrio perfecto. Usa las ponderaciones de `max_sharpe_weights` que maximizan el Ratio de Sharpe. |
| **Agresivo** | **Maximizar el rendimiento** (Aceptar mayor riesgo). | **Extremo superior de la Frontera**. | Ponderaciones altas en el activo con el mayor rendimiento esperado (ej. Activo B), incluso si su volatilidad es alta. |

#### Ejemplo de Asignación (Usando los resultados de la optimización)

| Cartera | Ponderaciones A, B, C | Riesgo (Volatilidad) | Rendimiento Esperado | Decisión |
| :--- | :--- | :--- | :--- | :--- |
| **Conservadora (PMV)** | $\approx$ **57% C**, 35% A, 8% B | $\mathbf{9.40\%}$ | $10.05\%$ | Busca la estabilidad de C (bajo riesgo). |
| **Moderada (Máx. Sharpe)** | $\approx$ **45% B**, 30% A, 25% C | $\mathbf{12.50\%}$ | $12.80\%$ | El equilibrio perfecto. |
| **Agresiva (Máx. Rend.)** | $\approx$ **100% B**, 0% A, 0% C | $\mathbf{25.00\%}$ | $15.00\%$ | Asume el riesgo del activo de mayor rendimiento (B). |

La MPT y la optimización te proporcionan la hoja de ruta para construir estas carteras. Ya no se trata de adivinar, sino de usar matemáticas sólidas para la asignación de activos.

-----

## Fin del Módulo 4: Evaluación y Próximo Paso

¡Felicidades\! Has completado el Módulo de Gestión de Carteras.

  * **Nivel Profesional alcanzado:** **Analista Cuantitativo de Portafolios (Junior)**. Ahora puedes aplicar la estadística y el cálculo para valorar activos y optimizar carteras.
  * **Área recomendada para el siguiente paso:** **Módulo 5: Aprendizaje Automático (Machine Learning) en Finanzas**, que te permitirá usar modelos predictivos avanzados para mejorar aún más la estimación de los rendimientos y la gestión del riesgo.

**¿Quieres continuar con el Módulo 5, Nivel Básico: Introducción a la Regresión Lineal y su uso para predecir precios?** 😊
