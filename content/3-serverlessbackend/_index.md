---
title: "Build a serverless backend: AWS Lambda and AWS SAM"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

#### Overview

In this section, we will use the Serverless Application Model (SAM) to build a backend process to handle requests from the web application. The application that we are going to deploy here allows users to create todo tasks and assign files to those tasks. To meet those requirements, JavaScript running in the browser needs to rely on a service in the cloud.

You will use a Lambda function so that every time a user creates a task, it will be called to execute. This function will store the task to DynamoDB, then will send the response to the front-end and update the new task on the front-end.

The function is called from the browser using Amazon API Gateway. You will implement this connection later. In this section, you will only test your function in isolation.

#### What is SAM?

[Serverless Application Model (SAM)](https://aws.amazon.com/serverless/sam/) is an open-source framework that will make serverless application deployment easier. It provides a simple way to define serverless applications, and provides a set of tools for deploying those applications.

It allows us to specify the requirements of the application in code. SAM transforms and extends the SAM syntax to AWS Cloudformation to deploy your applications. You will see and use SAM templates throughout this workshop.

#### How does SAM work?

AWS SAM is based on AWS Cloudformation. A serverless application is defined with a CloudFormation template and deployed with the CloudFormation stack. In short, the AWS SAM template is a CloudFormation template.

AWS SAM defines a set of resources that describe common components of a serverless application. In order for AWS SAM to have objects defined in the CloudFormation template, the template must include a Transform section in the document root of `AWS::Serverless-2016-10-31`.

#### Our sample application: Tasks API

The API Tasks that we will build in this workshop include Amazon API Gateway HTTP endpoints to trigger AWS Lambda functions, which will read and write data to the Amazon DynamoDB database. The SAM template of the Tasks API will include a DynamoDB table, Lambda functions to list, view, and update Tasks in the table.

In this section, you will work with a Lambda function to create a new task. Tasks API components are defined in `template.yaml`. Next we will go into detail about the structure of the Lambda function.

#### Anatomy of SAM template

Go to the **serverless-tasks-web** folder followed by the **sam** folder then open the **template.yaml** file. Below is a piece of code in the SAM template file that lists the tasks:

![VPC](/images/3.serverlessbackend/3-1.png)

These are the properties defined in the resource `AWS:Serverless:Function`, we will start to learn about them.

**FunctionName**\
The **FunctionName** property is optionally named for the Lambda function. If we don't name it, CloudFormation will use the name of CloudFormation Stack, CloudFormation Resource and random ID.

**CodeUri**\
The **CodeUri** attribute specifies the path to the source code of the Lambda function in the workspace associated with the SAM template in the `sam/` path. In this example, the Code snippet will be in the path `src/handlers/` and `getTasks` will be its function.

**Handler**\
The **Handler** attribute specifies the entry point for the Lambda function. For Javascript, it will be formatted as **file.function**, where file is the filename containing the function without the extension **.js** belonging to the path **CodeUri** defined above and **function** is the name of the function in that file that will be executed when the Lambda function is called.

**Environment**\
The **Environment** attribute defines an environment variable, which will be available in the Lambda function. In this example, we are defining an environment variable **TASKS_TABLE** with a value of **TasksTable**. This environment variable will be used in the Lambda function to access the DynamoDB table where the tasks are stored.

**Events**\
The **Events** attribute defines the events that will trigger the Lambda function when called. [Event API](https://github.com/aws/serverless-application-model/blob/master/versions/2016-10-31.md#api) is integrated with Lambda function and API Gateway endpoint, however SAM only supports Lambda function triggers from [the following sources](https://github.com/aws/serverless-application-model/blob/master/versions/2016-10-31.md#event-source-types) .

**The event API** is used to view the details of a Task defined in the RESTful resource `/tasks` and accessed using the HTTP GET method. SAM will convert API event to API Gateways to call Lambda function.

### Content

- [Define a DynamoDB table](3.1-dynamodb/)
- [Create a Lambda function](3.2-lambdafunction/)
