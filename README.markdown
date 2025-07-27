# Apache Hive Tutorial Repository

Welcome to the companion GitHub repository for **"Hive: The Data Architect’s Blueprint – A Comprehensive Tutorial on Apache Hive"**. This repository contains all code snippets, a sample dataset generator, and a Docker setup to help you explore Apache Hive’s capabilities, from installation to advanced performance tuning. Designed for data engineers and analysts, this repository allows you to set up a Hive environment and run the tutorial’s examples in minutes.

The tutorial covers Hive installation, data types, bucketing, partitioning, HiveQL operations, operators, performance tuning, and more, using the unique `EcoSalesTrend` dataset. Clone this repository to test the code and bring the tutorial to life!

## Repository Structure

| Directory/File | Description |
|----------------|-------------|
| `scripts/` | HQL scripts for creating tables, loading data, running queries, using UDFs, and optimizing Hive. |
| `data/` | Python script (`generate_dataset.py`) to create the `EcoSalesTrend` dataset. |
| `Dockerfile` | Docker configuration to set up a Hive environment with MySQL Metastore. |
| `README.md` | This file, providing setup and usage instructions. |

## Prerequisites

- **Docker**: Installed on your system ([install Docker](https://docs.docker.com/get-docker/)).
- **Git**: To clone the repository ([install Git](https://git-scm.com/downloads)).
- **Python 3**: For generating the dataset (optional, included in Docker image).
- **Internet Access**: To pull the `apache/hive:3.1.3` Docker image.

## Setup Instructions

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/yourusername/hive-tutorial.git
   cd hive-tutorial
   ```

2. **Build the Docker Image**:
   - Build the Hive environment with MySQL and Python dependencies:
     ```bash
     docker build -t hive-tutorial .
     ```

3. **Run the Docker Container**:
   - Start the container, exposing HiveServer2’s port (10000):
     ```bash
     docker run -d -p 10000:10000 --name hive-container hive-tutorial
     ```

4. **Generate the Dataset**:
   - Inside the container, run the Python script to create `sales_2025-07.csv`:
     ```bash
     docker exec -it hive-container bash
     python3 /data/generate_dataset.py
     ```
   - This generates a CSV file with 1000 rows of sales data (columns: `order_id`, `customer_id`, `product`, `amount`, `dt`, `region`).

5. **Run HQL Scripts**:
   - Connect to HiveServer2 using `beeline`:
     ```bash
     docker exec -it hive-container beeline -u jdbc:hive2://localhost:10000
     ```
   - Execute scripts in the `scripts/` directory:
     ```sql
     !run /scripts/create_tables.hql
     !run /scripts/load_data.hql
     !run /scripts/queries.hql
     !run /scripts/udf_example.hql
     !run /scripts/optimization.hql
     ```

## Usage Examples

### Create Tables
Run `create_tables.hql` to set up the `EcoMart` database and tables:
```sql
CREATE DATABASE EcoMart;

CREATE TABLE EcoCustomers (
    customer_id INT,
    name STRING,
    preferences MAP<STRING, INT>,
    addresses ARRAY<STRUCT<street:STRING, city:STRING>>
)
STORED AS ORC;

CREATE TABLE EcoSalesTrend (
    order_id INT,
    customer_id INT,
    product STRING,
    amount DOUBLE
)
PARTITIONED BY (dt STRING, region STRING)
CLUSTERED BY (customer_id) INTO 10 BUCKETS
STORED AS ORC;
```

### Run Queries
Execute `queries.hql` to analyze the `EcoSalesTrend` dataset:
```sql
SELECT product, SUM(amount) AS total_sales
FROM EcoSalesTrend
WHERE dt = '2025-07-01' AND region = 'NY'
GROUP BY product;
```

### Optimize Hive
Run `optimization.hql` to enable Tez and vectorization:
```sql
SET hive.execution.engine=tez;
SET hive.vectorized.execution.enabled=true;
```

## Troubleshooting

- **Docker Port Conflict**: If port 10000 is in use, change it (e.g., `-p 10001:10000`).
- **MySQL Errors**: Ensure the MySQL service starts correctly in the container (`service mysql start`).
- **File Not Found**: Verify that `sales_2025-07.csv` is generated in `/data/` before loading data.
- **Beeline Connection**: Confirm HiveServer2 is running (`docker ps`) and use `jdbc:hive2://localhost:10000`.

## Converting the Tutorial to .docx

The tutorial (`apache_hive_tutorial.md`) is provided in Markdown format. To convert it to `.docx` without installing software:
1. Visit [www.cloudxdocs.com](https://www.cloudxdocs.com).
2. Upload `apache_hive_tutorial.md` or paste its content.
3. Select `.docx` as the output format and enable the table of contents.
4. Download the `.docx` file and verify formatting in Microsoft Word or Google Docs.
5. Adjust fonts (e.g., Calibri) or colors (e.g., green for Pro-Tips) for a polished look.

Alternative tools: [mconverter.eu](https://mconverter.eu) or [tomarkdown.dev](https://tomarkdown.dev).

## Additional Notes

- **Dataset**: The `EcoSalesTrend` dataset is a fictional retail sales dataset designed for the tutorial, ensuring originality.
- **Verifiability**: Run the HQL scripts in the Docker environment to verify query outputs, demonstrating the tutorial’s accuracy.
- **Customization**: Modify table names (e.g., `EcoSalesTrend` to `GreenSalesWave`) or add your own data to the dataset generator for personalization.
- **Feedback**: For issues or enhancements, create a GitHub issue or contact [your email].

## License

This repository is licensed under the MIT License. See [LICENSE](LICENSE) for details.

---

*Created for the Apache Hive tutorial, July 2025, by Your Name.*