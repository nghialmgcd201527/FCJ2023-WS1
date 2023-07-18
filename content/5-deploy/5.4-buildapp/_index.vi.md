---
title: "Build ứng dụng web"
date: "`r Sys.Date()`"
weight: 4
chapter: false
pre: " <b> 5.4 </b> "
---

{{% notice warning %}}
Đầu tiên, hãy đảm bảo rằng bạn đang ở thư mục gốc của project (`/webapp`)

```
cd ~/environment/serverless-tasks-webapp/webapp

```

{{% /notice %}}

Tiếp theo, cài đặt các dependencies cần thiết cho project bằng lệnh sau:

```
npm install
```

Chúng ta sẽ thấy như hình sau khi cài đặt xong:

![VPC](/images/5.deploy/5.4-buildapp/5.4-1.png)

{{% notice info %}}
Bỏ qua những cảnh báo về bảo mật khi chạy câu lệnh `npm install` trong phạm vi buổi workshop này. Trong môi trường production thực tế, chúng ta sẽ xử lí chúng trước đó.
{{% /notice %}}

Sau khi cài đặt xong, chúng ta sẽ build ứng dụng web bằng lệnh sau:

```
npm run build
```

Nếu build thành công, chúng ta sẽ nhận được như này:

![VPC](/images/5.deploy/5.4-buildapp/5.4-2.png)
