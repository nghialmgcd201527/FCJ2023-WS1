---
title: "Tạo Cloud9 Workspace"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 2.1 </b> "
---

#### Khởi tạo Cloud9

1. Nhập "Cloud9" ở thanh tìm kiếm service trên AWS Console sau đó chọn "Cloud9"

Tổng quan kiến trúc sau khi các bạn hoàn tất bước này sẽ như sau:

![VPC](/images/arc-01.png)

Để tìm hiểu cách tạo các EC2 instance và VPC với public/private subnet các bạn có thể tham khảo bài lab :

- [Giới thiệu về Amazon EC2](https://000004.awsstudygroup.com/vi/)
- [Làm việc với Amazon VPC](https://000003.awsstudygroup.com/vi/)

### Nội dung

- [Tạo VPC](2.1.1-createvpc/)
- [Tạo Public subnet](2.1.2-createpublicsubnet/)
- [Tạo Private subnet](2.1.3-createprivatesubnet/)
- [Tạo security group](2.1.4-createsecgroup/)
- [Tạo máy chủ Linux public](2.1.5-createec2linux/)
- [Tạo máy chủ Windows private](2.1.6-createec2windows/)
