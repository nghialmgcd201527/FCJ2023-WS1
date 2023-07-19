---
title: "Dọn dẹp tài nguyên"
date: "`r Sys.Date()`"
weight: 8
chapter: false
pre: " <b> 8. </b> "
---

#### SAM-created Resources

Cách đơn giản nhất để xóa resources được tạo ra bởi SAM là sử dụng câu lệnh `sam delete`. Tuy nhiên, trước khi chạy câu lệnh đó, bạn cần chắn chắn rằng S3 bucket chứa hình ảnh tải lên đã được dọn dẹp sạch.

Bạn hãy sử dụng câu lệnh `aws s3 ls` để thấy được danh sách cái file trong bucket. Hãy làm như hình.

![VPC](/images/8.clean/8-1new.png)

Bạn sẽ thấy bucket tên **uploads-tasks-app-ap-southeast-1-127779471063** trong đó **ap-southeast-1** là region của bạn và **127779471063** là ID của AWS account.

Hãy đi đến thư mục **/sam** bằng câu lệnh `cd ~/environment/serverless-tasks-webapp/sam`. Bucket của workshop này được kích hoạt versioning nên bạn phải dọn dẹp nó bằng AWS SDK. Tìm đến file **emtpy_versioned_bucket.py** trong thư mục **/sam** và modify nó bằng cách chỉnh sửa tên bucket trên sau khi chạy câu lệnh `aws s3 ls`. Lưu lại file và chạy câu lệnh sau để dọn dẹp bucket:

```
python empty_versioned_bucket.py

```

Chúng ta sẽ có kết quả như hình:

![VPC](/images/8.clean/8-2.png)

Bây giờ chúng ta có thể chạy câu lệnh:

```
sam delete
```

Vậy là chúng ta đã xóa thành công các resources được tạo ra bởi SAM.

![VPC](/images/8.clean/8-3.png)

#### Amplify Hosting

Di chuyển vào thư mục **/webapp,** chúng ta có thể xóa Amplify project bằng câu lệnh:

```
cd ~/environment/serverless-tasks-webapp/webapp
amplify delete

```

Sau khi xóa thành công, chúng ta sẽ thấy như hình:

![VPC](/images/8.clean/8-4.png)

{{% notice warning %}}

Các bạn hãy nhớ vào IAM console, chọn user **amplify-user** được tạo ra ở [bước 2.4 - Cài đặt Amplify CLI](/vi/2-prerequiste/2.4-installamplifycli) và xóa nó cùng Access Key được tạo ra vì nó không được tự động xóa khi chạy câu lệnh `amplify delete`.

{{%/notice%}}

#### CloudWatch Logs

Bạn phải xóa các nhóm log trong CloudWatch Logs và streams được tạo ra khi sử dụng ứng dụng. Hãy vào **CloudWatch Logs** console, tiếp theo vào **Log groups** ở thanh điều hướng bên trái, chọn các Log group của workshop này và xóa chúng.

![VPC](/images/8.clean/8-5new.png)

Vậy là chúng ta đã dọn dẹp tất cả tài nguyên trong workshop này.
