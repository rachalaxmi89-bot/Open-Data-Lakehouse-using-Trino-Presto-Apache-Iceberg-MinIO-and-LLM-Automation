# **Module 5 Assignment — Steps Taken**





## Task 1: Lakehouse Environment Setup

## 

* Downloaded professor's Module5\_Docker\_Environment.zip and extracted to C:\\Users\\5th-Project\\
* Created .env file by copying .env.example
* Ran docker compose up -d --build to start all containers
* Fixed Trino worker node.properties issue (Windows permission + wrong node.environment)
* Verified all containers running with docker ps
* Opened MinIO Console at http://localhost:9001 (minio / minio123)
* Opened Trino UI at http://localhost:13579





## Task 2: Data Ingestion into Iceberg

## 

* Uploaded sales.csv to MinIO warehouse bucket via browser console
* Connected to Trino CLI: docker exec -it trino-coordinator trino
* Created Iceberg schema:

&#x09;CREATE SCHEMA IF NOT EXISTS iceberg.sales\_db

&#x09;WITH (location = 's3://warehouse/sales\_db/');

* Created Iceberg table with PARQUET format
* Loaded 19 rows using INSERT INTO iceberg.sales\_db.sales
* Verified with SELECT \* FROM iceberg.sales\_db.sales LIMIT 10





## Task 3: Analytical Queries using Trino



Ran 3 SQL queries on the Iceberg table:



**Total Revenue:**





SELECT SUM(price \* quantity) AS total\_revenue

FROM iceberg.sales\_db.sales;



**Category-wise Revenue:**



SELECT category, SUM(price \* quantity) AS category\_revenue

FROM iceberg.sales\_db.sales

GROUP BY category ORDER BY category\_revenue DESC;



**Top Selling Products by Quantity:**



SELECT product, SUM(quantity) AS total\_quantity\_sold

FROM iceberg.sales\_db.sales

GROUP BY product ORDER BY total\_quantity\_sold DESC;



## Task 4: LLM-based Query Automation

## 

* Pulled Ollama model: docker exec -it ollama ollama pull qwen2.5:3b
* Installed Python dependencies: pip install requests trino pandas tabulate
* Ran the LLM agent: python llm\_pipeline\_agent.py
* Demonstrated with 3 natural language questions:



"What is the total revenue from all sales?"

"Show me the revenue broken down by category"

"Which are the top selling products by quantity sold?"





Agent generated SQL, executed on Trino, and returned AI insights for each question







