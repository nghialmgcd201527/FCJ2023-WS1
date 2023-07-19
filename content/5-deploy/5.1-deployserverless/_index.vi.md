---
title: "Deploy Serverless Application"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 5.1 </b> "
---

Trong bước này, chúng ta sẽ sử dụng SAM CLI để build và deploy ứng dụng serverless.

Đầu tiên, chúng ta cần chắc chắn rằng đã cập nhật phiên bản mới nhất của SAM CLI.

```
pip install --user --upgrade aws-sam-cli

```

Chúng ta sẽ thấy output như hình.
![VPC](/images/5.deploy/5.1-serverless/5.1-1.png)

Tiếp theo, đi đến thư mục **sam:**

```
cd ~/environment/serverless-tasks-webapp/sam

```

Chạy câu lệnh dưới đây để build ứng dụng:

```
sam build
```

Khi build thành công, chúng ta sẽ có như hình.

![VPC](/images/5.deploy/5.1-serverless/5.1-2.png)

Câu lệnh `sam build` xử lí file template AWS SAM, code của ứng dụng và những dependencies. Nó cũng coppy build artifacts theo một format và ví trị được xác định sẵn trong workflow của bạn.

Bây giờ, chúng ta sẽ deploy ứng dụng:

```
sam deploy --guided

```

Câu lệnh này sẽ đóng gói và deploy ứng dụng lên account AWS của chúng ta.

Đầu tiên, nó sẽ hỏi chúng ta tên stack, chúng ta có thể đặt tên bất kỳ, ở đây chúng ta đặt là **tasks-app.** Sau đó sẽ chọn region mà chúng ta muốn deploy ứng dụng lên, ở đây chúng ta chọn **ap-southeast-1**.

![VPC](/images/5.deploy/5.1-serverless/5.1-3.png)

Tiếp theo, chúng ta sẽ nhập **y** và nhấn **Enter** để confirm deploy.

Để cho phép SAM có thể tạo roles để kết nối tới các resources trong template của chúng ta, hãy cho phép SAM CLI tạo ra IAM roles, chúng ta nhập **Y** và nhấn **Enter.**

Vô hiệu hóa rollback để bảo vệ những resources của chúng ta khi quá trình khiển khai thất bại, chúng ta nhập **y** và nhấn **Enter.**

Cho phép lưu các arguments vào file cấu hình, chúng ta nhập **Y** và nhấn **Enter.**

Vì file SAM configuration chưa có nên chúng ta tạo ra nó, chúng ta nhấn **Enter** để mặc định, tương tự với môi trường của SAM configuration.

Kiểm tra lại thông tin trước khi deploy.

![VPC](/images/5.deploy/5.1-serverless/5.1-4.png)

Nếu deploy thành công, chúng ta sẽ thấy output như hình.

![VPC](/images/5.deploy/5.1-serverless/5.1-5new.png)

{{% notice note %}}
Hãy ghi chú và lưu lại giá trị của key **TasksApi** và **S3BucketName,** nó sẽ cần cho những bước tiếp theo.
{{% /notice %}}
