---
title: "Deploy ứng dụng"
date: "`r Sys.Date()`"
weight: 6
chapter: false
pre: " <b> 5.6 </b> "
---

Trong bước này, chúng ta sẽ publish ứng dụng với amplify bằng câu lệnh sau:

```
amplify publish

```

Và nếu các bạn nhận được output giống như hình, các bạn đã deploy thành công ứng dụng của mình lên web.

![VPC](/images/5.deploy/5.6-deployapp/5.6-1.png)

Sau khi publish, ở cuối trang terminal sẽ hiển thị URL của app chúng ta với domain **amplifyapp.com**

Bạn có thể đăng nhập với bất kì username và password nào mà bạn muốn bởi vì chưa có phần xác thực ở phía backend.

![VPC](/images/5.deploy/5.6-deployapp/5.6-2.png)

{{% notice tip %}}

Nếu bạn muốn tìm hiểu cách thêm xác thực vào ứng dụng của mình, [hãy truy cập tại đây](https://docs.amplify.aws/lib/auth/getting-started/q/platform/js/)

{{%/notice%}}

Trang chính của ứng dụng chúng ta sẽ trông như thế này.

![VPC](/images/5.deploy/5.6-deployapp/5.6-3.png)

Bất cứ khi nào chúng ta muốn thay đổi gì ở ứng dụng và publish nó, chỉ cần chạy lại câu lệnh `amplify publish`. Chúng ta sẽ không cần modify ứng dụng web, chỉ cần modify ở SAM template và Lambda functions.

Để quản lí ứng dụng và cấu hình hosting ở Amplify Console, hãy chạy câu lệnh `amplify console`.
