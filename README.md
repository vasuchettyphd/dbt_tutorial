# 🏡 DBT House Prices Project (Docker + Postgres)

This project is a minimal example to help you learn how to use [DBT](https://www.getdbt.com/) (Data Build Tool) with a PostgreSQL database using Docker Compose.

It includes:
- A preloaded seed dataset (`house_prices.csv`)
- A basic DBT model that calculates average sale price by neighborhood
- Docker containers for PostgreSQL and DBT

---

## 🚀 Quick Start

### 1. ✅ Clone the Repository

```bash
git clone https://github.com/vasuchettyphd/dbt_tutorial.git
cd dbt_tutorial
```

---

### 2. 🐳 Start the Environment

```bash
docker-compose up --build
```

This will:
- Start the PostgreSQL container (`dbt_postgres`)
- Start the DBT CLI container (`dbt_cli`)
- Run `dbt debug` and `dbt seed` automatically

---

### 3. 🧪 Run DBT Models and Tests

Enter the DBT container:

```bash
docker exec -it dbt_cli bash
```

Then inside the container:

```bash
dbt debug       # check config and connection
dbt run         # build models (e.g., create views)
```

---

## 🗞 What's in the Project?

### 📂 `data/house_prices.csv` & `house_prices_large.csv`

Seeded datasets with house sale information:

- `neighborhood`
- `lot_area`
- `year_built`
- `sale_price`

The datasets are:

- `house_prices.csv`: A small, clean dataset for introductory exercises.
- `house_prices_large.csv`: A larger, messier dataset with:
  - 500+ rows
  - Missing values
  - Duplicate rows

These are loaded as separate seed tables and can be used to teach data cleaning, filtering, and testing.

### 📂 `models/avg_sale_price.sql`

```sql
-- Calculates average sale price by neighborhood and overall quality
select
  neighborhood,
  overall_qual,
  avg(sale_price) as avg_price,
  count(*) as num_sales
from {{ ref('house_prices') }}
group by neighborhood, overall_qual
order by avg_price desc
```

## 🔍 Inspect the Results: Use `psql` from Docker

```bash
docker exec -it dbt_postgres psql -U dbt_user -d dbt_db
```

Then inside PostgreSQL:

```sql
\dt      -- List tables
\dv      -- List views
SELECT * FROM avg_sale_price;
```

---

## 🢼 Shut Down

To stop and remove containers and volumes:

```bash
docker-compose down -v
```

---

## 🧠 Want to Learn More?

- [DBT Learn](https://docs.getdbt.com/docs/introduction)
- [DBT CLI Reference](https://docs.getdbt.com/reference/dbt-cli)