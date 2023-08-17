---
title: "Define a DynamoDB table"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

#### What is Amazon DynamoDB?

[Amazon DynamoDB](https://aws.amazon.com/dynamodb/) is a serverless key-value pair and a type of non-relational (Non-SQL) database that delivers millisecond performance at all sizes.

Similar to other databases, Amazon DynamoDB stores data in tables. In our application, we will store the information of the tasks in the table of DynamoDB. This table will be accessed by the Lambda function in response to the API from our web application.

We will use SAM to initialize the DynamoDB table.

#### Add the DynamoDB table to the SAM template

The line `AWS:DynamoDB::Table` is used to define the DynamoDB table.

Go to the `template.yaml` file in the **sam** folder, add the following code in the **Resource** section and below the **MyAuthFunction** function.

```
# Create DynamoDB table
  TasksTable:
    Type: AWS::DynamoDB::Table
    Properties:
      AttributeDefinitions:
        - AttributeName: "user"
          AttributeType: "S"
        - AttributeName: "id"
          AttributeType: "S"
      KeySchema:
        - AttributeName: "user"
          KeyType: "HASH"
        - AttributeName: "id"
          KeyType: "RANGE"
      BillingMode: PAY_PER_REQUEST

```

First you will have **AttributeDefinitions** where the attributes (columns) are defined in the DynamoDB table. In our table, there are 2 attributes defined as **users** and **id**. Both are of type **string** (`AttributeType: "S"`).

**KeySchema** defines the primary key of the table. The primary key of the table is a combination of the two attributes **user** and **id**. **user** is the hash key (`KeyType: "HASH"`) and **id** is the range key (`KeyType: "RANGE"`). This indicates that the table will be managed, partitioned based on the **user** and **id** attributes.

**BillingMode: PAY_PER_REQUEST** defines how to pay for that table as payment on demand.

{{% notice warning %}}
The syntax in YAML is space sensitive, so make sure the scope of the **TasksTable** function is indented deeper than the scope of **Resources.**
{{% /notice %}}

We will have the result as shown below.

![VPC](/images/3.serverlessbackend/3.1-dynamodb/3.1-1.png)

For more information about `AWS::Serverless::Table`, refer to SAM's resource [here](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-dynamodb-table.html).
