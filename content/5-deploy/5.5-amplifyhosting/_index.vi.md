---
title: "Thêm Amplify Hosting vào ứng dụng"
date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: " <b> 5.5 </b> "
---

Chúng ta đã thành công build ứng dụng với Amplify! Bây giờ sau khi build xong, chúng ta sẽ deploy nó lên web bằng Amplify Console.

Tất cả static resource của ứng dụng web bao gồm HTML, CSS, JavaScript, images và những file khác sẽ được quản lí bởi AWS Amplify Console. Người dùng sẽ truy cập vào ứng dụng thông qua URL public được cấp bởi Amplify Console. Bạn không cần khởi chạy bất kì web servers hoặc những services khác để làm cho ứng dụng của bạn hoạt động.

{{% notice tip %}}
Trong thực tế, bạn sẽ sử dụng custom domain để làm chủ ứng dụng. Nếu bạn muốn sử dụng domain của bạn, [hãy theo dõi cách thiết lập custom domain trên Amplify](https://docs.aws.amazon.com/amplify/latest/userguide/custom-domains.html)
{{% /notice %}}

Chạy câu lệnh dưới đây để thêm Amplify Hosting vào ứng dụng web của bạn:

```
amplify add hosting

```

Khi nó hỏi bạn chọn plugin để thực thi, hãy chọn **Hosting with Amplify Console**. Tiếp đến, chọn phương thức deployment, các bạn hãy chọn **Manual deployment.** Vậy là chúng ta đã thêm Amplify Hosting vào ứng dụng web của mình, các bạn tham khảo hình bên dưới.

![VPC](/images/5.deploy/5.5-amplifyhosting/5.5-1.png)

Tiếp theo, chúng ta sẽ publish ứng dụng lên web.
