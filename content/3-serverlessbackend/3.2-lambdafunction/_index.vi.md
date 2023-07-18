---
title: "Tạo Lambda function"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

#### AWS Lambda là gì?

AWS Lambda là một dịch vụ serverless nó sẽ thực thi những dòng code mà không cần phải chuẩn bị và quản lí servers. Lambda tự động phân bổ dung lượng máy tính và chạy code của bạn dựa vào những yêu cầu hoặc sự kiện được gửi đến trong tất cả các môi trường, các điều kiện.

Cách nó hoạt động:

1. Tải code của bạn lên AWS Lambda hoặc viết code trên trình chỉnh sửa của Lambda. (Trong workshop này, chúng ta sẽ viết code và tải lên bằng SAM)
2. Cài đặt code của bạn cho phép trigger từ những AWS services khác, từ HTTP endpoints, hay là những hoạt động xảy ra bên trong ứng dụng.
3. AWS Lambda sẽ thực thi code của bạn khi trigger được kích hoạt, sau đó nó sẽ tự động quản lí tài nguyên tính toán cho code của bạn.
4. Chỉ trả tiền cho thời gian thực thi code của bạn (và số lượng tài nguyên tính toán mà code của bạn sử dụng).

#### Phân tích một Lambda function

Lambda function **handler** là một hàm trong code của bạn, nó sẽ xử lí các events. Khi một chức năng được gọi, Lambda sẽ thực thi hàm **handler.** Khi hàm **handler** kết thúc và return response, nó sẽ sẵn sàng thực hiện những events khác.

Ví dụ về cấu trúc của Lambda function:

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

Ở đây, **event** là request được gửi đến và **response** là kết quả trả về.

#### Tạo Lambda function

Truy cập đến đường dẫn **sam/src/handlers/createTask** và chọn file tên là **app.js**, coppy và paste đoạn code sau vào file:

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

Đoạn code này đã import thư viện **AWS SDK** cho DynamoDB, thư viện **uuid** cho việc nhận dạng, cài đặt DynamoDB client.

Lambda function được khởi tạo với `exports.handler`, nó đóng vai trò như một **entry point** của function. Nó lấy object **envent** làm tham số đầu vào và trả về một object **response.**

Nó tạo ra object **params** với những yếu tố cần thiết, bao gồm tên của DynamoDB table, thuộc tính của item trong table **(user, id, title, bodyText, dueDate, createdAt).**

Function ghi lại dữ liệu vào DynamoDB table bằng **PutCommand** và logs ra lời nhắn thành công.

Response sẽ trả về một object với **statusCode** là 200, đặt headers với CORS và **body** là dữ liệu được ghi lại.

Tóm lại, Đoạn code này phục vụ cho việc tạo và cập nhật task trong DynamoDB table dựa vào việc gửi request API. Nó tận dụng AWS SDK dành cho DynamoDB, Node.js và AWS Lambda để cung cấp giải pháp quản lý tác vụ của serverless và mở rộng quy mô.

#### Thêm Lambda function vào SAM template

Coppy và paste đoạn code dưới đây vào mục Resource trong file **template.yml,** sau function **TasksTable.**

{{% notice warning %}}
Cú pháp trong YAML có phân biệt khoảng trắng, vì vậy hãy chắc chắn rằng phạm vi của function **CreateTaskFunction** được thụt lề vào sâu hơn so với phạm vi của **Resources.**
{{% /notice %}}

Giá trị `AWS::Serverless::Function` được dùng để khởi tạo Lambda function. Thuộc tính **CodeUri** được dùng để xác định rõ vị trí của file **app.js** trong thư mục `src/handlers/createTask`.

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

Chi tiết và công dụng của các thuộc tính được khai báo ở trên các bạn có thể xem lại ở phần [này](/vi/3-serverlessbackend/).

Hãy làm như hình bên dưới.

![VPC](/images/3.serverlessbackend/3.2-lambdafunction/3.2-1.png)
