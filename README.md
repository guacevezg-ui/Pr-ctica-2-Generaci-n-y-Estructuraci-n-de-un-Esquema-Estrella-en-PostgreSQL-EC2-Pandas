# Pipeline ETL: Modelado de Esquema Estrella para Data Warehouse de Ventas

Este repositorio contiene la solución práctica para la **Transformación y Modelado Multidimensional** de datos operativos transaccionales (OLTP) en un diseño analítico de **Esquema Estrella (Star Schema)** optimizado para su posterior ingesta en una base de datos PostgreSQL (AWS EC2).

## 🚀 Estructura del Proyecto

```text
├── .gitignore               # Exclusiones de archivos de entorno y caché
├── README.md                # Documentación principal del proyecto (este archivo)
├── etl_proceso.py           # Script principal de transformación y carga (Pandas)
└── requirements.txt         # Dependencias del proyecto ajustadas a la práctica
```

## 📋 Requisitos del Sistema

El desarrollo se construyó utilizando **Python 3.10+** y las siguientes librerías core de ingeniería de datos:
* **pandas** (Manipulación y estructuración de DataFrames)
* **numpy** (Simulación de ingesta operativa mediante vectores aleatorios)

## 🛠️ Instalación y Configuración

Sigue estos pasos secuenciales en tu terminal para replicar el entorno de ejecución local o en tu instancia de **AWS Academy**:

1. **Clonar el repositorio:**
   ```bash
   git clone https://github.com
   cd tu-repositorio
   ```

2. **Crear e iniciar el entorno virtual de Python:**
   ```bash
   python -m venv venv
   source venv/bin/activate  # En Linux/macOS
   # venv\Scripts\activate   # En Windows
   ```

3. **Instalar dependencias requeridas:**
   ```bash
   pip install -r requirements.txt
   ```

## ⚙️ Desarrollo del Script (`etl_proceso.py`)

El pipeline ejecuta un proceso compuesto por tres etapas críticas:

1. **Ingesta de Datos Crudos (OLTP):** Generación artificial de un dataset plano de 500 transacciones con clientes, productos y estampas de tiempo altamente redundantes.
2. **Separación Dimensional:** Generación automatizada de **Surrogate Keys (SK)** secuenciales e inteligentes para romper la dependencia de las llaves naturales de negocio.
3. **Extensiones Analíticas Aplicadas:**
   * **Dim_Tiempo:** Desglose calendárico nativo enriquecido con los atributos agregados `dia_semana` y la bandera binaria `es_fin_semana`.
   * **Fact_Ventas:** Cálculo de la métrica financiera compuesta `monto_total` e inclusión de la columna analítica fiscal `impuesto_calculado` (IVA del 16%).

## 💻 Ejecución y Pruebas

Para lanzar el pipeline completo y visualizar la estructuración multidimensional directamente en la consola, ejecuta el siguiente comando:

```bash
python etl_proceso.py
```

### Ejemplo de Salida Esperada de la Tabla de Hechos:
```text
=== TABLA DE HECHOS (FACT_VENTAS) ===
   id_transaccion  id_cliente_sk  id_producto_sk  id_tiempo_sk  cantidad  monto_total  impuesto_calculado
0            1000              1               1      20260101         3      45000.0              7200.0
1            1003              1               4      20260101         4      18000.0              2880.0
2            1015              1              10      20260101         1       4500.0               720.0
```

## 📊 Arquitectura del Esquema Estrella Diseñado

El modelo se desnormaliza para reducir drásticamente el costo computacional de las consultas analíticas masivas (OLAP):

* **Fact_Ventas (Centro):** Resguarda las métricas (`cantidad`, `monto_total`, `impuesto_calculado`) y las llaves foráneas.
* **Dim_Cliente / Dim_Producto / Dim_Tiempo (Periferia):** Almacenan el contexto descriptivo de negocio indexado por claves subrogadas de tipo entero rápido.
