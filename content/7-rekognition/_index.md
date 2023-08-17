---
title: "Configure image metadata extraction: Amazon Rekognition"
date: "`r Sys.Date()`"
weight: 7
chapter: false
pre: " <b> 7. </b> "
---

#### What is Amazon Rekognition?

Amazon Rekognition makes it easy to add image and video analysis to your applications using deep learning technology that requires no machine learning expertise to use. With Amazon Rekognition, you can identify objects, people, text, scenes, and activities in images and videos.

In this step, we will add a feature to detect objects in an uploaded image and apply labels to the detected objects.

#### Configure Amazon Rekognition Integration

Go to the **sam/src/handlers/detectLabels** folder and open the **app.js** file. Copy the code below and paste it into that file:

```
const { DynamoDBClient } = require('@aws-sdk/client-dynamodb')
const { DynamoDBDocumentClient, UpdateCommand } = require('@aws-sdk/lib-dynamodb')
const { RekognitionClient, DetectLabelsCommand } = require('@aws-sdk/client-rekognition')
const ddbClient = new DynamoDBClient()
const ddbDocClient = DynamoDBDocumentClient.from(ddbClient)
const rekognitionClient = new RekognitionClient()
const tableName = process.env.TASKS_TABLE

exports.handler = async (event) => {
  console.info(JSON.stringify(event, null, 2))

  const bucket = event.Records[0].s3.bucket.name
  const key = decodeURIComponent(event.Records[0].s3.object.key.replace(/\+/g, ' '))

  const user = key.split('/')[0]
  const taskId = key.split('/')[1]

  const command = new UpdateCommand({
    TableName: tableName,
    Key: { user: `user#${user}`, id: `task#${taskId}` },
    UpdateExpression: 'SET upload = :u',
    ExpressionAttributeValues: {
      ':u': `s3://${bucket}/${key}`
    },
    ReturnValues: 'UPDATED_NEW'
  })

  console.log(`UpdateCommand: ${JSON.stringify(command, null, 2)}`)

  try {
    console.log(`Saving upload for task ${taskId}: s3://${bucket}/${key}`)
    const data = await ddbDocClient.send(command)
    console.log('UpdateItem succeeded:', JSON.stringify(data, null, 2))
  } catch (err) {
    console.log('Unable to update item. Error JSON:', JSON.stringify(err, null, 2))
    throw err
  }

  console.log(`Detecting labels for bucket ${bucket} and key ${key}`)

  const imageParams = {
    Image: {
      S3Object: {
        Bucket: bucket,
        Name: key
      }
    }
  }

  const labelData = await rekognitionClient.send(
    new DetectLabelsCommand(imageParams)
  )
  console.log('Success, labels detected.', labelData)
  const labels = []
  for (let j = 0; j < labelData.Labels.length; j++) {
    const name = labelData.Labels[j].Name
    labels.push(name)
  }

  console.log(`Label data: ${JSON.stringify(labels)}`)

  const updateLabelsCommand = new UpdateCommand({
    TableName: tableName,
    Key: { user: `user#${user}`, id: `task#${taskId}` },
    UpdateExpression: 'SET labels = :s',
    ExpressionAttributeValues: {
      ':s': labels
    },
    ReturnValues: 'UPDATED_NEW'
  })

  try {
    console.log(`Saving labels for task ${taskId}: ${labels}`)
    const data = await ddbDocClient.send(updateLabelsCommand)
    console.log('UpdateItem succeeded:', JSON.stringify(data, null, 2))
  } catch (err) {
    console.log('Unable to update item. Error JSON:', JSON.stringify(err, null, 2))
    throw err
  }

  const response = {
    statusCode: 200,
    headers: {
      'Access-Control-Allow-Origin': '*'
    },
    body: JSON.stringify(labels)
  }
  return response
}

```

The main function of the above code:

- The code imports the AWS SDK and initializes the clients for DynamoDB and Rekognition.
- Take the environment variable **TASKS_TABLE** to create a variable named **tableName.**
  The function extracts information related to the event object. It retrieves the buckect and filename of the object uploaded in the S3 bucket.
- It separates the keys to get the user and task ID associated with the uploaded object.
- **UpdateCommand** is created to update items in DynamoDB. Commands are logged in the console for debugging purposes.

```
  const command = new UpdateCommand({
    TableName: tableName,
    Key: { user: `user#${user}`, id: `task#${taskId}` },
    UpdateExpression: 'SET upload = :u',
    ExpressionAttributeValues: {
      ':u': `s3://${bucket}/${key}`
    },
    ReturnValues: 'UPDATED_NEW'
  })

  console.log(`UpdateCommand: ${JSON.stringify(command, null, 2)}`)

```

- Function runs to **UpdateCommand** to update the item in DynamoDB. It logs a message when it saves the uploaded object's information and the object's URL.
- If the update is successful, the response will be logged. Otherwise, an error will be logged and thrown.

```
  try {
    console.log(`Saving upload for task ${taskId}: s3://${bucket}/${key}`)
    const data = await ddbDocClient.send(command)
    console.log('UpdateItem succeeded:', JSON.stringify(data, null, 2))
  } catch (err) {
    console.log('Unable to update item. Error JSON:', JSON.stringify(err, null, 2))
    throw err
  }

```

- The function is executed to detect the labels of images uploaded using Amazon Rekognition. It logs the message of the detec label action in the bucket.
- Object imageParams is created to identify the image (S3 object) detected label.
- **DetectLabelsCommand** is created to call the Rekognition API to detect the label of the image.
- The detected labels are stored in the array of labels.

```
  console.log(`Detecting labels for bucket ${bucket} and key ${key}`)

  const imageParams = {
    Image: {
      S3Object: {
        Bucket: bucket,
        Name: key
      }
    }
  }

  const labelData = await rekognitionClient.send(
    new DetectLabelsCommand(imageParams)
  )
  console.log('Success, labels detected.', labelData)
  const labels = []
  for (let j = 0; j < labelData.Labels.length; j++) {
    const name = labelData.Labels[j].Name
    labels.push(name)
  }

```

In short, this Lambda function handles events when an object is uploaded in the S3 bucket. It updates the item in DynamoDB with the uploaded information as the S3 URL. It then uses Amazon Rekognition to detect labels in the image and updates the item in DynamoDB with the detected labels. The function responds with a JSON object that includes detected labels.

Next we need to add a new resource `AWS::Serverless::Function` to the **template.yaml** file in the **sam folder,** in the **Resources,** section under the **UploadsBucket function. ** Copy and paste the code below into the **template.yaml** file in the **sam.** folder

```
# DetectLables Lambda Function
  DetectLabelsFunction:
    Type: AWS::Serverless::Function
    Properties:
      CodeUri: src/handlers/detectLabels
      Environment:
        Variables:
          TASKS_TABLE: !Ref TasksTable
      Events:
        ObjectCreatedEvent:
          Type: S3
          Properties:
            Bucket: !Ref UploadsBucket
            Events: s3:ObjectCreated:*
      Handler: app.handler
      Policies:
        - DynamoDBCrudPolicy:
            TableName: !Ref TasksTable
        - RekognitionDetectOnlyPolicy: {}
        - Version: 2012-10-17
          Statement:
            - Effect: Allow
              Action: s3:GetObject*
              Resource: !Sub "arn:aws:s3:::uploads-${AWS::StackName}-${AWS::Region}-${AWS::AccountId}*"
      Runtime: nodejs14.x

```

They will do as shown below:

![VPC](/images/7.rekog/7-1.png)

Notice in the **ObjectCreatedEvent.** property This event will trigger the Lambda function when an object is uploaded to the bucket.

#### Deploy the changes

With our code change editing the **template.yml** file and adding the **detectLabels.js,** file we need to build and redeploy the application to implement the changes. We will deploy running the following command:

{{% notice warning %}}

Make sure that in terminal you are in **sam** directory, run this command `cd ~/environment/serverless-tasks-webapp/sam
`

{{%/notice%}}

First we will build the application:

```
sam build
```

When the build is successful, we will see something like this:

![VPC](/images/7.rekog/7-2.png)

Next, we will deploy the application:

```
sam deploy
```

After successfully deploying, we will see as shown

![VPC](/images/7.rekog/7-3.png)

Now, we can experience Amazon Rekognition in our application.

Amazon Rekognition will analyze images stored in the Amazon S3 bucket. Our web application allows you to upload images and assign them to tasks. We do the following:

1. After you create a new task, in the **My Tasks** section, you will see it and the **Choose file.** button Click on that button to select an image from your computer with the PNG extension. , JPEG, etc.
2. Click the **Upload** button to upload the image.

![VPC](/images/7.rekog/7-4new.png)

After doing the above steps, you will see the line showing the **Detecting Labels.** process

![VPC](/images/7.rekog/7-5new.png)

After the detection process is complete, you will see the detected labels displayed.

![VPC](/images/7.rekog/7-6new.png)

#### What just happened?

![VPC](/images/7.rekog/swimlane.svg)

1. Images uploaded to S3 bucket
2. The S3 bucket is configured to enable the Lambda function **DetectLabels** trigger when an object is created in the bucket.
3. Lambda function **DetectLabels** is called and it uses Amazon Rekognition to detect the label of the image.
4. Detected lables are saved to DynamoDB.
5. The application refreshes the details of the task.

Notice how we add a new feature to our application without changing too much. This demonstrates the flexibility of serverless architectures - its scalability and modularity.
