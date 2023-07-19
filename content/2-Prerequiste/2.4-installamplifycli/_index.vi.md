---
title: "Cài đặt Amplify CLI"
date: "`r Sys.Date()`"
weight: 4
chapter: false
pre: " <b> 2.4 </b> "
---

Theo AWS, Amplify là một dịch vụ được thiết kế để giúp các nhà phát triển xây dựng các ứng dụng web và mobile nhanh hơn. Amplify cung cấp một tập hợp các công cụ và dịch vụ để xây dựng các ứng dụng web và mobile. Amplify CLI là một phần của Amplify Framework, nó cung cấp các lệnh để tạo và quản lý các tài nguyên AWS như Amazon Cognito, Amazon API Gateway, Amazon Lambda function, Amazon DynamoDB table, và Amazon S3 bucket.

![VPC](/images/2.prerequisite/2.4-amplifycli/amplifycli-1.gif)

Trong workshop này, chúng ta sẽ chỉ cấu hình [AWS Amplify Hosting](https://aws.amazon.com/amplify/hosting/), nó cung cấp một dịch vụ có thể quản lí dễ dàng cho việc triển khai các ứng dụng web tĩnh trên toàn cầu. Nó sẽ sử dụng Amazon S3 và Amazon CloudFornt để lưu trữ và phân phối các tài nguyên tĩnh của ứng dụng đó.

1. Cài đặt phiên bản mới nhất của Amplify CLI bằng cách chạy command sau:

```
npm i -g @aws-amplify/cli

```

Hoàn tất cài đặt Amplify CLI.

![VPC](/images/2.prerequisite/2.4-amplifycli/2.4-1.png)

2. Sau khi tải xong, hãy cấu hình cho Amplify CLI:

```
amplify configure
```

Chúng ta sẽ nhận được một đường dẫn **https://console.aws.amazon.com/** để đăng nhập vào AWS console.

![VPC](/images/2.prerequisite/2.4-amplifycli/2.4-2.png)

Sau khi đăng nhập, nhấn **Enter** và chọn **Region** mà bạn đang làm workshop, với mình là **ap-southeast-1.** Sau đó nhấn **Enter** để tiếp tục, chúng ta sẽ nhận được một đường dẫn **https://console.aws.amazon.com/iamv2/home#/users/create** để tạo một user IAM mới.

![VPC](/images/2.prerequisite/2.4-amplifycli/2.4-3new.png)

Truy cập vào đường dẫn trên, đặt tên **amplify-user** cho user đó, chọn **Next.**

![VPC](/images/2.prerequisite/2.4-amplifycli/2.4-4new.png)

{{% notice note  %}}
Mặc định, chúng ta sẽ gán quyền **AdministratorAccess** cho user được cấu hình cho Amplify CLI
{{% /notice %}}

Ở section **Permmissions options**, chọn **Attach policies directly.** Tìm và chọn **AdministratorAccess**. Sau đó nhấn **Next.** Ở trang **Review and create,** chọn **Create user.**

![VPC](/images/2.prerequisite/2.4-amplifycli/2.4-5new.png)

Tiếp theo, chọn user vừa được tạo **amplify-user,** chọn tab **Security credentials,** ở mục **Access keys,** click vào **Create access key.**

![VPC](/images/2.prerequisite/2.4-amplifycli/2.4-6new.png)

Ở bước **Access key best practices & alternatives,** chọn **Command Line Interface (CLI)** và tick vào mục **Confirm** sau đó chọn **Next.**

![VPC](/images/2.prerequisite/2.4-amplifycli/2.4-7.png)

Ở trang **Set description tag,** hãy nhập mô tả cho **Access key** tùy theo ý của bạn, mình để mặc định và chọn **Create access key.** Vậy là bạn đã tạo thành công **Access key** cho user **amplify-user,** lưu lại **Access key** và **Secret access key.** Quay trở lại terminal ở Cloud9, nhấn **Enter,** nó sẽ yêu cầu mình nhập **Access key** và **Secret access key** vừa tạo. Sau khi nhập xong, nhấn **Enter,** ở mục **Profile name,** mình sẽ để default, nhấn tiếp **Enter,** vậy là đã cấu hình thành công Amplify CLI.

![VPC](/images/2.prerequisite/2.4-amplifycli/2.4-8new.png)

Bây giờ chúng ta đã sẵn sàng để sử dụng Amplify!
