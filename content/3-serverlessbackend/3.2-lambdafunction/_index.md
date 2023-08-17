---
title: "Create a Lambda function"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

#### What is AWS Lambda?

AWS Lambda is a serverless compute service that lets you run code without provisioning or managing servers. Lambda automatically allocates compute power and runs your code based on the incoming request or event, for any scale of traffic.

How it works:

1. Upload your code to AWS Lambda or write code in Lambda's code editor. (In this workshop, we'll be writing code and uploading it using SAM.)
2. Set up your code to trigger from other AWS services, HTTP endpoints, or in-app activity.
3. Lambda runs your code only when triggered, using only the compute resources needed.
4. Just pay for the compute time you use.

#### Anatomy of a Lambda function

The Lambda function **handler** is the method in your function code that processes events. When your function is invoked, Lambda runs the **handler** method. When the **handler** exits or returns a response, it becomes available to handle another event.

Example Lambda function structure:

```
exports.handler = async (event) => {
    // TODO implement
    const response = {
        statusCode: 200,
        body: JSON.stringify('Hello from Lambda!'),
    };
    return response;
};

```

Here, the **event** is the incoming request. The **response** is the outgoing response.

#### Create a Lambda function

Go to the path **sam/src/handlers/createTask** and select the file named **app.js**, copy and paste the following code into the file:

```
const { DynamoDBClient } = require('@aws-sdk/client-dynamodb')
const { DynamoDBDocumentClient, PutCommand } = require('@aws-sdk/lib-dynamodb')
const uuid = require('uuid')

const ddbClient = new DynamoDBClient()
const ddbDocClient = DynamoDBDocumentClient.from(ddbClient)
const tableName = process.env.TASKS_TABLE

exports.handler = async (event) => {
  console.info('received:', event)

  const body = JSON.parse(event.body)
  const user = event.requestContext.authorizer.principalId
  const id = uuid.v4()
  const title = body.title
  const bodyText = body.body
  const createdAt = new Date().toISOString()

  let dueDate = createdAt

  if ('dueDate' in body) {
    dueDate = body.dueDate
  }

  const params = {
    TableName: tableName,
    Item: { user: `user#${user}`, id: `task#${id}`, title: title, body: bodyText, dueDate: dueDate, createdAt: createdAt }
  }

  console.info(`Writing data to table ${tableName}`)
  const data = await ddbDocClient.send(new PutCommand(params))
  console.log('Success - item added or updated', data)

  const response = {
    statusCode: 200,
    headers: {
      'Access-Control-Allow-Origin': '*'
    },
    body: JSON.stringify(data)
  }
  return response
}

```

This code imported the **AWS SDK** library for DynamoDB, the **uuid** library for identification, and installed the DynamoDB client.

Lambda function is initialized with `exports.handler`, which acts as an **entry point** of the function. It takes an **envent** object as an input parameter and returns a **response.** object

It creates a **params** object with the necessary elements, including the name of the DynamoDB table, the properties of the item in the table **(user, id, title, bodyText, dueDate, createdAt).**

The function logs data to the DynamoDB table with **PutCommand** and logs a success message.

Response will return an object with **statusCode** of 200, set headers with CORS and **body** as the logged data.

In short, this code serves to create and update tasks in DynamoDB table based on sending API request. It leverages the AWS SDK for DynamoDB, Node.js, and AWS Lambda to provide a scalable and serverless task management solution.

#### Add the Lambda function to the SAM template

Copy and paste the code below into the Resource section of the file **template.yml,** after the function **TasksTable.**

{{% notice warning %}}
The syntax in YAML is space sensitive, so make sure the scope of the **CreateTaskFunction** function is indented deeper than the scope of **Resources.**
{{% /notice %}}

The value `AWS::Serverless::Function` is used to initialize the Lambda function. The **CodeUri** attribute is used to specify the location of the **app.js** file in the `src/handlers/createTask` directory.

```
# CreateTask Lambda Function
  CreateTaskFunction:
    Type: AWS::Serverless::Function
    Properties:
      CodeUri: src/handlers/createTask
      Handler: app.handler
      Policies:
        - DynamoDBCrudPolicy:
            TableName: !Ref TasksTable
      Environment:
        Variables:
          TASKS_TABLE: !Ref TasksTable
      Events:
        PostTaskFunctionApi:
          Type: Api
          Properties:
            RestApiId: !Ref TasksApi
            Path: /tasks
            Method: POST
            Auth:
              Authorizer: MyLambdaTokenAuthorizer

```

Details and uses of the attributes declared above can be reviewed in [this](/vi/3-serverlessbackend/).

Do as shown below.

![VPC](/images/3.serverlessbackend/3.2-lambdafunction/3.2-1.png)
