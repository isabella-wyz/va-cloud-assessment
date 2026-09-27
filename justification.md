## Vehicle Analytics Cloud Assessment – Justification

Use this file to briefly explain your design decisions. Bullet points are fine.

### 1. High-level architecture

- Summary of your overall cloud design:
- The proposed architecture adds a dedicated weather data ingestion path to the existing Vehicle Analytics platform. Weather data is sent from the weather station to Amazon API Gateway using HTTPS. API Gateway triggers a Lambda function to validate and process the data. Valid weather records are stored in S3 and can also be integrated with the existing streaming service for real-time analysis.

### 2. Weather station integration

- How the weather station connects to the cloud:
- The weather station is located near the track and sends weather measurements to Amazon API Gateway through a secure HTTPS connection using a POST /weather endpoint.
  
- How weather data flows into the Vehicle Analytics platform:
- API Gateway receives the weather data and forwards it to a Lambda function for validation and processing. Valid records are converted into a standard format and stored in S3. The processed data can also be sent to the existing streaming service where it can be correlated with vehicle telemetry using timestamps.

### 3. Infrastructure as Code (IaC)

- Which IaC tool(s) you would use (e.g. Terraform, AWS CDK) and why:
- Terraform or AWS CDK can be used to provision the required AWS resources. These tools provide repeatable deployments, simplify infrastructure management, and support version control.
  
- How IaC fits into deployment / environments:
- Infrastructure definitions should be stored in a Git repository and deployed through separate environments such as development, testing, and production to ensure consistency and reduce deployment risks.

### 4. Security, reliability, observability

- Key security choices (network boundaries, authn/z, secrets, etc.):
- HTTPS is used to secure communication between the weather station and AWS. Authentication is applied to the API endpoint. Lambda uses least-privilege IAM permissions, and the S3 bucket blocks public access and enables encryption at rest.

- Reliability and failure modes:
- Invalid or malformed weather data is rejected before storage. S3 provides durable storage for historical records. If additional buffering is required in the future, Amazon SQS can be introduced between ingestion and processing.
  
- Monitoring/alerting and operational concerns:
- AWS CloudWatch can be used for logging, monitoring, and alerting to detect API Gateway and Lambda failures. Operational metrics can be monitored to ensure reliable data ingestion and processing.

### 5. Cost and scalability considerations

- Expected cost drivers and how you would keep costs under control:
- The primary cost drivers are API requests, Lambda executions, and S3 storage. Using serverless services helps minimise costs because resources are only consumed when weather data is received.
  
- How the design scales to more stations/events:
- API Gateway, Lambda, and S3 can scale automatically to support additional weather stations and track-day events with minimal infrastructure changes. The architecture remains simple while supporting future growth.
