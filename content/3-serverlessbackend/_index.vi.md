---
title: "Xây dựng một serverless backend với AWS Lambda và AWS SAM"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

#### Tổng quan

Trong phần này, chúng ta sẽ sử dụng Serverless Application Model (SAM) để xây dựng một quy trình backend để xử lí các yêu cầu từ ứng dụng web. Ứng dụng mà tới đây chúng ta sẽ triển khai cho phép người dùng tạo ra những todo tasks và gán file vào các task đó. Để đáp ứng được những yêu cầu đó, JavaScript đang chạy trên trình duyệt cần nhờ vào một dịch vụ trên cloud.

Bạn sẽ dùng Lambda function để mỗi lần user tạo một task, nó sẽ được gọi thực thi. Funtion này sẽ lưu trữ task vào DynamoDB, sau đó sẽ gửi phản hồi đến phần front-end và cập nhật task mới trên giao diện người dùng.

Function được gọi từ trình duyệt bằng Amazon API Gateway. Bạn sẽ triển khai sự kết nối này ở phần sau. Trong phần này, bạn sẽ chỉ kiểm tra function của bạn ở phạm vi cô lập.

#### SAM là gì?

[Serverless Application Model (SAM)](https://aws.amazon.com/serverless/sam/) là một open-source framework nó sẽ giúp việc triển khai serverless application trở nên dễ dàng hơn. Nó cung cấp một cách đơn giản để định nghĩa các serverless application, và cung cấp một tập các công cụ để triển khai các ứng dụng đó.

Nó cho phép chúng ta xác định rõ những yêu cầu của ứng dụng bằng code. SAM chuyển đổi và mở rộng SAM syntax lên AWS Cloudformation để triển khai ứng dụng của bạn. Bạn sẽ thấy và sử dụng SAM templates throughout this workshop.

#### SAM hoạt động như thế nào?

AWS SAM dựa trên AWS Cloudformation. Một ứng dụng serverless được định nghĩa bằng một CloudFormation template và được triển khai với CloudFormation stack. Tóm lại, AWS SAM template là CloudFormation template.

AWS SAM xác định một tập hợp tài nguyên mô tả các components chung của ứng dụng serverless. Để AWS SAM có những objects được định nghĩa trong CloudFormation template thì template đó phải gồm phần Transform trong document root là `AWS::Serverless-2016-10-31`.

#### Trong ứng dụng của chúng ta: Tasks API

Tasks API mà chúng ta sẽ xây dựng trong workshop này bao gồm Amazon API Gateway HTTP endpoints để trigger AWS Lambda functions, nó sẽ đọc và ghi dữ liệu đến cơ sở dữ liệu Amazon DynamoDB. SAM template của Tasks API sẽ gồm một bảng DynamoDB, Lambda functions để liệt kê, xem và cập nhật Tasks trong bảng.

Trong phần này, bạn sẽ làm việc với Lambda function để tạo một task mới. Các thành phần của Tasks API sẽ được định nghĩa trong `template.yaml`. Tiếp theo chúng ta sẽ đi vào chi tiết cấu trúc của Lambda function.

#### Phân tích SAM template

Vào thư mục **serverless-tasks-web** tiếp đến là thư mục **sam** sau đó mở file **template.yaml**. Bên dưới là một đoạn code trong file SAM template dùng để liệt kê danh sách các tasks:

![VPC](/images/3.serverlessbackend/3-1.png)

Đây là properties được định nghĩa trong resource `AWS:Serverless:Function`, chúng ta sẽ bắt đầu tìm hiểu chúng.

**FunctionName**\
Thuộc tính **FunctionName** được đặt tên tùy ý cho Lambda function. Nếu chúng ta không đặt tên cho nó, CloudFormation sẽ sử dụng tên của CloudFormation Stack, CloudFormation Resource và random ID.

**CodeUri**\
Thuộc tính **CodeUri** chỉ định đường dẫn đến mã nguồn của Lambda function trong workspace liên kết với SAM template trong đường dẫn `sam/`. Trong ví dụ này, Đoạn code sẽ nằm ở đường dẫn `src/handlers/` và `getTasks` sẽ là function của nó.

**Handler**\
Thuộc tính **Handler** chỉ định entry point cho Lambda function. Đối với Javascript, nó sẽ được format dưới dạng **file.function**, trong đó file là tên file chứa function ko có đuôi là **.js** thuộc đường dẫn **CodeUri** được định nghĩa phía trên và **function** là tên của function trong file đó sẽ được thực thi khi Lambda function được gọi.

**Environment**\
Thuộc tính **Environment** định nghĩa biến môi trường, nó sẽ có sẵn trong Lambda function. Trong ví dụ này, chúng ta đang định nghĩa một biến môi trường **TASKS_TABLE** với giá trị là **TasksTable**. Biến môi trường này sẽ được sử dụng trong Lambda function để truy cập vào DynamoDB table nơi lưu trữ các tasks.

**Events**\
Thuộc tính **Events** định nghĩa các events sẽ trigger Lambda function khi được gọi. [Event API](https://github.com/aws/serverless-application-model/blob/master/versions/2016-10-31.md#api) được tích hợp với Lambda function cùng API Gateway endpoint, tuy nhiên SAM chỉ hỗ trợ Lambda function triggers từ [những nguồn sau](https://github.com/aws/serverless-application-model/blob/master/versions/2016-10-31.md#event-source-types).

**API event** được dùng để xem chi tiết của một Task được định nghĩa ở resource RESTful `/tasks` và được truy cập khi sử dụng phương pháp HTTP GET. SAM sẽ chuyển đổi API event đến API Gateways để gọi Lambda function.

### Nội dung

- [Khai báo Table trong DynamoDB](3.1-dynamodb/)
- [Tạo Lambda function](3.2-lambdafunction/)
