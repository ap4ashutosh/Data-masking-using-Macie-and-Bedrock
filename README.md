# 🛡️ PII Masking with AWS Bedrock and Macie: Protecting Your Personal Data Before Analysis 🔒

## Introduction

In today's data-driven world, protecting Personally Identifiable Information (PII) is a top priority. This repository provides a comprehensive solution for masking PII and other sensitive data across various file formats stored in Amazon S3. By following the steps outlined in this guide, you'll ensure that your data is secure before it's analyzed. Let's dive into the process, and don't worry, we'll keep it interesting!

![Architecture Diagram](./arch.png)

## Step 1: Securely Storing Data in S3

First things first, data from various sources lands in a tightly secured S3 bucket. This bucket serves as the initial holding area for your data, which may come in formats like CSV, TSV, JSON, Parquet, and Avro. Think of this bucket as Fort Knox for your raw data. 🚀

### Why Secure S3 Storage?

- **Data Integrity**: Ensures that the data is intact and unaltered.
- **Access Control**: Only authorized personnel and processes can access the data.
- **Compliance**: Helps meet regulatory requirements by protecting sensitive information.

## Step 2: Running Macie to Detect PII

AWS Macie steps in to scan the data for PII. Macie uses pattern matching and machine learning to identify sensitive information. Once it finds PII, it generates a locked result JSON file and stores it in another S3 bucket.

### How Macie Works

1. **Data Classification**: Macie analyzes the data to identify and classify sensitive information.
2. **Pattern Matching**: It uses predefined patterns to detect PII.
3. **Machine Learning**: Enhances detection accuracy by learning from data patterns.

### Why Use Macie?

- **Automated Detection**: Saves time and reduces manual effort.
- **Accuracy**: High detection accuracy with machine learning.
- **Compliance**: Helps meet data protection regulations.

## Step 3: Using EventBridge to Trigger Bedrock Model

As soon as Macie detects PII and generates the result JSON, an EventBridge rule triggers the Bedrock model, Anthropic Haiku. This model creates a Python dictionary indicating the exact locations (rows and columns) of the PII to be masked.

### EventBridge and Bedrock Model

- **EventBridge**: Captures events from S3 and triggers actions.
- **Anthropic Haiku**: A powerful language model that can parse extremely long JSON files and pinpoint PII locations.

### Why Anthropic Haiku?

- **Language Understanding**: Parses complex JSON structures effectively.
- **Precision**: Accurately identifies the locations of sensitive data.
- **Efficiency**: Handles large datasets with ease.

## Step 4: Masking PII with AWS Glue

Next, an EventBridge rule triggers an AWS Glue workflow to mask the identified PII using PySpark. This step modifies the original data to protect sensitive information while retaining its analytical value.

### AWS Glue and PySpark

- **AWS Glue**: A serverless data integration service that prepares data for analysis.
- **PySpark**: An interface for Apache Spark in Python, used for large-scale data processing.

### Masking Process

1. **Extract**: Pulls the data from S3.
2. **Transform**: Masks the PII as identified by the Bedrock model.
3. **Load**: Stores the masked data back in a different S3 bucket.

## Step 5: Triggering the ETL Pipeline

Once the data is masked, it's stored in another S3 bucket, ready for analysis. This bucket serves as the source for your ETL (Extract, Transform, Load) pipeline. When the process is complete, an SNS notification is sent via email.

### Final Steps

- **ETL Pipeline**: Processes the masked data for analysis.
- **SNS Notification**: Alerts you that the process is complete and data is ready.

## Conclusion

Congratulations! You've set up a robust system for securing and masking PII in your data stored in S3. This process ensures that your data is safe and compliant with regulations before it reaches your ETL pipeline. With AWS services like Macie, EventBridge, Bedrock, and Glue, you've got a powerful toolkit to handle sensitive data securely and efficiently. Now, go ahead and implement this solution to protect your data like a pro! 🎉

For more details on Macie, check out the [AWS Macie Documentation](https://docs.aws.amazon.com/macie/latest/userguide/what-is-macie.html).
