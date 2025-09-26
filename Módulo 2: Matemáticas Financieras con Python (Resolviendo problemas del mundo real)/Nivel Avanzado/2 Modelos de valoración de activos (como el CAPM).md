¡Estupendo\! Estás a punto de adentrarte en el corazón de la teoría financiera moderna. El **Modelo de Valoración de Activos de Capital (CAPM, por sus siglas en inglés)** es la herramienta estándar que los profesionales usan para determinar el rendimiento esperado de una inversión, basándose en su nivel de riesgo.

En esencia, el CAPM te dice: **"Por el riesgo adicional que tomas, ¿cuánto rendimiento extra deberías esperar?"**

-----

### Módulo 2: Matemáticas Financieras con Python

#### Nivel Avanzado: Modelos de Valoración de Activos (CAPM)

### 1\. Entendiendo la Lógica del CAPM

El CAPM asume que el riesgo se puede dividir en dos tipos:

1.  **Riesgo No Sistemático (Diversificable):** Es el riesgo específico de una empresa (una huelga, un mal CEO, etc.). Se elimina al diversificar tu cartera (Módulo 4). El mercado no te paga por tomar este riesgo.
2.  **Riesgo Sistemático (No Diversificable):** Es el riesgo de que todo el mercado se mueva (una recesión, un cambio en las tasas de interés). Es el riesgo que no puedes evitar. **El CAPM solo valora este riesgo.**

El modelo relaciona el riesgo sistemático de un activo con el rendimiento esperado a través de una métrica clave: el **Beta ($\beta$)**.

### 2\. La Fórmula Matemática del CAPM

La fórmula calcula el rendimiento esperado de un activo ($E(R_i)$):

$$E(R_i) = R_f + \beta_i \times (E(R_m) - R_f)$$

Donde:

  * $E(R_i)$: **Rendimiento Esperado** del activo (lo que queremos calcular).
  * $R_f$: **Tasa Libre de Riesgo** (Risk-Free Rate). El rendimiento de una inversión muy segura, como los bonos del gobierno.
  * $E(R_m)$: **Rendimiento Esperado del Mercado** (Market Expected Return). Lo que se espera que rinda el mercado en general (un índice, como el S\&P 500).
  * $(E(R_m) - R_f)$: **Prima de Riesgo del Mercado** (Market Risk Premium). La recompensa extra que el mercado ofrece por invertir en acciones en lugar de activos sin riesgo.
  * $\beta_i$: **Beta del Activo**. Mide la sensibilidad del activo a los movimientos del mercado.

### 3\. Calculando la Beta ($\beta$) con Python

La Beta es la parte más matemática del CAPM. Se calcula usando la relación histórica del activo con el mercado:

$$\beta = \frac{Covarianza(R_{activo}, R_{mercado})}{Varianza(R_{mercado})}$$

Usaremos NumPy y Pandas para calcular esto con datos simulados.

**Caso de Estudio:** Vamos a calcular la Beta para una acción.

```python
import numpy as np
import pandas as pd

# 1. Datos simulados de rendimientos diarios (en decimal)
# Usamos un DataFrame con 100 días de rendimientos
np.random.seed(42)
datos = {
    'Rendimientos_Activo': np.random.normal(0.001, 0.015, 100),
    'Rendimientos_Mercado': np.random.normal(0.0005, 0.01, 100)
}
df_rendimientos = pd.DataFrame(datos)

# Simulación para que el Activo se mueva más o menos con el Mercado
df_rendimientos['Rendimientos_Activo'] = (df_rendimientos['Rendimientos_Mercado'] * 1.2) + np.random.normal(0, 0.005, 100)

# 2. Cálculo de Covarianza y Varianza
# Usamos .cov() para la covarianza de las dos series, que devuelve una matriz
matriz_covarianza = df_rendimientos.cov()
covarianza_activo_mercado = matriz_covarianza.loc['Rendimientos_Activo', 'Rendimientos_Mercado']

# Usamos .var() para la varianza del mercado
varianza_mercado = df_rendimientos['Rendimientos_Mercado'].var()

# 3. Cálculo de la Beta (β)
beta = covarianza_activo_mercado / varianza_mercado

print("--- 3. Cálculo de Beta (β) ---")
print(f"Covarianza Activo/Mercado: {covarianza_activo_mercado:.6f}")
print(f"Varianza del Mercado: {varianza_mercado:.6f}")
print(f"Beta (β) del Activo: {beta:.2f}")
# Conclusión: Una Beta de 1.23 significa que el activo es 23% más volátil que el mercado.
```

### 4\. Aplicando el CAPM para el Rendimiento Esperado

Una vez que tenemos la Beta, podemos aplicarla a la fórmula del CAPM.

**Ejemplo Final:**

  * $\beta$ del Activo: **1.23** (Calculada arriba)
  * $R_f$ (Tasa Libre de Riesgo): **3.0%** (0.03)
  * $E(R_m)$ (Rendimiento Esperado del Mercado): **10.0%** (0.10)

<!-- end list -->

```python
# Variables del Modelo
R_f = 0.03
E_R_m = 0.10

# 1. Calcular la Prima de Riesgo del Mercado (E(R_m) - R_f)
prima_de_riesgo_mercado = E_R_m - R_f

# 2. Aplicar la fórmula CAPM
Rendimiento_Esperado = R_f + (beta * prima_de_riesgo_mercado)

print("\n--- 4. Cálculo del Rendimiento Esperado con CAPM ---")
print(f"Prima de Riesgo del Mercado: {prima_de_riesgo_mercado*100:.2f}%")
print(f"Rendimiento Requerido (E(R_i)): {Rendimiento_Esperado*100:.2f}%")

# Conclusión: Un activo con una Beta de 1.23, debe rendir al menos 11.61% para que valga la pena el riesgo.
```

Si un analista cree que este activo rendirá **menos del 11.61%**, lo consideraría sobrevalorado. Si cree que rendirá **más del 11.61%**, lo consideraría una buena compra.
