## Abstract
Designing a financial monitoring system for a Woovi.

## Objectives

1. Detect potetinal fraud and anomalies
2. Compliance (LGPD)
3. Generate Alerts
4. Reports for humam review,

## Architecture for Fraud Detection

The goal is to create an architecture that is decoupled from the current codebase, so it can be easily audited.

### Kappa Architeture

Kappa Architecture is a streamlined data processing model designed for real-time data handling, using a single stream processing layer instead of separate batch and streaming layers like in Lambda Architecture.
It processes all data as a continuous stream, allowing historical data to be reprocessed by replaying it through the stream. This approach simplifies system design and reduces maintenance complexity


### Data Ingestion

To achieve the objectives, data ingestion can be performed:

- Through **streaming**, to identify in real time whether a transaction is fraudulent. 
- **At the end of the day**, by validating Woovi's entire *ledger* to ensure that customer wallets and Woovi’s wallet are in sync. (Daily Reconciliation)

### Processing, Normalization, and Transformation

The next step is to process, normalize, and transform the data to facilitate analysis. This step depends on how data is structured in each application.

- Streaming -> Apache Flink
- Batch -> Apache Spark

### Anomaly and Fraud Detection

Once the data is "clean", anomaly and fraud detection must be performed. There are several possible approaches:

- **Fixed rules**, for example:
  - Transactions above R$ 100,000,000 must be approved manually.
  - A user who has never made more than 10 transactions suddenly performs 20 in one hour.

- **Artificial intelligence**, trained to identify anomalous behavior.

### Classification of Suspicious Transactions

All suspicious transactions are **flagged**, indicating the level of potential risk.

## Design

I chose the technologies with a focus on being open source and deployable on any cloud platform. The design was created to facilitate fraud detection, which will be carried out using Apache Flink—ideal for real-time data processing—receiving transaction events through Apache Kafka. 
For the Daily Reconciliation scenario, it is recommended to use Airflow to manage task scheduling, allowing the creation of wallet validation routines. As the data is handled and processed, it should be stored in different databases to facilitate data analysis. It is crucial to maintain a clear distinction between processed and unprocessed data.

![image](https://github.com/user-attachments/assets/5419b25c-7fba-45bf-8b41-b3a4cd836bc0)

