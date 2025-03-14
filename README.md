# Multi-Agent AWS Bedrock System

This project implements a multi-agent system using AWS Bedrock for intelligent recommendations of accommodations and restaurants. The system uses a supervisor agent to coordinate specialized agents that can provide information about hotels, Airbnbs, and restaurants based on user queries.

![Multi-Agent Workflow Diagram](Multi-Agentic-Workflow-Diagram.png)

## Architecture

The system consists of the following components:

1. **Supervisor Agent**: Coordinates requests between the user and specialized agents
2. **Accommodation Agent**: Provides information about hotels and Airbnbs
3. **Restaurant Agent**: Provides information about restaurants
4. **API Gateway**: Exposes the system via HTTP endpoints for frontend integration
5. **S3 Bucket**: Stores CSV data for accommodations and restaurants

## Lambda Functions

### 1. Supervisor Agent (invoke-supervisor-agent.py)

- Handles initial user requests
- Routes queries to appropriate specialized agents
- Consolidates responses for the user

### 2. Accommodation Lambda (bedrock-accommodation-lambda.py)

- Processes requests for hotel and Airbnb information
- Filters accommodations based on location and amenities
- Returns formatted JSON responses

### 3. Restaurant Lambda (bedrock-restaurant-lambda.py)

- Processes requests for restaurant information
- Filters restaurants based on city and dining preferences
- Returns formatted JSON responses

## Environment Variables

The system uses environment variables for configuration:

### Supervisor Agent

- `AGENT_ID`: The ID of the Bedrock agent
- `AGENT_ALIAS_ID`: The alias ID of the Bedrock agent

### Accommodation Lambda

- `S3_BUCKET`: The S3 bucket name containing the data files
- `HOTEL_CSV_KEY`: The S3 key for the hotel CSV file (defaults to 'hotel.csv')
- `AIRBNB_CSV_KEY`: The S3 key for the Airbnb CSV file (defaults to 'airbnb.csv')

### Restaurant Lambda

- `S3_BUCKET`: The S3 bucket name containing the data files
- `RESTAURANT_CSV_KEY`: The S3 key for the restaurant CSV file (defaults to 'restaurant.csv')

## API Gateway Integration

The system is accessible via HTTP requests from your frontend application. The API Gateway exposes endpoints that trigger the Lambda functions:

- `POST /agent`: Endpoint to interact with the supervisor agent

Example request:

```json
{
  "text": "I'm looking for a hotel in New York",
  "sessionId": "user-session-123"
}
```

Example response:

```json
{
  "response": "I found 3 hotels in New York. Here are the details: [...]"
}
```

## Setup Instructions

1. Create an S3 bucket and upload the CSV files
2. Deploy the Lambda functions with appropriate IAM roles
3. Configure environment variables for each Lambda function
4. Set up the API Gateway to trigger the supervisor Lambda function
5. Configure CORS if needed for frontend integration

## Deployment

This project is designed to be deployed on AWS using:

- AWS Lambda for serverless function execution
- Amazon Bedrock for AI capabilities
- Amazon S3 for data storage
- Amazon API Gateway for HTTP endpoints

## Prerequisites

- AWS Account with access to Bedrock
- Appropriate IAM permissions
- Python 3.8+ for local development

## Local Development

1. Clone the repository
2. Install dependencies: `pip install -r requirements.txt`
3. Set up environment variables
4. Use AWS SAM or serverless framework for local testing

## Security Considerations

- Ensure proper IAM permissions for Lambda functions
- Implement API key or other authentication for API Gateway
- Consider encrypting sensitive data in S3
