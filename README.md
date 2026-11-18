# Pre-entrega: Análisis Visual de Distribuciones Categóricas y Outliers

**Curso:** Data Science II (Módulo 15 - Coderhouse)  
**Dataset:** Properati - Propiedades en CABA (`properati.csv`)

---

## 1. Objetivo de la actividad
En este proyecto realizamos un análisis exploratorio sobre una base de datos real de propiedades en la Ciudad Autónoma de Buenos Aires (**Properati**, con 29.067 registros) para:
- Analizar variables categóricas con gráficos de barras (`countplot` y `barplot`).
- Calcular e interpretar la **asimetría (*skewness*)** y la **curtosis (*kurtosis*)** de las variables numéricas principales.
- Visualizar y detectar **valores atípicos (*outliers*)** usando **Boxplots**, **Violin Plots** y el método del **Rango Intercuartílico (IQR)**.

---

## 2. Origen de los datos y variables utilizadas
El archivo `properati.csv` proviene de los datos abiertos de **Properati** y contiene publicaciones de inmuebles en CABA sin valores nulos.

**Variables principales analizadas:**
- **Categóricas:**
  - `property_type`: Tipo de propiedad (`apartment`, `PH`, `house`, `store`).
  - `place_name`: Barrio de ubicación en CABA.
- **Numéricas:**
  - `price_aprox_usd`: Precio de la propiedad en dólares (USD).
  - `surface`: Superficie total en metros cuadrados ($\text{m}^2$).
  - `price_m2`: Precio por metro cuadrado ($\text{USD/m}^2$).
  - `rooms`: Cantidad de ambientes.

---

## 3. Resumen de resultados obtenidos

### Distribución de variables categóricas
- El **85,45%** de los inmuebles son departamentos (`apartment`), seguidos por `PH` (**5,78%**), casas (`house`, **4,89%**) y locales (`store`, **3,88%**).
- Los barrios con mayor cantidad de publicaciones son **Palermo**, **Belgrano**, **Caballito**, **Villa Urquiza** y **Recoleta**.

### Asimetría (*Skewness*) y Curtosis (*Kurtosis*)
| Variable | Media | Mediana | Asimetría (Skewness) | Curtosis (Kurtosis) | Interpretación |
| :--- | :---: | :---: | :---: | :---: | :--- |
| `price_aprox_usd` | USD 228.084 | USD 148.000 | **3,92** | **22,20** | Asimetría positiva fuerte (cola hacia la derecha) y curtosis alta por propiedades muy caras. |
| `surface` | 92,59 m² | 60,00 m² | **5,86** | **85,86** | Asimetría positiva muy alta y gran concentración de valores extremos en inmuebles grandes. |
| `price_m2` | USD 2.531 | USD 2.410 | **1,76** | **6,44** | Asimetría positiva moderada; es más estable que el precio total porque divide por los m². |

### Detección de Outliers con el método IQR
Aplicando la fórmula $IQR = Q_3 - Q_1$ y los límites $[Q_1 - 1,5 \times IQR,\; Q_3 + 1,5 \times IQR]$:
- **`price_aprox_usd`:** $Q_1 = \text{USD } 99.715$, $Q_3 = \text{USD } 255.000$, $IQR = \text{USD } 155.285$. Se detectaron **2.575 outliers** (propiedades que superan los **USD 487.927,50**, equivalentes al **8,86%** de los datos).
- **`surface`:** $Q_1 = 42\text{ m}^2$, $Q_3 = 104\text{ m}^2$, $IQR = 62\text{ m}^2$. Se detectaron **2.624 outliers** (propiedades mayores a **197 m²**, equivalentes al **9,03%** de los datos).

---

## 4. ¿Qué decisión tomaríamos con los outliers encontrados?
Basándonos en los gráficos (Boxplots y Violin Plots) y en los registros filtrados:
1. **No eliminarlos todos directamente:** Los valores altos de precio y superficie no son errores de carga, sino propiedades reales (casas grandes, locales comerciales o departamentos de lujo en barrios como Palermo, Recoleta o Puerto Madero). Borrar casi el 9% del dataset haría que perdamos información importante del mercado.
2. **Transformarlos con logaritmo:** Como las distribuciones de precio y superficie tienen una cola muy larga hacia la derecha (asimetría positiva), conviene aplicar una **transformación logarítmica** (`np.log10`) para reducir el impacto de los valores extremos sin eliminar filas.
3. **Analizar por tipo de propiedad:** Como las casas y locales son naturalmente más grandes y caros que los departamentos, conviene separar o filtrar los outliers dentro de cada categoría de `property_type`.

---

## 5. Cómo ejecutar el proyecto
1. Clonar o descargar este repositorio.
2. Instalar las librerías necesarias:
   ```bash
   pip install -r requirements.txt
   ```
3. Abrir y ejecutar el archivo `analisis_distribuciones_outliers.ipynb` en Jupyter Notebook, VS Code o Google Colab.
