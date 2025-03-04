# Scalable ETL Data Pipeline

## Overview

This project implements a **scalable Extract, Transform, Load (ETL) data pipeline** designed to handle large datasets efficiently. The pipeline extracts data from various sources, transforms it according to specified business rules, and loads it into a data warehouse for analysis. The architecture ensures scalability, reliability, and maintainability, leveraging modern data engineering tools and practices.

## Architecture

The ETL pipeline comprises the following components:

1. **Data Ingestion**: Utilizes a custom Python wrapper to interact with the GoodReads API, collecting data such as book information, reviews, and ratings.

2. **Data Storage**:
   - **Landing Zone**: Raw data is initially stored locally before being uploaded to the **Landing Bucket** on AWS S3.
   - **Working Zone**: Data is moved from the landing zone to the working zone on S3 for processing.

3. **Data Processing**:
   - **ETL Jobs**: Implemented using Apache Spark, these jobs are scheduled via Apache Airflow to run at regular intervals (e.g., every 10 minutes).
   - **Transformations**: Data is cleaned, normalized, and transformed to fit the target schema.

4. **Data Loading**: Processed data is loaded into an Amazon Redshift data warehouse for analytical querying.

5. **Analytics Module**: Provides tools and scripts for data analysis and visualization.

## Project Structure

The repository is organized as follows:

```
Scalable-ETL-Data-Pipeline/
│
├── SampleData/               # Sample datasets for testing
│
├── Utility/                  # Utility scripts and modules
│   ├── s3_module.py          # Functions for S3 interactions
│   └── ...
│
├── airflow/                  # Apache Airflow configurations
│   ├── dags/                 # Directed Acyclic Graphs (DAGs)
│   └── plugins/              # Custom Airflow plugins
│
├── docs/                     # Documentation and resources
│   └── architecture.png      # Architecture diagram
│
├── goodreadsfaker/           # Module to simulate GoodReads API responses
│   └── faker.py
│
├── src/                      # Source code for ETL processes
│   ├── extract.py            # Data extraction logic
│   ├── transform.py          # Data transformation logic
│   └── load.py               # Data loading logic
│
├── .gitignore                # Git ignore file
├── LICENSE                   # License information
└── README.md                 # Project overview and instructions
```

## Setup Instructions

### Prerequisites

- **Python 3.7+**: Ensure Python is installed.
- **Apache Spark**: For data processing tasks.
- **Apache Airflow**: For workflow orchestration.
- **AWS Account**: Access to AWS services like S3 and Redshift.

### Installation Steps

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/AlexShao1116/Scalable-ETL-Data-Pipeline.git
   cd Scalable-ETL-Data-Pipeline
   ```

2. **Set Up Virtual Environment**:
   ```bash
   python3 -m venv venv
   source venv/bin/activate   # On Windows: venv\Scriptsctivate
   ```

3. **Install Dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

4. **Configure AWS Credentials**:
   - Ensure your AWS credentials are set up, typically in `~/.aws/credentials`.
   - Update the `Utility/s3_module.py` with your S3 bucket names and paths.

5. **Initialize Airflow**:
   ```bash
   airflow db init
   airflow users create --username admin --firstname Admin --lastname User --role Admin --email admin@example.com
   ```

6. **Start Airflow Scheduler and Webserver**:
   ```bash
   airflow scheduler
   airflow webserver --port 8080
   ```

7. **Deploy Airflow DAGs**:
   - Place your DAG files in the `airflow/dags/` directory.
   - Access the Airflow web interface at `http://localhost:8080` to monitor and trigger DAGs.

## Usage

1. **Data Ingestion**:
   - Run the `goodreadsfaker/faker.py` script to simulate data ingestion from the GoodReads API.

2. **ETL Process**:
   - Airflow schedules and triggers the ETL jobs as defined in the DAGs.
   - Monitor the ETL process via the Airflow web interface.

3. **Data Analysis**:
   - Use SQL clients or BI tools to query the data in Amazon Redshift.
   - Sample queries can be found in the `docs/queries.sql` file.

## Contributing

Contributions are welcome! Please follow these steps:

1. **Fork the Repository**: Click the "Fork" button on GitHub.
2. **Create a Feature Branch**:
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. **Commit Your Changes**:
   ```bash
   git commit -m "Description of your feature"
   ```
4. **Push to Your Fork**:
   ```bash
   git push origin feature/your-feature-name
   ```
5. **Submit a Pull Request**: Navigate to the original repository and click "New Pull Request".

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Acknowledgments

Special thanks to the contributors and the open-source community for their invaluable resources and tools.
