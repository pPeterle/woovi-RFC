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

Thi step is realized with Apache Kafka, because of high throughput, low latency, and strong durability, making it ideal for real-time data pipelines.

### Processing, Normalization, and Transformation

All the following steps are performed in Apache Flink, which is highly effective for data processing, normalization, and transformation due to its real time stream processing capabilities, low latency, and support for complex semantics.
It enables precise handling of data with features like stateful processing, windowing, and exactly once guarantees.

### Anomaly and Fraud Detection

Once the data is "clean", anomaly and fraud detection must be performed. There are several possible approaches:

- **Fixed rules**, for example:
  - Transactions above R$ 100,000,000 must be approved manually.
  - A user who has never made more than 10 transactions suddenly performs 20 in one hour.

- **Artificial intelligence**, trained to identify anomalous behavior.

### Classification of Suspicious Transactions

All suspicious transactions are **flagged**, indicating the level of potential risk.

## Security

- Authentication for client and brokers and RBAC
- Enable ACLs
- SASL
- TLS data transition
- VPC to restrict acces

## Design

I chose the technologies with a focus on being open source and deployable on any cloud platform. The design was created to facilitate fraud detection, which will be carried out using Apache Flink, ideal for real-time data processing—receiving transaction events through Apache Kafka. 
As the data is handled and processed, it should be stored in different databases to facilitate data analysis. It is crucial to maintain a clear distinction between processed and unprocessed data.

![image](https://github.com/user-attachments/assets/137439e9-f815-49cb-847c-3da848fbad12)


