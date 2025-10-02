
## ✅ `README.md`

```markdown
# Laboratorio DB - COVID-19 (OWID Dataset)

Este proyecto implementa una base de datos en **PostgreSQL** usando el dataset público de [Our World in Data (COVID-19)](https://github.com/owid/covid-19-data).  
Se ejecuta en contenedores con **Docker Compose**, y permite cargar más de **400k filas** para consultas y análisis posteriores (ej. con Chart.js en frontend).

---

## 🚀 Requisitos

- [Docker](https://www.docker.com/get-started)  
- [Docker Compose](https://docs.docker.com/compose/)  
- Git

---

## 📂 Estructura del proyecto

```

laboratorio-db/
├── docker-compose.yml          # Configuración de servicios (Postgres)
├── .gitignore                  # Ignora CSVs pesados
├── init-db/
│   ├── schema.sql              # Definición de la tabla covid_data (67 columnas)
│   ├── load.sql                # Script de carga del CSV en la tabla
│   └── owid-covid-data.csv     # (NO se sube al repo, se descarga localmente)
└── docs/
└── dataset_link.txt        # Contiene la URL oficial del dataset

````

---

## ⚙️ Instrucciones de uso

1. **Clonar el repositorio**
   ```bash
   git clone https://github.com/tu-usuario/laboratorio-db.git
   cd laboratorio-db
````

2. **Descargar el dataset (no está en el repo)**

   * Opción 1: Descargar manualmente el archivo CSV desde [este enlace](https://github.com/owid/covid-19-data/raw/master/public/data/owid-covid-data.csv) y guardarlo como:

     ```
     init-db/owid-covid-data.csv
     ```


3. **Levantar la base de datos**

   ```bash
   docker compose up --build
   ```

4. **Entrar a PostgreSQL**

   ```bash
   docker exec -it lab_postgres psql -U labuser -d labdb
   ```

5. **Cargar la tabla y los datos (manual en Windows)**
   Dentro de `psql`:

   ```sql
   \i /docker-entrypoint-initdb.d/schema.sql
   \i /docker-entrypoint-initdb.d/load.sql
   ```

6. **Verificar datos**

   ```sql
   \dt
   SELECT COUNT(*) FROM covid_data;
   SELECT location, date, total_cases, total_deaths FROM covid_data LIMIT 5;
   ```

---

## 🔍 Consultas de ejemplo

```sql
-- Total de filas
SELECT COUNT(*) FROM covid_data;

-- Casos totales por continente
SELECT continent, MAX(total_cases) AS casos_totales
FROM covid_data
WHERE continent IS NOT NULL
GROUP BY continent
ORDER BY casos_totales DESC;

-- Top 5 países con más muertes
SELECT location, MAX(total_deaths) AS muertes_totales
FROM covid_data
WHERE continent IS NOT NULL
GROUP BY location
ORDER BY muertes_totales DESC
LIMIT 5;

-- Top 5 países con más vacunados
SELECT location, MAX(people_vaccinated) AS vacunados
FROM covid_data
WHERE continent IS NOT NULL
  AND people_vaccinated IS NOT NULL
GROUP BY location
ORDER BY vacunados DESC
LIMIT 5;
```

---

## 📌 Notas importantes

* El archivo `owid-covid-data.csv` **NO se sube al repositorio** (está en `.gitignore`).
* Cada integrante debe descargar el dataset localmente antes de ejecutar el contenedor.
* La tabla `covid_data` incluye **67 columnas**, reflejando exactamente el encabezado del dataset OWID.

---

✍️ Autor: *Jean Marthé, Oscar Gil, Juan Palacios*
📅 Proyecto de laboratorio - Universidad del Norte

```

