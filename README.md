# ConnectaTel: Análisis de Comportamiento y Perfil Estadístico de Clientes 📞📊

Este repositorio contiene el análisis de datos de **ConnectaTel**, una empresa de telecomunicaciones en Latinoamérica. El objetivo principal es evaluar el comportamiento de los clientes registrados hasta el año 2024 para identificar patrones de consumo, diseñar estrategias de retención y sugerir mejoras en los planes actuales.

---

## 🎯 Objetivo del Proyecto

- **Explorar y limpiar** tres fuentes de datos con inconsistencias técnicas reales (valores nulos, registros fuera de límite, y valores centinela).
- **Construir un perfil estadístico** robusto del consumo de llamadas y mensajes de los clientes.
- **Detectar e interpretar comportamientos atípicos (outliers)** para el negocio.
- **Segmentar la base de usuarios** bajo criterios de edad y volumen de uso histórico para proponer recomendaciones accionables para los stakeholders.

---

## 📂 Datasets Utilizados

El análisis integra tres conjuntos de datos clave ubicados originalmente en `/datasets/`:

1. **`plans.csv`**: Detalle de las tarifas del negocio.
   - Contiene la información de los dos planes vigentes: `Basico` (64.88% de los usuarios) y `Premium` (35.12%).
2. **`users_latam.csv`**: Datos demográficos y de registro de los clientes.
   - Variables clave: `user_id`, `age` (rango 18-79 años), `city`, `reg_date`, `plan` y `churn_date` (tasa de bajas del 11.65%).
3. **`usage.csv`**: Detalle transaccional del uso real del servicio.
   - Registros de tipo `call` (llamada) y `text` (mensaje de texto) con duraciones y longitudes de caracteres.

---

## 🔄 Etapas del Análisis Realizadas

### 1. Carga y Exploración Inicial 🔍
- Carga de datos utilizando `pandas` y análisis dimensional inicial con `.shape` e `.info()`.
- Identificación preliminar de un desbalance estructural de nulos y tipos de datos a corregir.

### 2. Calidad de Datos y Limpieza de Inconsistencias 🛠️
- **Imputación de Valores Centinela**: Corrección del valor `-999` en la columna `age` sustituyéndolo con la mediana real (`48.0` años), resolviendo un fuerte sesgo inicial en la media de edad.
- **Estandarización de Fechas**: Conversión de tipos de datos y descarte de registros imposibles (40 cuentas con registro en el año futuro `2026`).
- **Unificación de Categorías Desconocidas**: Tratamiento del marcador `"?"` en `city` y conversión a `pd.NA` para representar con precisión el 14.12% de nulos totales en la columna.
- **Confirmación de Mecanismos de Nulos**: Demostración estadística de que las ausencias en las columnas de consumo (`duration` y `length`) son **Missing At Random (MAR)** y son dependientes de la variable `type`.

### 3. Perfil de Consumo Histórico (Estadística Descriptiva) 📈
- Agrupación transaccional del uso real a nivel de cada usuario (`cant_mensajes`, `cant_llamadas`, `cant_minutos_llamada`).
- Visualización detallada de las distribuciones mediante histogramas cruzados por plan (`Basico` y `Premium`). Se identificó un patrón de consumo general de sesgo positivo (cola derecha ligera).

### 4. Detección Estratégica de Outliers 🕵️‍♂️
- Aplicación del método de **Rango Intercuartílico (IQR)** para aislar consumos excepcionalmente altos.
- Hallazgo crítico para el negocio: **30 registros fijos con exactamente `120.0` minutos** en duración de llamada y **30 registros fijos con `1490.0` caracteres** en mensajes, interpretados como potenciales límites operativos de red o errores de sistema.

### 5. Segmentación de Clientes 🎯
- **Por Uso**: Clasificación en `Bajo uso` (< 5 llamadas y mensajes), `Uso medio` (< 10 llamadas y mensajes) y `Alto uso` (resto de casos).
- **Por Edad**: División demográfica en `Joven` (<30), `Adulto` (<60) y `Adulto Mayor` (>=60).

---

## 💡 Principales Hallazgos 🏆

- **Sanidad de Datos Lograda**: Se neutralizó exitosamente el impacto del valor centinela `-999` en la edad de los usuarios, estabilizando la media del perfil demográfico en **48.14 años** (frente al valor distorsionado previo de `33.74`).
- **Estructura de la Clientela**: El plan **Básico** es el más popular concentrando el **64.88%** de los clientes, mientras que el **Premium** retiene al **35.12%**, observándose un consumo de llamadas y mensajes bastante similar entre ambos planes (concentrados en rangos bajos y moderados).
- **Patrón de Uso**: La mayoría de los usuarios utiliza de manera estándar entre 0 y 40 minutos totales de llamadas mensuales, con un pico fuerte en torno a los 15 minutos.
- **Hallazgo Crítico de Red (Outliers)**: Se detectó una anomalía idéntica de **30 llamadas fijas de exactamente 120.0 minutos** y **30 mensajes con exactamente 1,490 caracteres**. Al estar sumamente alejados de los percentiles habituales, estos hallazgos apuntan a restricciones operativas del sistema o topes técnicos de la red en lugar de un uso orgánico del cliente.
- **Oportunidad de Negocio**: Dado que los clientes con mayor frecuencia de llamadas (más de 12 al mes) pertenecen casi en su totalidad al plan Básico, se identifica una oportunidad estratégica de conversión/upgrade hacia planes superiores o intermedios diseñados para mitigar la fricción de consumo extra.

---

## 💻 Cómo Ejecutar el Notebook

Este análisis fue diseñado para ejecutarse en entornos interactivos de Python.

### Opción 1: Google Colab (Recomendado) 🚀
1. Entra a [Google Colab](https://colab.research.google.com/).
2. Ve a la pestaña **GitHub** en el menú de apertura.
3. Introduce el enlace al repositorio: `https://github.com/marianafm01/sprint7-final-project`.
4. Abre el archivo `.ipynb` correspondiente y ejecuta las celdas secuencialmente.

### Opción 2: Ejecución Local 🖥️
Si deseas ejecutar el proyecto localmente:
1. Asegúrate de tener instalado Python 3.8+ y Jupyter Notebook o VS Code.
2. Instala las dependencias requeridas:
   ```bash
   pip install pandas numpy matplotlib seaborn
   ```
3. Clona el repositorio:
   ```bash
   git clone https://github.com/marianafm01/sprint7-final-project.git
   ```
4. Coloca los archivos `.csv` en una carpeta local llamada `/datasets/` o actualiza los paths del código a tus rutas locales.

---

## 🧪 Guía de Reproducción

Para asegurar que los resultados y gráficos se generen de manera idéntica:
1. **Ejecuta las celdas en orden estricto** de arriba hacia abajo (`Run All` o `Ctrl + F9` en Colab).
2. El proceso de limpieza redefinirá los tipos de datos a tipo `datetime` y `category` de forma automática.
3. Los gráficos estadísticos e histogramas de seaborn se auto-renderizarán inmediatamente después de la celda de agrupamiento IQR.

---

## 👤 Autor

- **Mariana Flores Maldonado** - *Análisis de Datos y Desarrollo* - [marianafm01](https://github.com/marianafm01)
