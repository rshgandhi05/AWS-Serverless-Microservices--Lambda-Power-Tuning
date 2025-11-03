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
The AWS Lambda Power Tuning tool uses AWS Step Functions to automatically test your Lambda function across various memory configurations. This process is essential for data-driven optimization:

The Goal: Find the optimal memory setting where the execution speed gain outweighs the increased cost per millisecond.

The Method: The tool invokes the function with different memory settings, analyzes the execution logs, and suggests the configuration to minimize cost or maximize performance

# Lambda Function 
The function acts as a command handler, routing the input operation from the API Gateway payload to the appropriate boto3 (DynamoDB SDK) method.

```python
from __future__ import print_function
import boto3
import json

print('Loading function')


def lambda_handler(event, context):
    Provide an event that contains the following keys:

      - operation: one of the operations in the operations dict below
      - tableName: required for operations that interact with DynamoDB
      - payload: a parameter to pass to the operation being performed
    '''
    #print("Received event: " + json.dumps(event, indent=2))

    operation = event['operation']

    if 'tableName' in event:
        dynamo = boto3.resource('dynamodb').Table(event['tableName'])

    operations = {
        'create': lambda x: dynamo.put_item(**x),
        'read': lambda x: dynamo.get_item(**x),
        'update': lambda x: dynamo.update_item(**x),
        'delete': lambda x: dynamo.delete_item(**x),
        'list': lambda x: dynamo.scan(**x),
        'echo': lambda x: x,
        'ping': lambda x: 'pong'
    }

    if operation in operations:
        return operations[operation](event.get('payload'))
    else:
        raise ValueError('Unrecognized operation "{}"'.format(operation))
```
# Deployment Summary

<img width="2180" height="435" alt="image" src="https://github.com/user-attachments/assets/456a843c-f74f-4a7e-b2ec-abd9e2b3f674" />

# Validation and Performance Analysis
1. Postman Testing

Postman was used to simulate user traffic (10 Virtual Users (VU) with a 2-minute duration) and perform a load test on the API Gateway endpoint. Postman enables simulating user traffic and observing how the API behaves under load, helping identify issues or bottlenecks.

2. Comparative Performance Benchmarking
To validate the Power Tuning principle, two load tests were run on the same API endpoint, only changing the AWS Lambda memory setting for LambdaFunctionOverHttps.
<img width="2797" height="375" alt="image" src="https://github.com/user-attachments/assets/3efcaf6e-bc18-447a-9483-23f4ba22eb05" />

# Result Visualization (128 MB vs 1024 MB)

# 128 MB
<img width="1930" height="1657" alt="image" src="https://github.com/user-attachments/assets/37eabf70-bfa6-46a8-9989-66e3d461d49a" />

# 1024 MB
<img width="1925" height="1650" alt="image" src="https://github.com/user-attachments/assets/6b02b637-bd1d-46fc-8f99-5a08248d0301" />

Power Tuning Outcome: The increase in memory from 128MB to 1024MB resulted in a significant performance increase (75% reduction in average latency) and higher efficiency (more requests processed), validating that 1024MB is the superior setting for this workload from a Performance Efficiency standpoint.

# Cost Analysis
<img width="1675" height="280" alt="image" src="https://github.com/user-attachments/assets/be2cf5c1-1823-4455-b866-8c68ec365fe6" />


Cost Optimization Trade-off: While the cost factor for 1024 MB is mathematically higher (88 vs 43.375), the massive 75% reduction in latency (347ms $\rightarrow$ 88ms) creates a better user experience and allows the service to handle higher throughput (6.06 req/s vs 4.97 req/s).

Final Decision: For a user-facing API, the Performance Efficiency gained by using 1024MB often outweighs the marginal increase in Cost Optimization. The best balanced solution would be found by testing values between 128MB and 1024MB using the Power Tuning visualization.





