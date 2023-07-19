---
title: "Cấu hình API Gateway endpoint"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 5.2 </b> "
---

Sau khi chạy câu lệnh `sam deploy --guided` ở bước trước đó, bạn sẽ nhận được URL của API Gateway endpoint.

Ứng dụng web yêu cầu cấu hình cho endpoint này để nó có thể biết được nơi để gửi các request.

Coppy URL của API Gateway endpoint của ứng dụng chúng ta, vào thư mục `/webapp/src`, mở file **main.js.** Paste URL đó vào giá trị của `axios.defaults.baseURL` như hình.

{{% notice warning %}}
Hãy lưu ý rằng bạn đã coppy bao gồm tên của stage **/v1** trong URL đó và xóa đi dấu **/** ở cuối URL.
{{% /notice %}}

![VPC](/images/5.deploy/5.2-apigateway/5.2-1new.png)

Dòng `Vue.config.productionTip = false` sẽ tắt các tip được hiển thị trong Vue console, nó sẽ tối ưu hóa trong môi trường production.

Tiếp theo, chúng ta sẽ set mặc định URL cho tất cả Axios requests. Nó sẽ là endpoint URL nơi mà API request được gửi đến.

Tổng quát, đoạn code phía trên khởi tạo ứng dụng Vue.js bằng cách tạo một Vue instance, cấu hình base URL cho Axios requests. Nó cũng tắt các tip được hiển thị trong Vue console.
