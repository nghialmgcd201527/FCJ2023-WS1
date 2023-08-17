---
title: "Viewing the data in DynamoDB"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 6.1 </b> "
---

1. Type **DynamoDB** in the service search bar on the AWS Console then select **DynamoDB**.

![VPC](/images/6.test/6.1-dynamodb/6.1-1new.png)

2. In the left navigation bar, select **Tables.**
3. Click on the table whose name includes the name of our application **tasks-app-TasksTable.** This is the table created by the SAM template.

![VPC](/images/6.test/6.1-dynamodb/6.1-2new.png)

4. Click the **Explore table items** button.

![VPC](/images/6.test/6.1-dynamodb/6.1-3new.png)

We will see the data in the table in **Items returned.**

![VPC](/images/6.test/6.1-dynamodb/6.1-4new.png)

Clicking on the partition key (**user**) of that table will take you to where to edit the information of that item.

![VPC](/images/6.test/6.1-dynamodb/6.1-5.png)

When you change data in DynamoDB, we need to refresh the application's web page to see the change.
