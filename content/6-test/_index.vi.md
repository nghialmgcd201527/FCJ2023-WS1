---
title: "Chạy thử ứng dụng"
date: "`r Sys.Date()`"
weight: 6
chapter: false
pre: " <b> 6. </b> "
---

![VPC](/images/6.test/6-1.png)

Sau khi đăng nhập với bất kì username hay password nào, bạn có thể tạo task mới bằng cách nhập **Title** và **Body**. Phần **Due date** có thể để trống.

Nhấn **Create** để tạo task mới, nó sẽ được hiển trị trong phần **My Tasks.**

Như các bạn sẽ thấy ở hình bên dưới.

![VPC](/images/6.test/6-2.png)

Khi bạn click vào nút **Create task,** sẽ có những thứ hoạt động:

1. Request với phương thức **POST** được gửi đến API Gateway endpoint **/tasks**.
2. Request đó được xác thực bởi Lambda authorizer, sử dụng HTTP header `Authentication: Bearer <token>`.
3. Body của request đó được chuyển đến Lambda function **CreateTaskFunction.**
4. Những thông tin của task được nhập ở trên sẽ được lưu trữ ở DynamoDB table.
5. Sẽ có phản hồi với HTTP status code 200 được trả về ứng dụng web.
6. Ứng dụng web refresh lại danh sách các tasks bởi vì request với phương thức **GET** được gửi đến API Gateway endpoint **/tasks.**
7. Lại lần nữa, request đó được xác thực bởi Lambda authorizer và chuyển đến Lambda function **GetTasksFunction.**
8. Lambda function **GetTasksFunction** truy xuất danh sách các task từ DynamoDB table và trả về danh sách các task đó cho người dùng đã được xác thực.

Hãy tạo thêm nhiều tasks mới và thử xóa một task để hiểu rõ hơn về quy trình request được xử lí.

### Nội dung

- [Quản lí dữ liệu trong DynamoDB](6.1-dynamodb/)
- [Logging và Monitoring](6.2-logandmonitor/)
