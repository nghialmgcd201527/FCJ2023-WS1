---
title: "Tạo Lambda function"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

#### AWS Lambda là gì?

AWS Lambda là một dịch vụ serverless nó sẽ thực thi những dòng code mà không cần phải chuẩn bị và quản lí servers. Lambda tự động phân bổ dung lượng máy tính và chạy code của bạn dựa vào những yêu cầu hoặc sự kiện được gửi đến trong tất cả các môi trường, các điều kiện.

Cách nó hoạt động:

1. Tải code của bạn lên AWS Lambda hoặc viết code trên trình chỉnh sửa của Lambda. (Trong workshop này, chúng ta sẽ viết code và tải lên bằng SAM)
2. Cài đặt code của bạn cho phép trigger từ những AWS services khác, từ HTTP endpoints, hay là những hoạt động xảy ra bên trong ứng dụng.
3. AWS Lambda sẽ thực thi code của bạn khi trigger được kích hoạt, sau đó nó sẽ tự động quản lí tài nguyên tính toán cho code của bạn.
4. Chỉ trả tiền cho thời gian thực thi code của bạn (và số lượng tài nguyên tính toán mà code của bạn sử dụng).

Đối với **Windows instance** nằm trong **private subnet**, không có **public IP**, không có **internet gateway** nên không thể đi ra ngoài **internet.**\
Với loại instance này, cách làm truyền thống là ta sẽ sử dụng kỹ thuật Bastion host tốn nhiều chi phí và công sức, nhưng ở đây chúng ta sẽ sử dụng Session Manager với loại này.\
Cơ bản là **private instance** vẫn phải mở cổng **TCP 443** tới **System Manager**, nhưng không cho kết nối đó đi ra ngoài internet mà chỉ cho đi trong chính VPC của mình, nên đảm bảo được vấn đề bảo mật.\
Để làm được điều đó, ta phải đưa endpoint của System Manager vào trong VPC, nghĩa là sử dụng **VPC interface endpoint:**

![ConnectPrivate](/images/arc-03.png)

**VPC interface endpoint** được gắn với subnet nên cách làm này không những với **private subnet** mà còn có thể làm với **public subnet**, nghĩa là với **public subnet**, bạn hoàn toàn có thể không cho **TCP 443** đi ra ngoài internet.

### Nội dung:

- [Kích hoạt DNS hostnames](./3.2.1-enablevpcdns/)
- [Tạo VPC Endpoint](./3.2.2-createvpcendpoint/)
- [Kết nối Private Instance](./3.3.3-connectec2/)
