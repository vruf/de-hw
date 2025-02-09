```sql
CREATE OR REPLACE EXTERNAL TABLE bigquery-450420.yellow_data_dataset.taxi_yellow_data
OPTIONS (
  format = 'parquet',
  uris = ['gs://ducket_1_123/yellow_tripdata_2024-*.parquet']
);


CREATE OR REPLACE TABLE bigquery-450420.yellow_data_dataset.taxi_yellow_data_regular AS
SELECT * FROM bigquery-450420.yellow_data_dataset.taxi_yellow_data;

CREATE MATERIALIZED VIEW bigquery-450420.yellow_data_dataset.taxi_yellow_data_materialized AS
SELECT * FROM bigquery-450420.yellow_data_dataset.taxi_yellow_data_regular; 


SELECT COUNT(DISTINCT(PULocationID)) FROM bigquery-450420.yellow_data_dataset.taxi_yellow_data;
SELECT COUNT(DISTINCT(PULocationID)) FROM bigquery-450420.yellow_data_dataset.taxi_yellow_data_materialized;

SELECT PULocationID FROM bigquery-450420.yellow_data_dataset.taxi_yellow_data_regular;
SELECT PULocationID, DOLocationID FROM bigquery-450420.yellow_data_dataset.taxi_yellow_data_regular;

SELECT COUNT(*) FROM bigquery-450420.yellow_data_dataset.taxi_yellow_data_regular WHERE fare_amount  = 0;

CREATE OR REPLACE TABLE bigquery-450420.yellow_data_dataset.taxi_yellow_data_regular_partitioned_clustered
PARTITION BY DATE(tpep_dropoff_datetime)
CLUSTER BY VendorID AS
SELECT * FROM bigquery-450420.yellow_data_dataset.taxi_yellow_data_regular;

SELECT DISTINCT(VendorID) FROM bigquery-450420.yellow_data_dataset.taxi_yellow_data_materialized WHERE DATE(tpep_dropoff_datetime) BETWEEN '2024-03-01' AND '2024-03-15';
SELECT DISTINCT(VendorID) FROM bigquery-450420.yellow_data_dataset.taxi_yellow_data_regular_partitioned_clustered WHERE DATE(tpep_dropoff_datetime) BETWEEN '2024-03-01' AND '2024-03-15';

SELECT COUNT(*) FROM bigquery-450420.yellow_data_dataset.taxi_yellow_data_materialized;
```