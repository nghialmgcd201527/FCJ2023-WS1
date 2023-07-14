---
title: "Các bước chuẩn bị"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

{{% notice warning %}}
Hãy nhớ rằng Cloud9 workspace chỉ nên được tạo ra bởi IAM user (hoặc là được gán vào một IAM role thích hợp) với đặc quyền của Admin, không được dùng root user.
{{% /notice %}}

#### **Cloud9 Workspace**

Chúng ta thường dùng môi trường phát triển tích hợp (Integrated Development Environment - IDE) ở local, trong bài workshop này, chúng ta sẽ dùng Cloud9. Nó là một IDE chạy trên cloud sử dụng trình duyệt, bao gồm những tính năng quan trọng, thiết yếu ở local IDE mà chúng ta thường dùng như viết, chạy, debug code. Cloud9 đã được trang bị sẵn những gói tệp tin như JavaScript, Python, NodeJS và những thứ khác [ở đây](https://aws.amazon.com/cloud9/)

> Để các dịch vụ của AWS phản hồi nhanh hơn, hãy chọn Region gần nhất trong suốt buổi workshop.

#### **Các vùng hỗ trợ service Amazon Rekognition**

Hãy lưu ý rằng không phải tất cả các service của AWS luôn có sẵn tại tất cả các vùng. Trong bài workshop này, chúng ta sẽ sử dụng service Amazon Rekognition, nó sẽ chỉ phục vụ cho các vùng dưới đây:

- US East (Ohio) us-east-2
- US East (N. Virginia) us-east-1
- US West (N. California) us-west-1
- US West (Oregon) us-west-2
- Asia Pacific (Mumbai) ap-south-1
- Asia Pacific (Seoul) ap-northeast-2
- Asia Pacific (Singapore) ap-southeast-1
- Asia Pacific (Sydney) ap-southeast-2
- Asia Pacific (Tokyo) ap-northeast-1
- Canada (Central) ca-central-1
- Europe (Frankfurt) eu-central-1
- Europe (Ireland) eu-west-1
- Europe (London) eu-west-2

Xem thêm cập nhật mới nhất về [Amazon Rekognition](https://docs.aws.amazon.com/general/latest/gr/rekognition.html "Amazon Rekognition")

### Nội dung

- [Tạo Cloud9 Workspace](2.1-createcloud9workspace/)
- [Cài đặt Amplify CLI](2.2-installamplifycli/)
