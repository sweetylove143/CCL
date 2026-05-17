# Practical 8: API Gateway GET/POST with Lambda

## Goal

Create GET and POST endpoints using API Gateway and connect them to a Lambda function that reads/writes data in DynamoDB.

## Prerequisites

- AWS account
- Basic Python knowledge

## Step 1: Create DynamoDB table

1. AWS Console -> DynamoDB -> Create table.
2. Table name: Users
3. Partition key: user_id (String)
4. Create table and wait for Status = Active.

## Step 2: Create IAM role for Lambda

1. AWS Console -> IAM -> Roles -> Create role.
2. Trusted entity: AWS service.
3. Use case: Lambda.
4. Permissions: attach AmazonDynamoDBFullAccess.
5. Role name: lambda-dynamodb-role.

## Step 3: Create Lambda function

1. AWS Console -> Lambda -> Create function.
2. Function name: userApiHandler.
3. Runtime: Python 3.x.
4. Execution role: Use existing role -> lambda-dynamodb-role.

## Step 4: Add Lambda code

Paste this code in the Lambda editor and Deploy:

```
import json
import boto3

dynamodb = boto3.resource("dynamodb")
table = dynamodb.Table("Users")

def lambda_handler(event, context):
    method = event["requestContext"]["http"]["method"]
    try:
        if method == "POST":
            body = json.loads(event["body"])
            user_id = body["user_id"]
            name = body["name"]
            table.put_item(Item={"user_id": user_id, "name": name})
            return {"statusCode": 200, "body": json.dumps("User added successfully")}

        if method == "GET":
            user_id = event["queryStringParameters"]["user_id"]
            response = table.get_item(Key={"user_id": user_id})
            return {"statusCode": 200, "body": json.dumps(response.get("Item", {}))}

        return {"statusCode": 405, "body": "Method not allowed"}
    except Exception as e:
        return {"statusCode": 500, "body": str(e)}
```

## Step 5: Create HTTP API (API Gateway)

1. AWS Console -> API Gateway -> Create API.
2. Choose HTTP API and click Build.
3. Add integration -> Lambda -> select userApiHandler.
4. Add routes:
   - POST /user
   - GET /user
5. Create a stage (default is fine) and deploy.
6. Copy the Invoke URL.

## Step 6: Enable CORS (if calling from browser)

1. API Gateway -> Your API -> CORS.
2. Allow origins: \* (for demo) or your domain.
3. Allow methods: GET, POST.
4. Save and deploy again.

## Step 7: Test with curl or Postman

POST (insert data):

```
curl -X POST "https://YOUR_API_ID.execute-api.YOUR_REGION.amazonaws.com/user" \
  -H "Content-Type: application/json" \
  -d "{\"user_id\":\"101\",\"name\":\"John\"}"
```

GET (fetch data):

```
curl "https://YOUR_API_ID.execute-api.YOUR_REGION.amazonaws.com/user?user_id=101"
```

## Step 8: Verify in DynamoDB

Open DynamoDB -> Users table -> Explore table items. The inserted item should appear.

## Notes

- Use least-privilege IAM policies for production.
- Keep table name and Lambda code consistent.
