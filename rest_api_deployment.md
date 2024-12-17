
![Architectural Diagram](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/z164b65yci9pbpc9kwhp.png)


In this tutorial, I will walk you through the process of building a serverless API that enables the creates, reads, updates, and deletes of items stored in a DynamoDB table. By following the steps outlined below, you will be able to develop a scalable and efficient API using AWS Lambda, API Gateway, and DynamoDB.
You can do it within the AWS Free Tier.

Here are the high-level steps we’ll follow:

> Step 1: Create a DynamoDB table
> Step 2: Create a Lambda function
> Step 3: Create an API gateway
> Step 4: Test your API
> Step 5: Monitor your Logs and Insights

Here is a link to my github repo: https://github.com/Uwadon1/AWS_RestLambdaAPI

<u>Step-by-Step Tutorial: Build a REST API (CRUD)</u>

Step ONE: Create a DynamoDB table

Set Up the Environment


Sign in to the AWS Management Console, we will be brought to the aws homepage.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/x2yzc3hv5vk47ys71ydt.png)


<u>Create a DynamoDB Table</u>


From the AWS dashboard, search for DynamoDb, then click on the DynamoDB service to navigate into the dynamo db dashboard.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/jvcrmjfmqip6azy2jb9j.png)




Click on the “create table icon

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/dt1na5gbr2b985n5d3yq.png)


Fill in the necessary details and give your table a name (e.g `employee_info` or any name of your choice). You can specify the partition key as `employeeid` (string). Let the table setting remain as default.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/jjrjdbxcmq7mr165u6qo.png)


Leave the remaining options as default and click “create table.”

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/odx7byezazme5zmygdoy.png)



By following these steps, you will successfully create a DynamoDB table named “employee_info” with the partition key set to “employeeid”.

Step TWO: Create a Lambda function
Create Lambda Functions


For this project, we will be using AWS Lambda for our backend logic:
From the AWS Management Console, search for AWS Lambda service.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zvocvxae3pi47ohbxbsr.png)


Click on “create a function”:

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xh4aev0gi5v7o54zpxsl.png)
'



In the “create a function” dashboard, select “author from scratch.” In the basic information section, give your lambda a function name e.g `api_processing`, we will select Python 3.13 as our runtime

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/mu2ubvyyuntzvm06u5df.png)




For Permission, in the execution role section, we will opt for the third/last option: “create a new role from AWS policy templates.” 
This new role should give us the opportunity to attach policy that grants full acces to DynamoDb and Cloudwatch or a custom policy with limited access to dynamo db. Afterwards, we will click on ‘create function’

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/t1kmnnl49gfklc8o3apl.png)


We will be brougth to the lambda dashboard, showing function successfully created.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/mnv05d1f92eqhtqwbx5i.png)




Right now, we’ll need to tweak the permission for our IAM role. Navigate to the configuration section, and select the permission button. Next you’ll need to click on the link in the role name section.


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/m6pginbfqd4o4y2ptyn0.png)


You will be redirected to the IAM service tab, you will notice by default we already have a policy that has AWSLambdaBasicExecutionRole, we will need to attach more policies like Dynamodb full access and Cloudwatchlog full access

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/joafllliknc5udl9gizf.png)


Next we will need to attach our desired policies to this role, first we’ll add “AwsDynamoDBFullAccess”

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/3llps098630po2kkn5gi.png)


We will add the next policy to this role, which is:  “CloudWatchLogsFullAccess”

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/vlnjdl64l18pspvtxqcj.png)


Policies have been added, hence our lambda now has the required access to interact with DynamoDB and to get logs from out Cloudwatch.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/eg0gbiirqw2efoupk4zu.png)


By following these steps, you will have created a Lambda function named “api_processing” and set up a new role with the appropriate permissions to interact with DynamoDB.
Step THREE: Create an  API gateway

In the AWS dashboard, search for API Gateway and click on it.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2izp6u8l1bme41sl768q.png)


You will be brought straight to Create API dashboard, choose REST API and click on build

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/y3lyoalt580a3t5j0xgj.png)


Choose “new API” and give it a name (e.g., serverless-demo`).
Leave the description section blank and select regional as the API endpoint type. Then click on create api

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/qwwrhf70e25uwp1htj52.png)

Next, we are going to create resources: Click on “Create Resource”

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/azjahu3oiolokgll944u.png)


   
We’ll need to give a resource a name, in the resource name section, write `status` and in the resource path select `/`. Ensure to check the button to enable CORS for cross-origin requests. And click create resource.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/126hi5jluzirs45vszup.png)


As seen below, the resource has been successfully created, next we’ll create methods for the status resource:

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/u0vpi8krw0m6kq6pv40s.png)


When you click on the “Create Method” icon, you will be brought to the tab below, select the method “get” and click the checkmark. Select lambda function as the Integration type. Ensure to click on the button to turn on lambda proxy integration. 

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/rw3v3lymfrugqwptf3kh.png)



Specify your Lambda function name (`api_processing`), leave the other options as default. Click ‘create method”. 

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/kpxxpc0cdd81h4vukbdx.png)

The get method for resource status has been successfully created. 


We’ll need to create a new resource and give it the name `employee` and in the resource path select `/`. Ensure to check the button to enable CORS for cross-origin requests. And click create resource.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/9vigu8il6jcq85uqvtel.png)


The newly created resource should look like this.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/dcqau6v01elx66ee1whv.png)


We’ll need to create a new resource and give it the name `employees` and in the resource path select `/`. Ensure to check the button to enable CORS for cross-origin requests. And click create resource.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/j6cr0jg6j4r9t820hdmb.png)



The newly created resource should look like this, next we’ll create ‘get’ method  for the employees resource:

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/bm108suuwmei2nt176wp.png)


Click on the “Create Method” icon, you will be brought to the tab below, select the method “get” and click the checkmark. Select lambda function as the Integration type. Ensure to click on the button to turn on lambda proxy integration. 

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/o25yepmrnolybvuxwvft.png)


Specify your Lambda function name (`api_processing`), leave the other options as default. Click ‘create method’, then confirm.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/g0qz6vzu7tu5prfc35s5.png)



The newly created get method for the /employees resource should look like this.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/khlnjb79dlu3f0l27ee9.png)




Lastly we will need to create the CRUD methods for the employee resource. For each HTTP method (GET, POST, DELETE, PATCH):
“Click on create Method” icon, you will be brought to the tab below, select the method “get” and click the checkmark. Select lambda function as the Integration type. Ensure to click on the button to turn on lambda proxy integration. 

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/b3qkbc13b71h45elbm3m.png)





Specify your Lambda function name (`api_processing`), leave the other options as default. Click ‘create method’, then confirm.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/kt38a1pqpcapqp8q0pba.png)



The newly created get method should look like this.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/pxx102ampt5njjk8904b.png)


In the “Create Method” icon, you will be brought to the tab below, select the method “post” and click the checkmark. Select lambda function as the Integration type. Ensure to click on the button to turn on lambda proxy integration. Specify your Lambda function name (`api_processing`), leave the other options as default. Click ‘create method’, then confirm.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/awi2qjehqxvnpneytdqq.png)

The newly created post method should look like this.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/y28cfketjsa6fjdvrfxh.png)


In the “Create Method” icon, you will be brought to the tab below, select the method “delete” and click the checkmark. Select lambda function as the Integration type. Ensure to click on the button to turn on lambda proxy integration. Specify your Lambda function name (`api_processing`), leave the other options as default. Click ‘create method’, then confirm.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/d3loj0etahl4hprp3q4r.png)


The newly created delete method should look like this.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/n9ybl82rjmyks0x6doma.png)


In the “Create Method” icon, you will be brought to the tab below, select the method “patch” and click the checkmark. Select lambda function as the Integration type. Ensure to click on the button to turn on lambda proxy integration. Specify your Lambda function name (`api_processing`), leave the other options as default. Click ‘create method’, then confirm.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/us5mt2ik78oaq10fsrra.png)

The newly created patch method should look like this.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/k4lzmc50iocil3u0w9e3.png)

Resources: These are endpoints in your API (e.g., /employee or /status). They represent logical containers for HTTP methods and map directly to backend operations.
Methods: These are the HTTP operations (like GET, POST, PUT, DELETE) applied to a resource. They determine what action is performed on the resource. For example:
GET: Fetch data.
POST: Create data.
PUT: Update existing data.
DELETE: Remove data.
API Gateway integrates these resources and methods with AWS Lambda to execute backend logic or interact with a database like DynamoDB

Click on deploy API
In the deploy API overview, a box would pop up for you to choose a stage (New Stage), and then type in a stage name (dev), you can fill in the description box or leave it blank, it is optional. Then click on deploy.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/83bg727a3n80vu4nwcd3.png)


By following these steps, you will have successfully created a REST API named “serverless-demo” that serves as an HTTP endpoint for your Lambda function. In the subsequent steps. Take note of the invoke URL, we will need it later.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/lk844rk9t7u4v1f1lbri.png)


Step FOUR: Test your API

This right here is the code editor overview, and we have the default lambda code, so we will try to run the invoke url code to test its functionality. You can use tools like Postman or a browser to make API requests. We should get the response “Hello from Lambda!” on our screen. 

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/gv72av1rnppackyambzq.png)


Here is the invoke url code on our browser, we will include the pathway to our status resource (e.g /status) at the end of the URL code. The essence of the status resource is to test the functionality..

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/c6u45m0g7dfr0i6d5mjj.png)



This is what we got on browser, “Hello from Lambda!” which shows that everything in our application and code is working fine.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/kcmnbdvdscpausqec46s.png)


Preferably we will also try the invoke url code on postman, we will also include the pathway to our status resource (e.g /status) at the end of the URL code. We will also select the “get” option and click on send..

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/wl765lewldaqtakcv47c.png)



This is the output on lambda: “Hello from Lambda!”

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/yviuo57fshbthbl29et8.png)



Next we will need to open the lambda dashboard and navigate to the code editor’s tab and replace the existing code with the following code snippet:
Here, we will write a logic for each CRUD operation:


POST: Insert a new record in DynamoDB.
GET: Fetch all or a specific record from DynamoDB.
UPDATE: Update a record.
DELETE: Remove a record.

You can write or upload your code on the console editor:
 
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/43akprl4td36r6eza6dt.png)

Here is the link to the [Code on my github](https://github.com/Uwadon1/AWS_RestLambdaAPI/blob/main/lambda_function.py)


This function serves as a robust backbone for employee management, leveraging the power of AWS Lambda and DynamoDB in a RESTful architecture.

Next we hit the deploy button to update our new python code.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/lj2ylwj526pyslgwgpch.png)


We will move, right away to test our Lambda function. We will need to choose our test event e.g: “test-crud” and save

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/v38sc30l9efg2cull224.png)



We will retest the /status ‘get’  API endpoint with Postman. Using the invoke url code on our browser, we will include the pathway to our status resource (e.g /status) at the end of the URL code. We will get the output below.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/gi0m7e1rrgcydxsxfj06.png)


Using the browser, we got this output. “Service is operational” which shows that everything in our application and code is working fine.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/pdt5yq95w6e7utnnapqo.png)

We will test the /employee ‘post’ API endpoint with Postman. Using the invoke url code on our browser, we will include the pathway to our employee resource (e.g employee) at the end of the URL code. We will choose the following options as shown below, choose ‘post’ option, and select body, then raw, and json format to insert in our command as json.

```
{
“Employeeid”: “102”,
“job_title”: “CEO”,
“full_name”: “John Tom”,
“salary”: “1000000”
}.
```
 
Note we changed the last endpoint from /status to /employee. We should see ‘200 okay’. Which shows everything is functional. We should also see our output in the down box.
We will follow the same steps to input two other values.


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/w0rzdhorl0e4n8or62ae.png)

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/nwb7vmumhpokuit9qieg.png)


We will test the /employee ‘delete’ API endpoint with Postman. Using the invoke url code on our browser, we will include the pathway to our employee resource (e.g /employee) at the end of the URL code. We will choose the following options as shown below, choose ‘delete’ option, and type in our command as json: 
`{
“Employee”: “102”
}. `
We should see the output below “Operation”: “DELETE”, and ‘200 okay’. Which shows everything is functional.
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/govbq4581pnjwmer71x2.png)

We will test the /employee ‘patch’ API endpoint with Postman. Using the invoke url code on our browser, we will include the pathway to our employee resource (e.g /employee) at the end of the URL code. We will choose the following options as shown below, choose patch’ option, and type in our command as json: 
`{
“EmployeeId”: “103”,
“updateKey”: “full_name”,
“updateValue”: “Chioma Chukwu”
}. `
We should see the output below “Operation”: “UPDATE”, and ‘200 okay’. Which shows everything is functional.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zd9gc0b0sw7ch777j6dy.png)



Lastly, we will test the /employees ‘get’  API endpoint with Postman. Using the invoke url code on our browser, we will include the pathway to our employees resource (e.g /employees) at the end of the URL code. We will get the info output of all the employees in the company .

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/5dq34hdr9c10qw6zjy2g.png)



We will go into our Dynamo DB dashboard to verify if all we have created using our API and lambda would reflect inside our database

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/b9267okynaonzse3545y.png)



Step FIVE: Monitoring the logs and insights
Using cloudwatch we can monitor the logs for insights to check for errors and troubleshoot where necessary


