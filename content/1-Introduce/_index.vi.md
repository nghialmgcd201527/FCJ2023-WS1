---
title : "Giới thiệu"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---
Mô hình kiến trúc của ứng dụng **Todo app** sử dụng **AWS Lamba, Amazon API Gateway, Amazon DynamoDB, Amazon Simple Storage Service (S3) và AWS Amplify Console.** Amplify Console được dùng để phục vụ cho việc triển khai liên tục (Continuos Deployment - CD) và quản lí tài nguyên cho một trang web tĩnh bao gồm HTML, CSS, JavaScript và file ảnh được tải lên từ người dùng trên trình duyệt. Trong đó, JavaScript thực thi việc gửi và nhận dữ liệu từ một public backend API được xây dựng bởi Lambda và API Gateway. DynamoDB cung cấp persistence layer nơi mà dữ liệu được lưu trữ bằng Lambda function từ API. S3 được sử dụng để lưu trữ các file image được tải lên. Và cuối cùng là Amazon Rekognition, được dùng để nhận diện những thuộc tính và gắn các thuộc tính đó vào những file image trên.

![VPC](/images/arc-01.png)

