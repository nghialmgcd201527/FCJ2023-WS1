---
title: "Quản lí dữ liệu trong DynamoDB"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 6.1 </b> "
---

1. Nhập **DynamoDB** ở thanh tìm kiếm service trên AWS Console sau đó chọn **DynamoDB**.

![VPC](/images/6.test/6.1-dynamodb/6.1-1new.png)

2. Ở thanh điều hướng bên trái, chọn **Tables.**
3. Click vào table có tên bao gồm tên của ứng của chúng ta **tasks-app-TasksTable.** Đây là table được tạo bởi SAM template.

![VPC](/images/6.test/6.1-dynamodb/6.1-2new.png)

4. Click vào nút **Explore table items.**

![VPC](/images/6.test/6.1-dynamodb/6.1-3new.png)

Chúng ta sẽ thấy dữ liệu ở trong table ở phần **Items returned.**

![VPC](/images/6.test/6.1-dynamodb/6.1-4new.png)

Click vào partition key (**user**) của bảng đó sẽ đưa bạn đến nơi chỉnh sửa thông tin của item đó.

![VPC](/images/6.test/6.1-dynamodb/6.1-5.png)

Khi bạn thay đổi dữ liệu ở DynamoDB, chúng ta cần refresh lại trang web của ứng dụng để thấy được sự thay đổi.
