Setting up API Gateway, DynamoDB, and Lambda to support the provided Python function for a CRUD REST API.



In this tutorial, I will walk you through the process of building a serverless API that enables the create, read, update, and delete of items stored in a DynamoDB table. By following the steps outlined below, you will be able to develop a scalable and efficient API using AWS Lambda, API Gateway, and DynamoDB.
You can do it within the AWS Free Tier.
Here are the high-level steps we’ll follow:
Step 1: Create a DynamoDB table
Step 2: Create a Lambda function
Step 3: Create an API gateway
Step 4: Test your API
Step 5: Monitor your Logs and Insights
Here is a link to my github repo: https://github.com/Uwadon1/AWS_RestLambdaAPI

Step-by-Step Tutorial: Build a REST API (CRUD)
Step ONE: Create a DynamoDB table

Set Up the Environment


Sign in to the AWS Management Console, we will be brought to the aws homepage.


Create a DynamoDB Table


From the AWS dashboard, search for DynamoDb, then click on the DynamoDB service to navigate into the dynamo db dashboard.

Click on the “create table icon

Fill in the necessary details and give your table a name (e.g `employee_info` or any name of your choice). You can specify the partition key as `employeeid` (string). Let the table setting remain as default.

Leave the remaining options as default and click “create table.”

By following these steps, you will successfully create a DynamoDB table named “employee_info” with the partition key set to “employeeid”.
Step TWO: Create a Lambda function
Create Lambda Functions


For this project, we will be using AWS Lambda for our backend logic:
From the AWS Management Console, search for AWS Lambda service.


Click on “create a function”:



In the “create a function” dashboard, select “author from scratch.” In the basic information section, give your lambda a function name e.g `api_processing`, we will select Python 3.13 as our runtime



For Permission, in the execution role section, we will opt for the third/last option: “create a new role from AWS policy templates.” 
This new role should give us the opportunity to attach policy that grants full acces to DynamoDb and Cloudwatch or a custom policy with limited access to dynamo db. Afterwards, we will click on ‘create function’



We will be brougth to the lambda dashboard, showing function successfully created.



Right now, we’ll need to tweak the permission for our IAM role. Navigate to the configuration section, and select the permission button. Next you’ll need to click on the link in the role name section.

. 

You will be redicted to the IAM service tab, you will notice by default we already have a policy that has AWSLambdaBasicExecutionRole, we will need to attach more policies like Dynamodb full access and Cloudwatchlog full access

Next we will need to attach our desired policies to this role, first we’ll add “AwsDynamoDBFullAccess”

We will add the next policy to this role, which is:  “CloudWatchLogsFullAccess”

Policies have been added, hence our lambda now has the required access to interact with DynamoDB and to get logs from out Cloudwatch.

By following these steps, you will have created a Lambda function named “api_processing” and set up a new role with the appropriate permissions to interact with DynamoDB.
Step THREE: Create an  API gateway

In the AWS dashboard, search for API Gateway and click on it.


You will be brought straight to Create API dashboard, choose REST API and click on build


Choose “new API” and give it a name (e.g., serverless-demo`).
Leave the description section blank and select regional as the API endpoint type. Then click on create api


Next, we are going to create resources: Click on “Create Resource”



   
We’ll need to give a resource a name, in the resource name section, write `status` and in the resource path select `/`. Ensure to check the button to enable CORS for cross-origin requests. And click create resource.



As seen below, the resource has been successfully created, next we’ll create methods for the status resource:



When you click on the “Create Method” icon, you will be brought to the tab below, select the method “get” and click the checkmark. Select lambda function as the Integration type. Ensure to click on the button to turn on lambda proxy integration. 



Specify your Lambda function name (`api_processing`), leave the other options as default. Click ‘create method”. 

The get method for resource status has been successfully created. 



We’ll need to create a new resource and give it the name `employee` and in the resource path select `/`. Ensure to check the button to enable CORS for cross-origin requests. And click create resource.


The newly created resource should look like this.



We’ll need to create a new resource and give it the name `employees` and in the resource path select `/`. Ensure to check the button to enable CORS for cross-origin requests. And click create resource.


The newly created resource should look like this, next we’ll create ‘get’ method  for the employees resource:



Click on the “Create Method” icon, you will be brought to the tab below, select the method “get” and click the checkmark. Select lambda function as the Integration type. Ensure to click on the button to turn on lambda proxy integration. 



Specify your Lambda function name (`api_processing`), leave the other options as default. Click ‘create method’, then confirm.


The newly created get method for the /employees resource should look like this.



Lastly we will need to create the CRUD methods for the employee resource. For each HTTP method (GET, POST, DELETE, PATCH):
“Click on create Method” icon, you will be brought to the tab below, select the method “get” and click the checkmark. Select lambda function as the Integration type. Ensure to click on the button to turn on lambda proxy integration. 




Specify your Lambda function name (`api_processing`), leave the other options as default. Click ‘create method’, then confirm.


The newly created get method should look like this.



In the “Create Method” icon, you will be brought to the tab below, select the method “post” and click the checkmark. Select lambda function as the Integration type. Ensure to click on the button to turn on lambda proxy integration. Specify your Lambda function name (`api_processing`), leave the other options as default. Click ‘create method’, then confirm.



The newly created post method should look like this.


In the “Create Method” icon, you will be brought to the tab below, select the method “delete” and click the checkmark. Select lambda function as the Integration type. Ensure to click on the button to turn on lambda proxy integration. Specify your Lambda function name (`api_processing`), leave the other options as default. Click ‘create method’, then confirm.


The newly created delete method should look like this.




In the “Create Method” icon, you will be brought to the tab below, select the method “patch” and click the checkmark. Select lambda function as the Integration type. Ensure to click on the button to turn on lambda proxy integration. Specify your Lambda function name (`api_processing`), leave the other options as default. Click ‘create method’, then confirm.


The newly created patch method should look like this.



Resources: These are endpoints in your API (e.g., /employee or /status). They represent logical containers for HTTP methods and map directly to backend operations.
Methods: These are the HTTP operations (like GET, POST, PUT, DELETE) applied to a resource. They determine what action is performed on the resource. For example:
GET: Fetch data.
POST: Create data.
PUT: Update existing data.
DELETE: Remove data.
API Gateway integrates these resources and methods with AWS Lambda to execute backend logic or interact with a database like DynamoDB

Click on deploy API
In the deploy API overview, a box would pop up for you to choose a stage (New Stage), and then type in a stage name (dev), you can fill in the description box or leave it blank, it is optional. Then click on deploy.


By following these steps, you will have successfully created a REST API named “serverless-demo” that serves as an HTTP endpoint for your Lambda function. In the subsequent steps. Take note of the invoke URL, we will need it later.


Step FOUR: Test your API

This right here is the code editor overview, and we have the default lambda code, so we will try to run the invoke url code to test its functionality. You can use tools like Postman or a browser to make API requests. We should get the response “Hello from Lambda!” on our screen. 


Here is the invoke url code on our browser, we will include the pathway to our status resource (e.g /status) at the end of the URL code. The essence of the status resource is to test the functionality..


This is what we got on browser, “Hello from Lambda!” which shows that everything in our application and code is working fine.

Preferably we will also try the invoke url code on postman, we will also include the pathway to our status resource (e.g /status) at the end of the URL code. We will also select the “get” option and click on send..


This is the output on lambda: “Hello from Lambda!”


Next we will need to open the lambda dashboard and navigate to the code editor’s tab and replace the existing code with the following code snippet:
Here, we will write a logic for each CRUD operation:


POST: Insert a new record in DynamoDB.
GET: Fetch all or a specific record from DynamoDB.
UPDATE: Update a record.
DELETE: Remove a record.

You can write or upload your code on the console editor:
 
Here is the link to the Code on my github
import json
import boto3
from botocore.exceptions import ClientError
from decimal import Decimal
from boto3.dynamodb.conditions import Key


# Initialize the DynamoDB client
dynamodb = boto3.resource('dynamodb', region_name='us-east-1')
dynamodb_table = dynamodb.Table('employee_info')


status_check_path = '/status'
employee_path = '/employee'
employees_path = '/employees'


def lambda_handler(event, context):
    print('Request event: ', event)
    response = None
   
    try:
        http_method = event.get('httpMethod')
        path = event.get('path')


        if http_method == 'GET' and path == status_check_path:
            response = build_response(200, 'Service is operational')
        elif http_method == 'GET' and path == employee_path:
            employee_id = event['queryStringParameters']['employeeid']
            response = get_employee(employee_id)
        elif http_method == 'GET' and path == employees_path:
            response = get_employees()
        elif http_method == 'POST' and path == employee_path:
            response = save_employee(json.loads(event['body']))
        elif http_method == 'PATCH' and path == employee_path:
            body = json.loads(event['body'])
            response = modify_employee(body['employeeId'], body['updateKey'], body['updateValue'])
        elif http_method == 'DELETE' and path == employee_path:
            body = json.loads(event['body'])
            response = delete_employee(body['employeeId'])
        else:
            response = build_response(404, '404 Not Found')


    except Exception as e:
        print('Error:', e)
        response = build_response(400, 'Error processing request')
   
    return response


def get_employee(employee_id):
    try:
        response = dynamodb_table.get_item(Key={'employeeid': employee_id})
        return build_response(200, response.get('Item'))
    except ClientError as e:
        print('Error:', e)
        return build_response(400, e.response['Error']['Message'])


def get_employees():
    try:
        scan_params = {
            'TableName': dynamodb_table.name
        }
        return build_response(200, scan_dynamo_records(scan_params, []))
    except ClientError as e:
        print('Error:', e)
        return build_response(400, e.response['Error']['Message'])


def scan_dynamo_records(scan_params, item_array):
    response = dynamodb_table.scan(**scan_params)
    item_array.extend(response.get('Items', []))
   
    if 'LastEvaluatedKey' in response:
        scan_params['ExclusiveStartKey'] = response['LastEvaluatedKey']
        return scan_dynamo_records(scan_params, item_array)
    else:
        return {'employees': item_array}


def save_employee(request_body):
    try:
        dynamodb_table.put_item(Item=request_body)
        body = {
            'Operation': 'SAVE',
            'Message': 'SUCCESS',
            'Item': request_body
        }
        return build_response(200, body)
    except ClientError as e:
        print('Error:', e)
        return build_response(400, e.response['Error']['Message'])


def modify_employee(employee_id, update_key, update_value):
    try:
        response = dynamodb_table.update_item(
            Key={'employeeid': employee_id},
            UpdateExpression=f'SET {update_key} = :value',
            ExpressionAttributeValues={':value': update_value},
            ReturnValues='UPDATED_NEW'
        )
        body = {
            'Operation': 'UPDATE',
            'Message': 'SUCCESS',
            'UpdatedAttributes': response
        }
        return build_response(200, body)
    except ClientError as e:
        print('Error:', e)
        return build_response(400, e.response['Error']['Message'])


def delete_employee(employee_id):
    try:
        response = dynamodb_table.delete_item(
            Key={'employeeid': employee_id},
            ReturnValues='ALL_OLD'
        )
        body = {
            'Operation': 'DELETE',
            'Message': 'SUCCESS',
            'Item': response
        }
        return build_response(200, body)
    except ClientError as e:
        print('Error:', e)
        return build_response(400, e.response['Error']['Message'])


class DecimalEncoder(json.JSONEncoder):
    def default(self, obj):
        if isinstance(obj, Decimal):
            # Check if it's an int or a float
            if obj % 1 == 0:
                return int(obj)
            else:
                return float(obj)
        # Let the base class default method raise the TypeError
        return super(DecimalEncoder, self).default(obj)


def build_response(status_code, body):
    return {
        'statusCode': status_code,
        'headers': {
            'Content-Type': 'application/json'
        },
        'body': json.dumps(body, cls=DecimalEncoder)
    }

Detailed Explanation and Walkthrough of the Lambda Function Code
This AWS Lambda function is a serverless backend that manages employee data using DynamoDB. It serves as a RESTful API that performs CRUD (Create, Read, Update, Delete) operations for an employee_info table in DynamoDB.
Key Features
HTTP Method Handling: Supports GET, POST, PATCH, and DELETE methods.
DynamoDB Integration: Queries, scans, updates, adds, and deletes records in a DynamoDB table.
Custom Error Handling: Ensures robust error messages for failed operations.
Custom JSON Handling: Encodes Decimal values (from DynamoDB) as standard JSON.

Code Breakdown
Imports and Initialization
import json
import boto3
from botocore.exceptions import ClientError
from decimal import Decimal
from boto3.dynamodb.conditions import Key

boto3: Used to interact with AWS services, specifically DynamoDB.
ClientError: Captures any AWS API request-related errors.
Decimal: Handles DynamoDB's Number type for precise floating-point arithmetic.
Key: Enables conditions for DynamoDB queries.
Global Variables
dynamodb = boto3.resource('dynamodb', region_name='us-east-1')
dynamodb_table = dynamodb.Table('employee_info')

status_check_path = '/status'
employee_path = '/employee'
employees_path = '/employees'

dynamodb_table: References the employee_info DynamoDB table.
URL paths (/status, /employee, /employees) define the API routes.
lambda_handler: Entry Point for the Lambda Function
def lambda_handler(event, context):

Input:
event: Contains the HTTP request details.
context: Metadata about the Lambda invocation.
The handler processes the request and determines which operation to perform based on:
HTTP Method: GET, POST, PATCH, or DELETE.
Path: Specific route (/employee, /employees, etc.).

HTTP Method-Based Operations
GET /status: Simple status check.
GET /employee: Retrieves a single employee record by employeeid.
GET /employees: Retrieves all employee records.
POST /employee: Adds a new employee record.
PATCH /employee: Updates a specific employee attribute.
DELETE /employee: Deletes a specific employee record.

Helper Functions
get_employee
def get_employee(employee_id):
    try:
        response = dynamodb_table.get_item(Key={'employeeid': employee_id})
        return build_response(200, response.get('Item'))
    except ClientError as e:
        print('Error:', e)
        return build_response(400, e.response['Error']['Message'])

Purpose: Fetch a record by employeeid.
Workflow:
Queries the DynamoDB table using get_item.
Returns the item (or an error message).
get_employees
def get_employees():
    try:
        scan_params = {'TableName': dynamodb_table.name}
        return build_response(200, scan_dynamo_records(scan_params, []))
    except ClientError as e:
        print('Error:', e)
        return build_response(400, e.response['Error']['Message'])

Purpose: Fetch all employee records.
Uses the helper function scan_dynamo_records for paginated DynamoDB scans.
scan_dynamo_records
def scan_dynamo_records(scan_params, item_array):
    response = dynamodb_table.scan(**scan_params)
    item_array.extend(response.get('Items', []))
    if 'LastEvaluatedKey' in response:
        scan_params['ExclusiveStartKey'] = response['LastEvaluatedKey']
        return scan_dynamo_records(scan_params, item_array)
    else:
        return {'employees': item_array}

Purpose: Handles large datasets by recursively scanning paginated data.
save_employee
def save_employee(request_body):
    try:
        dynamodb_table.put_item(Item=request_body)
        body = {'Operation': 'SAVE', 'Message': 'SUCCESS', 'Item': request_body}
        return build_response(200, body)
    except ClientError as e:
        print('Error:', e)
        return build_response(400, e.response['Error']['Message'])

Purpose: Adds a new record.
Uses put_item to insert data into DynamoDB.
modify_employee
def modify_employee(employee_id, update_key, update_value):
    try:
        response = dynamodb_table.update_item(
            Key={'employeeid': employee_id},
            UpdateExpression=f'SET {update_key} = :value',
            ExpressionAttributeValues={':value': update_value},
            ReturnValues='UPDATED_NEW'
        )
        body = {'Operation': 'UPDATE', 'Message': 'SUCCESS', 'UpdatedAttributes': response}
        return build_response(200, body)
    except ClientError as e:
        print('Error:', e)
        return build_response(400, e.response['Error']['Message'])

Purpose: Updates a specific field for an employee.
Dynamically constructs an update expression (SET ...).
delete_employee
def delete_employee(employee_id):
    try:
        response = dynamodb_table.delete_item(Key={'employeeid': employee_id}, ReturnValues='ALL_OLD')
        body = {'Operation': 'DELETE', 'Message': 'SUCCESS', 'Item': response}
        return build_response(200, body)
    except ClientError as e:
        print('Error:', e)
        return build_response(400, e.response['Error']['Message'])

Purpose: Deletes a record by employeeid.

Utility Functions
DecimalEncoder
class DecimalEncoder(json.JSONEncoder):
    def default(self, obj):
        if isinstance(obj, Decimal):
            if obj % 1 == 0:
                return int(obj)
            else:
                return float(obj)
        return super(DecimalEncoder, self).default(obj)

Converts Decimal types (common in DynamoDB) to JSON-compatible int or float.
build_response
def build_response(status_code, body):
    return {
        'statusCode': status_code,
        'headers': {'Content-Type': 'application/json'},
        'body': json.dumps(body, cls=DecimalEncoder)
    }

Formats the HTTP response with a status code, headers, and JSON body.

This function serves as a robust backbone for employee management, leveraging the power of AWS Lambda and DynamoDB in a RESTful architecture.

Next we hit the deploy button to update our new python code.


We will move, right away to test our Lambda function. We will need to choose our test event e.g: “test-crud” and save


We will retest the /status ‘get’  API endpoint with Postman. Using the invoke url code on our browser, we will include the pathway to our status resource (e.g /status) at the end of the URL code. We will get the output below.


Using the browser, we got this output. “Service is operational” which shows that everything in our application and code is working fine.


We will test the /employee ‘post’ API endpoint with Postman. Using the invoke url code on our browser, we will include the pathway to our employee resource (e.g employee) at the end of the URL code. We will choose the following options as shown below, choose ‘post’ option, and select body, then raw, and json format to insert in our command as json.
{
“Employeeid”: “102”,
“job_title”: “CEO”,
“full_name”: “John Tom”,
“salary”: “1000000”
}. 
Note we changed the last endpoint from /status to /employee. We should see ‘200 okay’. Which shows everything is functional. We should also see our output in the down box.
We will follow the same steps to input two other values.



We will test the /employee ‘delete’ API endpoint with Postman. Using the invoke url code on our browser, we will include the pathway to our employee resource (e.g /employee) at the end of the URL code. We will choose the following options as shown below, choose ‘delete’ option, and type in our command as json: 
{
“Employee”: “102”
}. 
We should see the output below “Operation”: “DELETE”, and ‘200 okay’. Which shows everything is functional.


We will test the /employee ‘patch’ API endpoint with Postman. Using the invoke url code on our browser, we will include the pathway to our employee resource (e.g /employee) at the end of the URL code. We will choose the following options as shown below, choose patch’ option, and type in our command as json: 
{
“EmployeeId”: “103”,
“updateKey”: “full_name”,
“updateValue”: “Chioma Chukwu”
}. 
We should see the output below “Operation”: “UPDATE”, and ‘200 okay’. Which shows everything is functional.


Lastly, we will test the /employees ‘get’  API endpoint with Postman. Using the invoke url code on our browser, we will include the pathway to our employees resource (e.g /employees) at the end of the URL code. We will get the info output of all the employees in the company .


We will go into our Dynamo DB dashboard to verify if all we have created using our API and lambda would reflect inside our database


Step FIVE: Monitoring the logs and insights
Using cloudwatch we can monitor the logs for insights to check for errors and troubleshoot where necessary


