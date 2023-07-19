---
title: "Khởi tạo Amplify"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 5.3 </b> "
---

Bước này, chúng ta sẽ set up Amplify để có thể khởi tạo những service cần thiết cho phần backend của ứng dụng. Chúng ta sẽ đi cấu hình cho Amplify hosting.

Chạy lệnh sau để vào thư mục root của web project (`/webapp`):

```
cd ~/environment/serverless-tasks-webapp/webapp
```

Chúng ta khởi tạo Amplify:

```
amplify init
```

Chúng ta sẽ đặt tên cho project là **tasks** và nhấn **Enter** để tiếp tục. Nó sẽ hiện ra những thông tin về project.

Tiếp theo, nó sẽ hỏi chúng ta có muốn khởi tạo project với những thông tin như trên hay không thì nhập **Y** và nhấn **Enter**.

Sau đó, nó sẽ hỏi chúng ta chọn cách xác thực mà chúng ta muốn, chúng ta chọn **AWS profile** và nhấn **Enter**. Và chọn profile mặc định mà chúng ta đã cấu hình ở bước trước đó.

Chúng ta sẽ làm như hình dưới đây.

![VPC](/images/5.deploy/5.3-initializeamplify/5.3-1new.png)

{{% notice tip %}}
CLI có thể tự xác định được cấu hình phù hợp với project mà Amplify được khởi tạo. Trong workshop này, CLI biết chúng ta đang sử dụng Vue.js và cung cấp cấu hình phù hợp với app type, framework, source, distribution, build và start options.
{{% /notice %}}

Khi chúng ta khởi tạo project Amplify mới, sẽ có một vài thứ thay đổi:

- Nó sẽ tạo ra một thư mục mới có tên là **amplify** trong thư mục root của project và chứa những khởi tạo của backend.
- Nó tạo ra một file tên là **aws-exports.js** trong thư mục **src** của project. File này chứa những thông tin cấu hình cần thiết cho những services mà bạn tạo ra với Amplify, đây là cách mà người dùng Amplify có thể lấy những thông tin cần thiết của những backend services.
- Những cloud project được tạo ra trong AWS Amplify Console có thể được truy cập bằng cách chạy lệnh `amplify console`. Console cung cấp danh sách các môi trường backend, những resources được thực thi trong Amplify category, trang trái hiện tại của deployments và những thông tin khác.

Quá trình khởi tạo Amplify hoàn thành, chúng ta sẽ nhận được thông báo như hình dưới đây.

![VPC](/images/5.deploy/5.3-initializeamplify/5.3-2.png)

Bây giờ, ứng dụng Vue.js đã được thiết lập và Amplify đã được khởi tạo, chúng ta đã sẵn sàng để sử dụng Amplify hosting ở bước tiếp theo.
