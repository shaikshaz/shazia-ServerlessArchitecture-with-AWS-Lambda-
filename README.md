# shazia-ServerlessArchitecture-with-AWS-Lambda
1.	Create s3-shazia-serverlesbucket
-Block all the public and private access except for the last cross account access.
- create bucket

2.	In bucket upload 2 files index.html-my website and error.html-in case if the page fails to fetch the data.
3.	Go back to the bucket and in the properties section go to the static web hosting and enable it and give our website name which are index.html and error.html
http://shazia-serverlesbucket.s3-website.eu-west-2.amazonaws.com-static website host endpoint-check if the website is up and running
4.	Create a lambda function:
-Create function ,take python 3.8 ,give function name: Helloworldfunction
-go to lambda function and give your own lambda function as below:
# define the handler function that the Lambda service will use as an entry point
def lambda_handler(event, context):
# extract values from the event object we got from the Lambda service and store in a variable
    name = event['firstName'] +' '+ event['lastName']
# write name and time to the DynamoDB table using the object we instantiated and save response in a variable
    response = table.put_item(
        Item={
            'ID': name,
            'LatestGreetingTime':now
            })
# return a properly formatted JSON object
    return {
        'statusCode': 200,
        'body': json.dumps('Hello from Lambda, ' + name)
    }
-create an test event in the test section – HelloWorldTestEvent- change json script - 
{
  "firstName": "shazia",
  "lastName": "shaik"
}
 then click on ‘Test’-it works-helloworld
5.create API gateway-to call this lambda function
-Create API-go to REST API-Select Build-New API- HelloWorldAPI -In API endpoint select    Edge-optimized- create API
6.Go to creted API -Select create method and select-POST-give our Lambda fun name-HelloWorld 
  -Enable CORS in the right side-save default
7.Next Deploy and test API:
-Go to Deploy in the right- New stage and Give dev -Deploy- 
https://7827umsqka.execute-api.eu-west-2.amazonaws.com/dev
-Hit the above DNS name in ‘Postman’-can See ‘Hello from Lambda’-that means API request is successfully able to send our request to the server,the serverless launch has run and gave the response.
8.Create a DynamoDB Table- HelloWorldDatabase- give primary key as ID-Create Table
arn:aws:dynamodb:eu-west-2:866934333672:table/HelloWorldDatabase   -  this ARN is imp to hit the table and give values
9.Create an IAM policy and give access to lambda function to data.
-to to lambda-in the permission section select our lambda it will navigate to IAM -in permission then to JSON-and paste our policy-
 {
"Version": "2012-10-17",
"Statement": [
    {
        "Sid": "VisualEditor0",
        "Effect": "Allow",
        "Action": [
            "dynamodb:PutItem",
            "dynamodb:DeleteItem",
            "dynamodb:GetItem",
            "dynamodb:Scan",
            "dynamodb:Query",
            "dynamodb:UpdateItem"
        ],
        "Resource": "YOUR-TABLE-ARN"
    }
    ]
}
-Give policy name- shaziaDynamopolicy
10.Go back to lambda function and modify the code:
# import the json utility package since we will be working with a JSON object
import json
# import the AWS SDK (for Python the package name is boto3)
import boto3
# import two packages to help us with dates and date formatting
from time import gmtime, strftime

# create a DynamoDB object using the AWS SDK
dynamodb = boto3.resource('dynamodb')
# use the DynamoDB object to select our table
table = dynamodb.Table('HelloWorldDatabase')
# store the current time in a human readable format in a variable
now = strftime("%a, %d %b %Y %H:%M:%S +0000", gmtime())

# define the handler function that the Lambda service will use as an entry point
def lambda_handler(event, context):
# extract values from the event object we got from the Lambda service and store in a variable
    name = event['firstName'] +' '+ event['lastName']
# write name and time to the DynamoDB table using the object we instantiated and save response in a variable
    response = table.put_item(
        Item={
            'ID': name,
            'LatestGreetingTime':now
            })
# return a properly formatted JSON object
    return {
        'statusCode': 200,
        'body': json.dumps('Hello from Lambda, ' + name)
    }
now give input and it will give the latest data update with time stamp and first ,last name
11.Update our HTML with our dynamic JAVA Script .-Java script is handling API call which is invoking our lambda function
blueprint
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
