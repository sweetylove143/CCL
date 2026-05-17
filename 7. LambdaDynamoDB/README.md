# Practical 7: Lambda writes to DynamoDB

## Goal

Create a Lambda function that accepts input and stores it in a DynamoDB table.

## Prerequisites

- AWS account
- Basic Python knowledge

## Step 1: Create DynamoDB table

1. AWS Console -> DynamoDB -> Create table.
2. Table name: Users
3. Partition key: user_id (String)
4. Create table and wait until Active.

## Step 2: Create IAM role for Lambda

1. AWS Console -> IAM -> Roles -> Create role.
2. Trusted entity: AWS service.
3. Use case: Lambda.
4. Attach policy: AmazonDynamoDBFullAccess.
5. Name: lambda-dynamodb-role.

## Step 3: Create Lambda function

1. AWS Console -> Lambda -> Create function.
2. Function name: insertUser.
3. Runtime: Python 3.x.
4. Execution role: use lambda-dynamodb-role.

## Step 4: Add Lambda code

Paste this code in the Lambda editor:

```
import json
import boto3

dynamodb = boto3.resource("dynamodb")
table = dynamodb.Table("Users")

def lambda_handler(event, context):
    try:
        user_id = event["user_id"]
        name = event["name"]

        table.put_item(Item={"user_id": user_id, "name": name})

        return {
            "statusCode": 200,
            "body": json.dumps("Data inserted successfully"),
        }
    except Exception as e:
        return {"statusCode": 500, "body": str(e)}
```

## Step 5: Test the function

Create a test event:

```
{
  "user_id": "101",
  "name": "John Smith"
}
```

Run the test and verify the item appears in the Users table.
