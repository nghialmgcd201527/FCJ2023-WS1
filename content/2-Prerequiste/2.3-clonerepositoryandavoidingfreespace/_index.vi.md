---
title: "Clone Git repository và vấn đề của dung lượng trống Cloud9"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 2.3 </b> "
---

Ở trang terminal trên Cloud9, chạy command sau để để clone Git repository **serverless-tasks-webapp**:

```
git clone https://github.com/aws-samples/serverless-tasks-webapp

```

Sau khi chạy câu lệnh trên, chúng ta sẽ được như hình. Chúng ta sẽ thấy thư mục **serverless-tasks-webapp** được tạo ra.

![VPC](/images/2.prerequisite/2.3-clonerepo/clonerepo-1.png)

#### Tránh những vấn đề liên quan đến dung lượng trống của Cloud9

{{% notice info %}}
Mặc định, dung lượng trống của một Cloud9 instance chỉ tầm khoảng 2GB. Dùng đoạn script dưới đây để tránh tình trạng hết dung lượng và vấn đề trong suốt buổi workshop.
{{% /notice %}}

Đầu tiên, chúng ta sẽ cập nhật phiên bản mới nhất của AWS CLI:

```
pip install --user --upgrade awscli aws-sam-cli

```

Nếu cập nhật thành công, chúng ta sẽ thấy như hình bên dưới:

![VPC](/images/2.prerequisite/2.3-clonerepo/clonerepo-2.png)

Chuyển đến thư mục **serverless-tasks-webapp,** kiểm tra dung lượng của volume hiện tại bằng lệnh sau:

```
cd serverless-tasks-webapp
df -h

```

Chúng ta sẽ nhận được kết quả như hình bên dưới:

![VPC](/images/2.prerequisite/2.3-clonerepo/2.3-3.png)

filesystem ở đường dẫn /dev/nvme0n1p1 là volume mà chúng ta đang sử dụng. Chúng ta sẽ thấy dung lượng trống của volume này là 3.5G. Để tránh tình trạng hết dung lượng trong suốt buổi workshop, chúng ta sẽ tăng dung lượng của volume này lên 20G.

Tăng kích thước volume của Amazon EBS lên 20G:

```
bash resize.sh 20

```

Chúng ta sẽ thấy output như này:

![VPC](/images/2.prerequisite/2.3-clonerepo/clonerepo-4.png)

Bây giờ hãy kiểm tra lại dung lượng của volume hiện tại bằng lệnh sau:

```
df -h

```

![VPC](/images/2.prerequisite/2.3-clonerepo/2.3-5.png)

Như chúng ta thấy ở đường dẫn /dev/nvme0n1p1, dung lượng trống của volume hiện tại đã tăng lên 14G.
