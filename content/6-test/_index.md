---
title: "Test the application"
date: "`r Sys.Date()`"
weight: 6
chapter: false
pre: " <b> 6. </b> "
---

![VPC](/images/6.test/6-1.png)

After logging in with any username or password, you can create a new task by entering **Title** and **Body**. The **Due date** part can be left blank.

Click **Create** to create a new task, it will be displayed in **My Tasks.**

As you will see in the image below.

![VPC](/images/6.test/6-2.png)

When you click the **Create task** button, things work:

1. Request with **POST** method is sent to API Gateway endpoint **/tasks**.
2. The request is authenticated by the Lambda authorizer, using the HTTP header `Authentication: Bearer <token>`.
3. The body of that request is passed to the Lambda function **CreateTaskFunction.**
4. The task information entered above will be stored in the DynamoDB table.
5. An HTTP status code 200 response will be returned to the web application.
6. The web application refreshes the list of tasks because the request with the **GET** method is sent to the API Gateway endpoint **/tasks.**
7. Again, the request is authenticated by the Lambda authorizer and passed to the Lambda function **GetTasksFunction.**
8. Lambda function **GetTasksFunction** retrieves the list of tasks from the DynamoDB table and returns the list of tasks to the authenticated user.

Let's create more new tasks and try deleting one to get a better understanding of how requests are processed.

### Content

- [Viewing the data in DynamoDB](6.1-dynamodb/)
- [Logging and Monitoring](6.2-logandmonitor/)
