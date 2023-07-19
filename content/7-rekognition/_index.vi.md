---
title: "Cấu hình cho trích xuất metadata từ hình ảnh: Amazon Rekognition"
date: "`r Sys.Date()`"
weight: 7
chapter: false
pre: " <b> 7. </b> "
---

#### Amazon Rekognition là gì?

Amazon Rekognition giúp chúng ta dễ dàng tải hình ảnh và video và phân tích cho ứng dụng của bạn bằng cách sử dụng công nghệ deep learning (khi sử dụng không cần phải có chuyên môn ở mảng machine learning). Với Amazon Rekognition, bạn có thể xác định objects, people, text, scenes và các activities trong ảnh và videos.

Ở bước này, chúng ta sẽ thêm tính năng dò tìm những objects của ảnh được tải lên và gán labels cho những objects đó.

#### Cấu hình Amazon Rekognition Integration

Vào thư mục **sam/src/handlers/detectLabels** và mở file **app.js**. Coppy đoạn code dưới đây và past vào file đó:

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

Chức năng chính của đoạn code trên:

- Đoạn code import AWS SDK và khởi tạo các client cho DynamoDB và Rekognition.
- Lấy biến môi trường **TASKS_TABLE** để tạo ra một biến tên **tableName.**
- Function trích xuất thông tin liên quan đến event object. Nó truy xuất buckect và filename của object được tải lên trong S3 bucket.
- Nó tách các key ra để lấy người dùng và ID của task liên kết với object được tải lên.
- **UpdateCommand** được tạo ra để cập nhật item trong DynamoDB. Command được log ở console cho mục đích debug.

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

- Funtion chạy đến **UpdateCommand** để cập nhật item trong DynamoDB. Nó log message khi lưu thông tin của object được tải lên và URL của object đó.
- Nếu cập nhật thành công, sẽ có response được log. Nếu không, sẽ có error được log và throw ra.

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

- Function được thực thi để detect các label của hình ảnh được tải lên bằng Amazon Rekognition. Nó log thông báo của hành động detec label trong bucket.
- Object imageParams được tạo ra để xác định hình ảnh (S3 object) được detect label.
- **DetectLabelsCommand** được tạo ra để gọi Rekognition API để detect label của hình ảnh.
- Các detected label được lưu trữ trong mảng các labels.

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

Tóm lại, Lambda function này xử lí các sự kiện khi có object được tải lên trong S3 bucket. Nó cập nhật item trong DynamoDB với thông tin được tải lên như S3 URL. Sau đó, nó sử dụng Amazon Rekognition để detect labels trong hình ảnh và cập nhật item trong DynamoDB với các detected labels. Function phản hồi bằng một object dạng JSON bao gồm detected labels.

Tiếp theo chúng ta cần thêm một resource mới `AWS::Serverless::Function` vào file **template.yaml** trong thư mục **sam,** trong mục **Resources,** dưới function **UploadsBucket.** Coppy và paste đoạn code bên dưới vào file **template.yaml** trong thư mục **sam.**

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

Chúng sẽ làm như hình dưới đây:

![VPC](/images/7.rekog/7-1.png)

Chú ý ở thuộc tính **ObjectCreatedEvent.** Event này sẽ trigger Lambda function khi một object được tải lên bucket.

#### Deploy lại ứng dụng sau khi thay đổi

Với sự thay đổi code của chúng ta khi chỉnh sửa file **template.yml** và thêm file **detectLabels.js,** chúng ta cần build và deploy lại ứng dụng để thực thi các thay đổi. Chúng ta sẽ deploy chạy lệnh sau:

{{% notice warning %}}

Hãy chắn chắn rằng ở terminal, bạn đang ở thư mục **sam**, chạy câu lệnh này `cd ~/environment/serverless-tasks-webapp/sam
`

{{%/notice%}}

Đầu tiên chúng ta sẽ build ứng dụng:

```
sam build
```

Khi build thành công, chúng ta sẽ thấy như hình:
![VPC](/images/7.rekog/7-2.png)

Tiếp đến, chúng ta sẽ deploy ứng dụng:

```
sam deploy
```

Sau khi deploy thành công, chúng ta sẽ thấy như hình

![VPC](/images/7.rekog/7-3.png)

Bây giờ, chúng ta đã có thể trải nghiệm Amazon Rekognition ở ứng dụng của chúng ta.

Amazon Rekognition sẽ phân tích các hình ảnh được lưu trữ ở Amazon S3 bucket. Ứng dụng web của chúng ta cho phép bạn được tải ảnh lên và gán vào task. Chúng ta làm như sau:

1. Sau khi các bạn tạo một task mới, ở phần **My Tasks**, các bạn sẽ thấy nó và nút **Choose file.** Click vào nút đó để chọn một ảnh từ máy tính của bạn có đuôi PNG, JPEG, etc.
2. Click vào nút **Upload** để tải ảnh lên.

![VPC](/images/7.rekog/7-4new.png)

Sau khi làm các bước trên, bạn sẽ thấy dòng hiển thị quá trình **Detecting Labels.**

![VPC](/images/7.rekog/7-5new.png)

Sau khi quá trình detect hoàn tất, bạn sẽ thấy các label được detect hiển thị.

![VPC](/images/7.rekog/7-6new.png)

#### Mô tả lại quá trình detect label vừa thực hiện

![VPC](/images/7.rekog/swimlane.svg)

1. Hình ảnh được tải lên vào S3 bucket
2. S3 bucket được cấu hình cho phép trigger Lambda function **DetectLabels** khi có object được tạo ra trong bucket.
3. Lambda function **DetectLabels** được gọi và nó sử dụng Amazon Rekognition để detect label của hình ảnh.
4. Detected lables được lưu vào DynamoDB.
5. Ứng dựng refreshes thông tin chi tiết của task.

Để ý rằng cách chúng ta thêm một tính năng mới vào ứng dụng của chúng ta mà không thay đổi quá nhiều. Điều này chứng tỏ sự linh hoạt của serverless architectures - khả năng mở rộng và tính module hóa của nó.
