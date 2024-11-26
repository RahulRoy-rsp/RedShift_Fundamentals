# Data Loading in Amazon Redshift
Below are the key methods for loading data into Amazon Redshift.

## 1. **Using the `COPY` Command**

The most efficient way to load large volumes of data into Amazon Redshift is using the `COPY` command, which loads data from **Amazon S3**, **Amazon DynamoDB**, or **SSH**-accessible files. The `COPY` command is optimized for bulk loading and can load data in parallel.

### Syntax:
```sql
COPY table_name
FROM 's3://your-bucket-name/data-file'
CREDENTIALS 'aws_access_key_id=<your-access-key>;aws_secret_access_key=<your-secret-key>'
DELIMITER ',' 
IGNOREHEADER 1;
```

### Key Options:
- **FROM**: Specifies the source location, typically an S3 bucket.
- **CREDENTIALS**: Provides AWS credentials or uses an IAM role to authenticate.
- **DELIMITER**: Specifies the delimiter for CSV or other text files (default is comma).
**IGNOREHEADER**: Ignores the first N rows if they contain headers.

### Example:
```sql
COPY sales_data
FROM 's3://bucket-name/scores_data.csv'
CREDENTIALS 'aws_iam_role=arn:aws:iam::457851214:role/RedshiftRole'
DELIMITER ',' 
IGNOREHEADER 1;
```

## 2. **Using Amazon Redshift Spectrum (For External Tables)**

With Redshift Spectrum, you can run SQL queries directly against data stored in Amazon S3 without needing to load it into Redshift. This is particularly useful for querying large datasets that you don't want to load into Redshift permanently.

### Syntax:
```sql
CREATE EXTERNAL TABLE spectrum.sales_data (
    sale_id INT,
    sale_amount DECIMAL(10, 2),
    sale_date DATE
)
STORED AS PARQUET
LOCATION 's3://your-bucket-name/spectrum-data/';

```

### Example:
```sql
CREATE EXTERNAL TABLE spectrum.sales_data (
    sale_id INT,
    sale_amount DECIMAL(10, 2),
    sale_date DATE
)
STORED AS CSV
LOCATION 's3://my-bucket/spectrum-sales-data/';
```
