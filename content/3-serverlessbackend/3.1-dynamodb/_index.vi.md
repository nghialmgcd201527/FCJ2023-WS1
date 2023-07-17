---
title: "Khai báo Table trong DynamoDB"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

#### Amazon DynamoDB là gì?

[Amazon DynamoDB](https://aws.amazon.com/dynamodb/) là một cặp key-value serverless và là một loại cơ sở dữ liệu phi quan hệ (Non-SQL) mang lại hiệu suất một phần nghìn giây ở mọi quy mô.

Tương tự với những cơ sở dữ liệu khác, Amazon DynamoDB sẽ lưu trữ data bằng các bảng. Trong ứng dụng của chúng ta, chúng ta sẽ lưu trữ thông tin của các tasks trong bảng của DynamoDB. Bảng này sẽ được truy cập bằng Lambda function bằng sự phản hồi đến API từ ứng dụng web của chúng ta.

Chúng ta sẽ sử dụng SAM để khởi tạo DynamoDB table.

#### Thêm DynamoDB table vào SAM template

Dòng `AWS:DynamoDB::Table` được dụng để định nghĩa DynamoDB table.

Hãy vào file `template.yaml` trong thư mục **sam**, thêm đoạn code dưới đây vào mục **Resource** và bên dưới function **MyAuthFunction**.

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

Trước hết bạn sẽ có **AttributeDefinitions** là nơi định nghĩa những thuộc tính (columns) trong bảng DynamoDB. Trong bảng của chúng ta, có 2 thuộc tính được định nghĩa là **users** và **id**. Cả 2 đều thuộc kiểu **string** (`AttributeType: "S"`).\
**KeySchema** định nghĩa khóa chính của table. Khóa chính của table là sự kết hợp của 2 thuộc tính **user** và **id**. **user** là hash key (`KeyType: "HASH"`) và **id** là range key (`KeyType: "RANGE"`). Điều này cho biết rằng bảng sẽ được quản lí, phân vùng dựa trên thuộc tính **user** và **id.**\
**BillingMode: PAY_PER_REQUEST** xác định cách thanh toán cho table đó là thanh toán theo yêu cầu.

{{% notice warning %}}
Cú pháp trong YAML có phân biệt khoảng trắng, vì vậy hãy chắc chắn rằng phạm vi của function **TasksTable** được thụt lề vào sâu hơn so với phạm vi của **Resources.**
{{% /notice %}}

Chúng ta sẽ có kết quả như hình bên dưới.

![VPC](/images/3.serverlessbackend/3.1-dynamodb/3.1-1.png)

Để biết thêm thông tin về `AWS::Serverless::Table`, hãy tham khảo resource của SAM [tại đây](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-dynamodb-table.html).
