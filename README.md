# shazia-ServerlessArchitecture-with-AWS-Lambda
Step 1. Create a DynamoDB Table
Navigate to the DynamoDB console.
Click on the "Create table" button.
Provide a table name (shazia-Serverless-DDBTable).
Set the primary key. For simplicity i have used a single attribute named id of type String.
Configure additional settings as needed (e.g., provisioned throughput, encryption).
Click "Create" to create the table.

Step 2. Create IAM Role for Lambda to assume access to DynamoDB
Go to the IAM console.
Click on "Roles" in the left navigation pane.
Click "Create role. “Choose "AWS Lambda" as the service that will use this role.
Attach the policy Amazon Dynamo DBFull Access (or create a custom policy with specific permissions).
Review and name the role 
Create the role.(shazia-DDB1)

Step 3. Create an AWS Lambda Function
Choose from scratch // blueprint
Choose the role you created in step 2

Step 4. Add DynamoDB table as an environment variable (under configuration tab)
Add a variable:
Key: DYNAMODB_TABLE
Value: shazia-serverless-DDBTable 

Step 5: Test the Setup
Save the Lambda function.
Click on the "Test" button in the Lambda console to test the function.
Review the CloudWatch logs for any errors or successful executions.

Step 6: Set Up API Gateway 
If you want to expose your Lambda function via an API, set up an API Gateway and integrate it with your Lambda function by setting the trigger. That way you can use the provided endpoint to invoke your serverless Lambda function.
Go to the API Gateway console.
Click "Create API."

![image](https://github.com/shaikshaz/shazia-ServerlessArchitecture-with-AWS-Lambda-/assets/154241222/dcd98707-cbe2-4438-b99a-e82b13fea8a2)
