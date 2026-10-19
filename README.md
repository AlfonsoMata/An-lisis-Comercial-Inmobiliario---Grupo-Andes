# Análisis Comercial Inmobiliario - Grupo Andes

## Descripción del Proyecto

En el dinámico mercado inmobiliario, **Grupo Andes** enfrentaba una problemática crítica de toma de decisiones a ciegas: la falta de visibilidad centralizada sobre el desempeño real de sus operaciones comerciales. Esta desconexión impedía a la alta dirección responder preguntas clave sobre rentabilidad, comportamiento de compra de los clientes y eficiencia de sus canales de venta.

El conflicto principal a resolver radicaba en diagnosticar y mitigar la vulnerabilidad del negocio ante tres factores principales:
1. **Caídas estacionales de ingresos no controladas** durante el segundo y tercer trimestre del año.
2. **Una fuerte dependencia del canal de corredores externos**, la cual erosionaba el margen bruto mediante elevadas comisiones mientras el canal directo permanecía subexplotado.
3. **Una baja penetración en segmentos de alto valor** (Inversionistas y Clientes de Alto Patrimonio), concentrando el riesgo operativo casi en su totalidad en compradores de vivienda primaria.

Para abordar esta problemática, se desarrolló e implementó una solución analítica de extremo a extremo que integra procesamiento de datos en Python, modelado multidimensional en Power BI y lógica analítica avanzada mediante DAX y análisis de cohortes, permitiendo transformar datos dispersos en decisiones comerciales rentables y sustentables.

---

## Conjuntos de Datos Utilizados

* **`hecho_ventas_propiedades`**: Tabla de hechos principal con las transacciones de venta de propiedades, incluyendo precio, identificadores de cliente y propiedad, canal de distribución y fecha de transacción.

* **`dim_clientes`**: Tabla dimensional con información demográfica y segmentación estratégica de clientes (Compradores por Primera Vez, Inversionistas, Clientes de Alto Patrimonio, etc.).

* **`dim_propiedades`**: Tabla dimensional con las características detalladas de las propiedades (tipo de inmueble, tamaño, ubicación y atributos físicos).

* **`dim_fecha`**: Tabla calendario dinámica generada ad-hoc en Power BI para habilitar el modelado de inteligencia de tiempo y análisis estacionales.

---

## Metodología y Flujo de Trabajo

### 1. Limpieza de Datos y Control de Calidad

* Validaciones y depuración de datos previa al modelado en **Power BI** y **Python**.
* Conversión de la columna `fecha_venta` a formato estandarizado **Date**.
* Normalización y formateo correcto de campos numéricos (precios, comisiones) y ajuste del campo `porcentaje_comision` a formato porcentual.
* Verificación de duplicados en claves primarias y gestión de valores nulos para asegurar la integridad referencial entre dimensiones.

### 2. Modelado de Datos (Esquema en Estrella)

* Estructuración de un **Esquema Estrella (Star Schema)** optimizado para análisis ejecutivo:
  * **Tabla Central de Hechos:** `hecho_ventas_propiedades`.
  * **Tablas Dimensionales:** `dim_clientes`, `dim_propiedades` y `dim_fecha`.
* Creación en DAX de la tabla **`dim_fecha`** dinámicamente según el rango de transacciones:

```dax
Dim_Fecha = ADDCOLUMNS(
    CALENDAR(MIN(hecho_ventas_propiedades[fecha_venta]), MAX(hecho_ventas_propiedades[fecha_venta])),
    "Año", YEAR([Date]),
    "Mes", FORMAT([Date], "MMMM"),
    "Mes Numero", MONTH([Date]),
    "Año-Mes", FORMAT([Date], "YYYY-MM")
)
```

### 3. Creación de Medidas y Analítica DAX

* **Medidas Base:** Cálculo de KPIs clave (*Ingreso Total*, *Cantidad de Ventas*, *Ticket Promedio* y *Comisión Total*).
* **Contexto de Filtro (% Participación):** Construcción de medidas relativas para determinar la aportación al ingreso total por tipo de propiedad, canal de venta y segmento de cliente.
* **Inteligencia de Tiempo (Time Intelligence):** Implementación de métricas acumuladas y comparativas (*Ventas YTD*, *Ventas MTD*, *Ventas Año Anterior* y *Crecimiento YoY*).
* **Análisis de Cohortes:** Lógica DAX para determinar la *Primera compra por cliente*, el *Mes Cohorte* (mes de adquisición) y el *Mes Venta* para evaluar la recurrencia y comportamiento de los clientes a lo largo del tiempo.

---

## Hallazgos Clave de Negocio

* **Desempeño General (2023 - 2024):** Generación de **6.012 Billones MXN** en ingresos totales a través de **8.5K transacciones**, con un ticket promedio de **707.33 K MXN** y comisiones totales de **200 Millones MXN**.
* **Dominio Residencial:** El tipo de propiedad **Casa Hogar** representa la mayor participación de ingresos con un **37%**. Asimismo, el segmento de compradores por **primera vez** concentra el **62.9%** de las transacciones.
* **Estacionalidad de Ventas:** Se identificaron contracciones recurrentes en los ingresos entre el segundo y tercer trimestre (inicios de verano y finales de año), patrón típico del ciclo de compra residencial.
* **Concentración en Canales:** El canal de **corredores** genera el **72%** de los ingresos frente al **27%** obtenido mediante el canal directo.
* **Oportunidad en Clientes Premium:** Se observa una baja representación en los segmentos de **Inversionistas (24%)** y **Clientes de Alto Patrimonio (12%)**.
* **Crecimiento e Inteligencia de Cohortes:** Las ventas muestran un crecimiento del **10% Year over Year (YoY)**, destacando mayor recurrencia en las cohortes adquiridas a inicios de año (Enero 2023 y Enero 2024).

---

## Tecnologías Utilizadas

* **Lenguaje:** Python
* **Librerías:** Pandas, NumPy, Matplotlib, Seaborn
* **Business Intelligence & DAX:** Power BI Desktop (`.pbix`)
* **Herramientas de Desarrollo:** Jupyter Notebook (`.ipynb`), VS Code, Git/GitHub
