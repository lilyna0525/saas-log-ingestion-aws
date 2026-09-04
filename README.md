# SaaS Log Ingestion Pipeline on AWS

A serverless SaaS event log ingestion pipeline built with Amazon API Gateway, Amazon Kinesis Data Streams, and Amazon S3.

This project simulates an external SaaS service sending application event logs to an AWS-based data ingestion pipeline through HTTP POST requests.

---

## Overview

Modern applications and SaaS platforms continuously generate large volumes of event and application logs.

This project demonstrates how external event data can be collected through an HTTP endpoint and streamed into AWS for scalable storage.

The pipeline receives `application/x-www-form-urlencoded` requests through Amazon API Gateway, converts the incoming request data into a format suitable for downstream processing, and sends the data to Amazon Kinesis Data Streams.

The collected data is then delivered to Amazon S3 for persistent storage.

### Project Objectives

- Receive external event logs through an HTTP API
- Handle `application/x-www-form-urlencoded` request data
- Transform incoming request data through API Gateway
- Stream event data using Amazon Kinesis Data Streams
- Store collected logs in Amazon S3
- Validate the end-to-end ingestion pipeline using `curl`
- Monitor and verify successful data ingestion

---

## Architecture

```text
External Client / Streamlit
          |
          | HTTPS POST
          v
+-------------------------+
|     Amazon API Gateway  |
|                         |
|  Request Transformation |
+------------+------------+
             |
             v
+-------------------------+
| Amazon Kinesis Data     |
| Streams                 |
+------------+------------+
             |
             v
+-------------------------+
| Amazon Kinesis Data     |
| Firehose                |
+------------+------------+
             |
             v
+-------------------------+
|       Amazon S3         |
|     formevent bucket    |
+-------------------------+

             |
             v
+-------------------------+
|    Amazon CloudWatch    |
|       Monitoring        |
+-------------------------+
```

## Architecture Components

| Component                        | Role                                                                    |
| -------------------------------- | ----------------------------------------------------------------------- |
| **External Client / Streamlit**  | Simulates an external application or SaaS service generating event logs |
| **Amazon API Gateway**           | Receives HTTP POST requests from the external client                    |
| **API Gateway Mapping Template** | Transforms the incoming request data before sending it to Kinesis       |
| **Amazon Kinesis Data Streams**  | Provides the real-time streaming layer for incoming event data          |
| **Amazon Kinesis Data Firehose** | Delivers streaming data to Amazon S3                                    |
| **Amazon S3**                    | Provides persistent storage for the collected event logs                |
| **Amazon CloudWatch**            | Used to monitor and verify API activity                                 |
