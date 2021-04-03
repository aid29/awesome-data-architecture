# Build a data lake using Amazon Kinesis Data Streams for Amazon DynamoDB and Apache Hudi

## Use case Type
POC

## Reference link
https://aws.amazon.com/blogs/big-data/build-a-data-lake-using-amazon-kinesis-data-streams-for-amazon-dynamodb-and-apache-hudi/

## Team
NA

## Cloud Tech
`AWS`

## Input data
Fake online shopping order transaction data stored in `DynamoDB`

## Data final storage and usage
Parquet data on S3 queryable in `Athena`, for sales analysis.

## Injection process & Data flow
1. Send DynamoDB tables data to `Kinesis Data Streams`, Kinesis sink the stream to S3 raw Parquet.

2. Run Spark job with `hudi-spark-bundle.jar`, load the raw Parquet from S3 with Spark DF (Parquet format), 
   and write data into the Hudi dataset (org.apache.hudi format).

## Data transformation
None, data in DynamoDB and Athena are the same schema, one to one record mapping.

## Process scheduling/orchestration
NA

## Dataset Schema
Manually created DynamoDB table and Athena table

## Data Dictionary and Glossary
NA

## Data quality&validation
NA

## Testing
NA

## Monitoring alerting
NA

## Machine learning parts
NA

## Extension and Challenge
This example demoed also the `change data capture` (`CDC`) feature. This essentially handle by the `Apache Hudi`,
when the records in DynamoDB changed the value (identified by RECORDKEY_FIELD_OPT_KEY in hudiOptions),
Hudi will automatically update the value for the records in Parquet objects on S3 and that reflect from the Athena tables.

In the modern Data Lake architecture, data is usually immutable for maximising the scalability and performance. 
It will be good to see how Hudi handles this kind of updates. Imagine we have millions of records daily, 
and the Operations team occasionally update 10+ records per day, how Hudi handles this kind of scenario efficiently?

Another point to mention is we should keep all the CDC changes for our tables. It will be useful for the audit purpose or the business needs. 
I think Hudi already provides this kind of feature for Hudi tables.

