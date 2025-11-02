# AWS-Serverless-Microservices--Lambda-Power-Tuning
This project establishes a serverless REST API using Amazon API Gateway that interfaces with an AWS Lambda function to perform CRUD operations (Create, Read, Update, Delete) on an Amazon DynamoDB table. This pattern exemplifies a Serverless Microservice architecture, focusing on high scalability, low cost, and minimal operational overhead.

# Conceptual Foundations
1. Serverless Microservices Explained
is architecture is a prime example of a serverless microservice.

Microservice: An architectural style where a large application is broken down into smaller, independent services. Your entire application (DynamoDBOperations) is composed of a single, specialized service (DynamoDBManager) whose sole job is to manage the DynamoDB table.

Serverless: The cloud provider (AWS) fully manages the servers, scaling, and capacity planning. You simply upload your code (Lambda) and define the API endpoint (API Gateway), and you pay only when the code executes.

# AWS Well-Architected Pillars & Power Tuning Impact
The AWS Well-Architected Framework provides guiding principles for building workloads in the cloud. Two pillars are directly impacted by Power Tuning:
<img width="2445" height="290" alt="image" src="https://github.com/user-attachments/assets/ec97af01-ede4-4078-9e05-b0c74356cf07" />

