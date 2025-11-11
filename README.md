# 🦠 COVID Bronze-Silver Architecture

Repositorio que demuestra una arquitectura **Bronze → Silver** para el manejo de datos epidemiológicos del sistema de vigilancia COVID-19 en México.  
El proyecto muestra cómo estructurar, limpiar y documentar una base de datos masiva (65 millones de registros) de manera reproducible y trazable.

```

## 📂 Estructura general
covid-bronze-silver-architecture/
│
├── README.md
├── LICENSE
│
├── data_and_scripts/
│ ├── data/
│ │ ├── bronze/ → Datos originales o crudos (muestra)
│ │ │ └── covid_sample_bronze_10k.csv
│ │ │
│ │ ├── silver/ → Datos procesados (dimensiones e históricos)
│ │ │ ├── dim_clasificacion_final.csv
│ │ │ ├── dim_clasificacion_final_flu.csv
│ │ │ ├── dim_entidad.csv
│ │ │ ├── dim_municipio.csv
│ │ │ ├── dim_nacionalidad.csv
│ │ │ ├── dim_opcion.csv
│ │ │ ├── dim_origen.csv
│ │ │ ├── dim_paises.csv
│ │ │ ├── dim_resultado_antigeno.csv
│ │ │ ├── dim_resultado_lab.csv
│ │ │ ├── dim_resultados_pcr.csv
│ │ │ ├── dim_sector.csv
│ │ │ ├── dim_sexo.csv
│ │ │ ├── dim_tipo_paciente.csv
│ │ │ └── historicos.csv
│ │ │
│ │ └── silver_catalogo_origen/ → Documentación del autor original
│ │ ├── 240708 Catalogos.xlsx
│ │ ├── 240708 Descriptores_.xlsx
│ │ └── Actualizaciones en la presentación de información referente a casos de COVID.pdf
| |
│ └── scripts/
│ | ├── python/
│ | | └── bronze_to_silver_full.py
| |
│ | ├── sql/
│ | | ├── silver_structure_database.sql
│ | | ├── silver_dimensiones.sql
│ | | └── silver_historicos.sql
│ 
├── diagramas/
| ├── Bronze_DL.png
│ ├── CreateDBSilver.png
│ └── InsertSilver.png
│ │
├── metadata/
│ ├── data_dictionary.xlsx
│ ├── provenance.yaml
│ └── schema_documentation.xlsx
│
└── (📦 Enlace de descarga completa: OneDrive)


```

## 🧩 Ejecución paso a paso

### 1️⃣ Crear la base de datos

```bash
createdb -U tu_usuario base_nueva
```

### 2. Crea la estructura de las tablas 

Ejecuta el script SQL silver_structure_database.sql

```bash
 psql -U tu_usuario -d base_nueva -f silver_structure_database.sql
```

### 3. Poblar las dimensiones

Una vez creada la estructura, inserta las tablas maestras:

```bash
psql -U tu_usuario -d base_nueva -f silver_dimensiones.sql
```

### 4. Poblar los datos históricos

### Opción A – Cargar muestra pequeña (para pruebas):
```
Abrir el archivo bronze_to_silver_full.py y agregar path del archivo de muestra : "covid_sample_bronze_10k.csv"
```
### Opción B – Cargar datos completos (requiere el .sql de 13 GB descargado desde OneDrive):

```bash
psql -U tu_usuario -d base_nueva -f silver_historicos.sql
```

🧾 Créditos y referencias

```
Fuente de datos: Dirección General de Epidemiología – Gobierno de México
https://www.gob.mx/salud/documentos/datos-abiertos-152140

Autor: Luis Francisco Gordillo Salinas
```
