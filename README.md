# Introduction

The purpose of this project is to receive invoice data from Message Hub and persist it to a DB2 Warehouse using [IBM WDP Streaming Pipelines](https://datascience.ibm.com/docs/content/streaming-pipelines/overview-streaming-pipelines.html)

# Dependencies

- This project has a dependency on:
  - The deployment of the project [kafka-producer-for-simulated-data](https://github.com/ibm-cloud-streaming-retail-demo/kafka-producer-for-simulated-data)
  - On the dataset `OnlineRetail.csv` that is output from the [dataset-generator](https://github.com/ibm-cloud-streaming-retail-demo/dataset-generator) project.

# Prerequisites

- You have an IBM Cloud account
- You have followed the instructions in the project [dataset-generator](https://github.com/ibm-cloud-streaming-retail-demo/dataset-generator) to create a dataset, `OnlineRetail.json.gz`.
- You have deployed the project [kafka-producer-for-simulated-data](https://github.com/ibm-cloud-streaming-retail-demo/kafka-producer-for-simulated-data) to your IBM Cloud account
- You have some knowledge of DB2 Warehouse on Cloud
- You have some knowledge of [IBM WDP Streaming Pipelines](https://datascience.ibm.com/docs/content/streaming-pipelines/overview-streaming-pipelines.html)

# Develop

None of these steps have been developed.

- Extract dimension tables from OnlineRetail.csv:
  - County: `CountryID int | CountryName string`
  - Product: `StockID int | StockCode string | Dscription string`
  - Time: TBC
  - Customer: `CustomerID int | CustomerName string` (the input data only has customer id, so manually create customer (company) names or use a tool like faker: https://pypi.python.org/pypi/fake-factory/0.2)
  - Store: `StoreID int | StoreName string` (the input date doesn't have storename - we could just use geographic place names, e.g. Bristol, London, ...)
- Write python code to extract the above dimension tables and create the DB2 DDL and insert statements.
- Write fact table DDL, e.g. 
  - `TransactionID int | CountryID int | ProductID int | TimeID int | CustomerID int | StoreID int | Quantity int | UnitPrice decimal` 

- Streaming Pipelines
  - Create a pipeline:  Message Hub -> Python Code -> DB2
  - Python code:
    - In the `init(state)` method, lookup and hold in memory all of the above dimension tables
    - Map each record flowing through to retrieve the ID fields for each of the dimension tables

# Description

This project uses type 1 slowly changing dimensions.  A future improvement will be to use more sophisticated slowly changing dimensions.
