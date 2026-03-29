# 📊 ConnectaTel — Análisis Exploratorio de Datos (EDA)

Proyecto de análisis exploratorio aplicado a una empresa de telecomunicaciones ficticia. A partir de datos reales de usuarios y registros de uso, se detectaron problemas de calidad, se segmentaron 4,000 clientes por edad y nivel de uso, y se identificaron oportunidades de negocio mediante estadística descriptiva y visualizaciones.

---

## 🎯 Objetivo

El objetivo principal de este proyecto es realizar un análisis exploratorio completo sobre la base de clientes de ConnectaTel, cubriendo desde la limpieza y validación de los datos hasta la segmentación de usuarios y la generación de insights accionables para la toma de decisiones comerciales.

De forma específica, el proyecto busca:

- Detectar y corregir problemas de calidad en los datos (sentinels, nulos, fechas inválidas).
- Comprender el comportamiento de uso de los clientes (llamadas, mensajes, minutos).
- Segmentar a los usuarios según edad y nivel de uso.
- Identificar patrones de consumo extremo (outliers) y su implicación para el negocio.
- Generar recomendaciones estratégicas para mejorar la oferta de planes.

---

## 📁 Datasets utilizados

El proyecto trabaja con 3 datasets en formato `.csv`:

| Dataset | Descripción | Filas aprox. |
|---|---|---|
| `users.csv` | Información demográfica de los usuarios (nombre, edad, ciudad, plan, fechas) | 4,000 |
| `usage.csv` | Registros históricos de uso (llamadas, mensajes, duración, tipo) | 40,000 |
| `plans.csv` | Descripción de los planes disponibles (Básico y Premium) | 2 |

> ⚠️ El dataset `plans` solo contiene 2 registros (uno por plan), por lo que no requiere exploración estadística adicional.

---

## 🔬 Etapas del análisis

### 1. Carga y exploración inicial
- Carga de los 3 datasets con `pandas`.
- Revisión de tipos de datos, dimensiones y primeras filas.

### 2. Detección de valores inválidos y sentinels
- Resumen estadístico de columnas numéricas para detectar valores anómalos.
- Revisión de valores únicos en columnas categóricas (`city`, `plan`).
- Identificación de sentinels: `-999` en `age` y `"?"` en `city`.

### 3. Revisión y estandarización de fechas
- Conversión de columnas de fecha a tipo `datetime` con `errors='coerce'`.
- Conteo de registros por año para detectar fechas fuera de rango.
- Marcado como nulos (`pd.NaT`) de fechas imposibles (anteriores a 2000 o posteriores a 2024).

### 4. Limpieza básica de datos
- Reemplazo del sentinel `-999` en `age` por la mediana (calculada sin outliers).
- Reemplazo del sentinel `"?"` en `city` por `pd.NA`.
- Verificación de nulos MAR (Missing At Random) en `duration` y `length` según la columna `type`.

### 5. Construcción del perfil de usuario
- Creación de columnas auxiliares `is_text` e `is_call` para facilitar la agregación.
- Agrupación de `usage` por `user_id` calculando: total de mensajes, llamadas y minutos.
- Merge con `users` usando `how='left'` para conservar todos los usuarios.

### 6. Resumen estadístico y visualización de distribuciones
- Estadísticas descriptivas de columnas numéricas y distribución porcentual de `plan`.
- Histogramas con `hue='plan'` para comparar comportamiento entre planes.
- Boxplots para identificar outliers visualmente.
- Cálculo de límites IQR y decisión de tratamiento para cada variable.

### 7. Segmentación de clientes
- **Por uso:** clasificación en `Bajo uso`, `Uso medio` y `Alto uso` usando `np.select`.
- **Por edad:** clasificación en `joven`, `adulto` y `adulto mayor`.
- Visualización de la distribución de cada segmento con `countplot`.

### 8. Insight ejecutivo para stakeholders
- Síntesis de hallazgos en un análisis ejecutivo orientado al negocio.
- Recomendaciones estratégicas basadas en los patrones detectados.

---

## ▶️ Cómo ejecutar el notebook

### Opción 1 — Google Colab (recomendado)

1. Abre [Google Colab](https://colab.research.google.com/).
2. Ve a **Archivo → Abrir notebook → GitHub** y pega la URL de este repositorio.
3. Sube los datasets a la sesión de Colab:
   ```python
   from google.colab import files
   files.upload()  # sube users.csv, usage.csv y plans.csv
   ```
4. Ejecuta las celdas en orden con `Shift + Enter` o desde **Entorno de ejecución → Ejecutar todo**.

### Opción 2 — Jupyter Notebook local

1. Clona el repositorio:
   ```bash
   git clone https://github.com/tu-usuario/connectatel-eda.git
   cd connectatel-eda
   ```
2. Instala las dependencias:
   ```bash
   pip install -r requirements.txt
   ```
3. Abre el notebook:
   ```bash
   jupyter notebook
   ```
4. Ejecuta las celdas en orden desde el menú **Cell → Run All**.

---

## 🔁 Guía de reproducción

Para reproducir el análisis de principio a fin, sigue estos pasos:

1. **Coloca los datasets** (`users.csv`, `usage.csv`, `plans.csv`) en la misma carpeta que el notebook, o ajusta las rutas de carga según tu entorno.

2. **Ejecuta las celdas en orden.** El notebook está estructurado secuencialmente — cada sección depende de las anteriores. No saltes pasos.

3. **Dependencias requeridas:**

   | Librería | Versión recomendada |
   |---|---|
   | `pandas` | ≥ 1.5 |
   | `numpy` | ≥ 1.23 |
   | `matplotlib` | ≥ 3.6 |
   | `seaborn` | ≥ 0.12 |

4. **Puntos de atención:**
   - La corrección del sentinel `-999` en `age` debe aplicarse sobre `user_profile` (después del merge), no solo sobre `users`, para que los gráficos reflejen los datos limpios.
   - El análisis MAR de `duration` y `length` confirma que sus nulos son estructurales — no deben imputarse.
   - Los outliers en `cant_mensajes`, `cant_llamadas` y `cant_minutos_llamada` fueron **conservados** intencionalmente por tener sentido de negocio.

5. **Resultado esperado:** al finalizar la ejecución completa, el dataframe `user_profile` contará con 4,000 filas, 13 columnas y las nuevas variables `grupo_uso` y `grupo_edad` listas para análisis.

---

## 🛠️ Tecnologías utilizadas

- Python
- pandas
- numpy
- matplotlib
- seaborn
- Jupyter Notebook

---

## 👤 Autor

Proyecto desarrollado como parte de un programa académico de análisis de datos.
