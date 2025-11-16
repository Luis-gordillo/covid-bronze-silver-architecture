# 🦠 COVID Bronze-Silver Architecture

Arquitectura reproducible para limpieza, normalización y modelado analítico de datos COVID-19 (México)

Este repositorio demuestra una arquitectura Bronze → Silver diseñada para manejar datos epidemiológicos masivos (~65 millones de registros) provenientes de la Dirección General de Epidemiología (Gobierno de México).
Incluye:

- Datos crudos (muestra)
- Datos Silver normalizados
- Estructura SQL para reproducir la base completa
- Script de transformación Bronze → Silver
- Diagramas de linaje y diccionarios de datos
- Dump SQL grande para cargas completas (vía OneDrive)

## 📥 Solicitud de acceso al dataset completo (dump de 13 GB)

Debido al tamaño y naturaleza de los datos (65M+ registros), la base de datos completa no puede alojarse directamente en GitHub.

Para acceder al paquete completo que contiene:

- silver_historicos.sql (dump de ~13 GB)

- Archivos CSV completos de dimensiones

- Documentación original del proveedor

- Paquete Silver completo

Puedes solicitar acceso mediante este enlace:

👉 Formulario de solicitud de acceso
https://docs.google.com/forms/d/e/1FAIpQLSew57U7NzttwuoXqmEUwoCdiUanNWXS6EmaicdqKrRaqiWdDA/viewform?usp=publish-editor

## 📂 Estructura del repositorio

```
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

# 🧩 Descripción del dataset

Fuente oficial: Dirección General de Epidemiología – Gobierno de México
https://www.gob.mx/salud/documentos/datos-abiertos-152140

- Registros: ~65 millones
- Año de cobertura: 2020–2025
- Valores especiales incluidos por el proveedor

Frecuencia de actualización: periódica según publicación oficial

| Valor      | Significado                              |
| ---------- | ---------------------------------------- |
| 97         | No aplica                                |
| 99         | Dato desconocido                         |
| 997        | Sin información / no realizado           |
| 9999-99-99 | Fecha inválida → convertida a 9999-12-31 |

## 🏗️ Proceso Bronze → Silver
### 🥉 Bronze

- Datos crudos tal como se publican:
- Columnas variables por periodo
- Fechas inconsistentes
- Tipos no normalizados
- Códigos numéricos sin documentación contextual

### 🥈 Silver

Transformaciones aplicadas:

- Tipificación y casting
- Estandarización de fechas
- Mapeo contra catálogos oficiales
- Normalización a modelo estrella
- Creación de dimensiones
- Limpieza de valores sentinel
- Eliminación de duplicados

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

## 📘 Metadatos incluidos
✔ Diccionario de datos (data_dictionary.xlsx)

- Tipos
- Fuentes
- Dominios
- Descripción de cada atributo

✔ Documentación de schema (schema_documentation.xlsx)

- Mapeo de tablas
- Llaves primarias y foráneas
- Dependencias

✔ Provenance (provenance.yaml)

- Trazabilidad completa de:
- versiones
- hashes
- fechas
- fuentes

## ⚖️ Consideraciones éticas

- Datos oficiales, anónimos y públicos
- Uso permitido: educativo, científico, analítico
- No apto para diagnóstico clínico
- Se siguen normas de manejo ético basadas en datos abiertos

## 📚 Cómo citar este repositorio

Gordillo Salinas, L.F. (2025).
COVID Bronze–Silver Architecture: Ingesta, normalización y modelado analítico de datos COVID-19 (México).
GitHub: https://github.com/Luis-gordillo/covid-bronze-silver-architecture

Cita de la fuente de datos:
Dirección General de Epidemiología – Gobierno de México.
Datos Abiertos COVID-19. https://www.gob.mx/salud/documentos/datos-abiertos-152140

🧾 Licencia

Código: MIT

Datos y documentación: CC BY 4.0

👤 Autor y contacto

Autor: Luis Francisco Gordillo Salinas
GitHub: https://github.com/Luis-gordillo

Email: (agrega tu correo si quieres)
