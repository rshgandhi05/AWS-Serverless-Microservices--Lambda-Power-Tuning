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

# AWS Lambda Power Tuning
AWS Lambda Power Tuning is a crucial, open-source tool that uses AWS Step Functions to help you find the optimal memory setting for your Lambda function.

The Trade-off: In AWS Lambda, memory is directly proportional to CPU power. More memory = more CPU = faster execution, but also higher cost per millisecond.

How it Works: The tool automatically runs your Lambda function multiple times with various memory configurations (e.g., 128MB, 512MB, 1024MB) and generates a visualization.

Outcome: You can choose a strategy: optimize for the lowest cost, the fastest speed, or a balanced approach, and then set your Lambda's memory to the suggested optimal value. This ensures you're never over- or under-provisioning your resources.

# Lambda Function 
from __future__ import print_function
import boto3
import json
# ... (rest of your Python code)
def lambda_handler(event, context):
# ...
    # Your Lambda code goes here
# ...
