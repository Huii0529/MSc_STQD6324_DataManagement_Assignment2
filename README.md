# MSc_STQD6324_DataManagement_Assignment2

# MovieLens 100K Analytics with Apache Spark and Apache Cassandra

## Overview

This project demonstrates the integration of **Apache Spark** and **Apache Cassandra** to build a scalable data analytics pipeline using the **MovieLens 100K** dataset. Apache Spark is used for distributed data processing and analytics through DataFrames and Spark SQL, while Apache Cassandra serves as the NoSQL database for storing and retrieving structured data.

The project implements five analytical tasks specified in the assignment by importing, transforming, storing, and querying data from the MovieLens dataset.

---

## Project Objectives

The objectives of this project are to:

* Configure Apache Cassandra and create the required database schema.
* Import MovieLens data into Cassandra using Apache Spark.
* Process structured data using Spark DataFrames.
* Perform analytical queries using Spark SQL.
* Demonstrate the integration of Apache Spark with Apache Cassandra for big data analytics.

---

## Technologies Used

* Apache Spark
* PySpark
* Apache Cassandra
* Cassandra Query Language (CQL)
* Spark SQL
* Spark DataFrames
* DataStax Spark Cassandra Connector
* Python 3
* Google Colab
* PuTTY (Linux server access)

---

## Dataset

This project uses the **MovieLens 100K Dataset** from GroupLens.

**Dataset:** https://grouplens.org/datasets/movielens/

The following files are used:

| File     | Description                  |
| -------- | ---------------------------- |
| `u.user` | User demographic information |
| `u.data` | User ratings                 |
| `u.item` | Movie information and genres |

---

# Project Structure

```text
MovieLens-Spark-Cassandra/
│
├── README.md
├── report.pdf
├── create_keyspace.cql
├── users.py
├── ratings.py
├── movies.py
├── analysis.py
├── ml-100k/
│   ├── u.user
│   ├── u.data
│   └── u.item
└── screenshots/
```

> **Note:** The filenames above are examples. Update them to match the actual files in your repository.

---

# Development Environment

This project was developed using two different platforms.

## 1. PuTTY

PuTTY was used to connect to a Linux environment where Apache Cassandra was installed and managed. The following tasks were performed using Cassandra Query Language (CQL):

* Creating the keyspace
* Creating Cassandra tables
* Verifying table structures
* Viewing stored data
* Managing the Cassandra database

## 2. Google Colab

Google Colab was used to develop and execute the Apache Spark application.

The Spark application performs:

* Reading MovieLens datasets
* Creating Spark DataFrames
* Data cleaning and transformation
* Writing DataFrames into Cassandra
* Reading data back from Cassandra
* Executing Spark SQL analytical queries

---

# Important Notes

Running **Apache Cassandra directly in Google Colab** may not work consistently because Google Colab is a temporary cloud environment whose software configuration changes over time.

Possible issues include:

* Java version incompatibility
* Cassandra startup failures
* Spark-Cassandra connector compatibility
* Background service restrictions
* Runtime resets

If you plan to reproduce this project in Google Colab, you may need to:

* Install a compatible Java Development Kit (JDK).
* Install Apache Cassandra manually.
* Configure the Spark Cassandra Connector.
* Mount Google Drive if the dataset is stored there.
* Restart the runtime after modifying system configurations.

For a more stable setup, it is recommended to run Apache Cassandra on:

* A local Linux machine
* Windows Subsystem for Linux (WSL)
* Docker
* A virtual machine
* A remote Linux server (accessed through PuTTY)

---

# Cassandra Database Setup

Start Cassandra:

```bash
cqlsh
```

## Create Keyspace

```sql
CREATE KEYSPACE movielens
WITH replication = {
    'class':'SimpleStrategy',
    'replication_factor':1
};
```

## Select Keyspace

```sql
USE movielens;
```

## Create Users Table

```sql
CREATE TABLE users(
    userID INT PRIMARY KEY,
    age INT,
    gender TEXT,
    occupation TEXT,
    zip TEXT
);
```

## Create Ratings Table

```sql
CREATE TABLE ratings(
    userID INT,
    movieID INT,
    rating INT,
    timestamp BIGINT,
    PRIMARY KEY(userID, movieID)
);
```

## Create Movies Table

```sql
CREATE TABLE movies(
    movieID INT PRIMARY KEY,
    title TEXT,
    releaseDate TEXT,
    videoReleaseDate TEXT,
    imdbURL TEXT,
    unknown INT,
    action INT,
    adventure INT,
    animation INT,
    childrens INT,
    comedy INT,
    crime INT,
    documentary INT,
    drama INT,
    fantasy INT,
    filmNoir INT,
    horror INT,
    musical INT,
    mystery INT,
    romance INT,
    sciFi INT,
    thriller INT,
    war INT,
    western INT
);
```

---

# Apache Spark Workflow

The Apache Spark pipeline performs the following steps:

1. Create a SparkSession.
2. Connect Spark to Apache Cassandra.
3. Load MovieLens datasets.
4. Parse raw text into structured records.
5. Convert records into Spark DataFrames.
6. Store DataFrames in Cassandra.
7. Read data from Cassandra.
8. Register temporary SQL views.
9. Perform analytical queries using Spark SQL.

---

# Data Pipeline

```text
MovieLens Dataset
        │
        ▼
Read Raw Files
        │
        ▼
Parse Records
        │
        ▼
Spark DataFrames
        │
        ▼
Write to Cassandra
        │
        ▼
Read from Cassandra
        │
        ▼
Spark SQL Analysis
        │
        ▼
Results
```

---

# Analytical Tasks

The following analytical tasks were implemented.

## Task 1

### Calculate the average rating for each movie

The ratings dataset is grouped by movie ID and the average rating is computed using Spark DataFrame aggregation.

---

## Task 2

### Identify the top ten movies with the highest average ratings

Movies are sorted based on their average ratings in descending order, and the top ten highest-rated movies are displayed.

---

## Task 3

### Identify users who have rated at least 50 movies and determine their favourite movie genre

The application:

* Identifies users with at least 50 ratings.
* Joins ratings with movie information.
* Counts the number of ratings for each genre.
* Determines each user's most frequently rated genre.

---

## Task 4

### Find all users who are less than 20 years old

Example Spark SQL:

```sql
SELECT *
FROM users
WHERE age < 20;
```

---

## Task 5

### Find users whose occupation is "scientist" and whose age is between 30 and 40 years old

Example Spark SQL:

```sql
SELECT *
FROM users
WHERE occupation='scientist'
AND age BETWEEN 30 AND 40;
```

---

# Spark Features Demonstrated

This project demonstrates the use of:

* SparkSession
* Spark Context
* Spark DataFrames
* DataFrame Transformations
* DataFrame Aggregations
* DataFrame Filtering
* DataFrame Joins
* GroupBy Operations
* Spark SQL
* Temporary Views
* Cassandra Integration

---

# Results

The project successfully demonstrates the integration of Apache Spark and Apache Cassandra to perform distributed data processing and NoSQL data storage.

The analytical tasks produce:

* Average movie ratings
* Top 10 highest-rated movies
* Favourite movie genre for active users
* Users younger than 20 years old
* Scientists aged between 30 and 40 years old

Since the execution output is extensive, only selected outputs are included in this repository.

For the complete execution results, screenshots, and detailed analysis, please refer to the **PDF report** included in this repository. The report contains:

* Cassandra keyspace creation
* Table creation
* Data import process
* Spark DataFrame operations
* Spark SQL query outputs
* Results for all five analytical tasks
* Execution screenshots and discussion

---

# Learning Outcomes

Through this project, the following concepts were explored:

* Apache Cassandra database design
* Cassandra Query Language (CQL)
* NoSQL data storage
* Apache Spark DataFrames
* Spark SQL
* Distributed data processing
* Data aggregation
* Data transformation
* Spark-Cassandra integration
* Big data analytics pipeline development

---

# Conclusion

This project demonstrates how Apache Spark and Apache Cassandra can be integrated to build an efficient and scalable big data analytics pipeline. Apache Spark provides distributed data processing, DataFrame operations, and SQL-based analytics, while Apache Cassandra offers scalable NoSQL storage for structured datasets.

By leveraging these technologies, the project successfully completes five analytical tasks on the MovieLens 100K dataset, including average movie rating computation, top-rated movie identification, user preference analysis, and demographic filtering. The project highlights the practical application of modern big data frameworks for data ingestion, transformation, storage, and analytical processing.

---

## Author

**Name:** FANG JIA HUI
**Matrics No.:** P167347
**Course:** STQD6324 Data Management
**Dataset:** MovieLens 100K Dataset by GroupLens Research
**Repository:** *(Add your GitHub repository link here if applicable)*
