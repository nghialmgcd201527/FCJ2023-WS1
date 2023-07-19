---
title: "Logging và Monitoring"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 6.2 </b> "
---

#### **Logging**

Centralized logging cho chúng ta 2 lợi ích quan trọng. Đầu tiên các log được ghi lại được lưu trữ trong một nơi duy nhất và được format theo một tiêu chuẩn nhất định, giúp chúng ta đơn giản trong việc quản lí các tasks và phân tích logs. Thứ hai, nó cung cấp cho chúng ta một nơi lưu trữ an toàn cho dữ liệu của các logs của chúng ta.

Trong AWS Lambda, service logging mặc định là Amazon CloudWatch.

Lambda tự động truyền thông tin về các lúc gọi function, cùng với các logs và output từ code của functions của chúng ta sang CloudWatch Logs.

Các nhóm log là một phần tiêu chuẩn của CloudWatch và được dùng để phân loại các logs. Bất kì các logs được tạo bởi Lambda function đều có quy ước đặt tên là `/aws/lambda/function-name`. Một nhóm log là một tập hợp các log streams, bạn có thể xem chi tiết nó trong CloudWatch console.

1. Nhập **CloudWatch** ở thanh tìm kiếm service trên AWS Console sau đó chọn **CloudWatch**.

![VPC](/images/6.test/6.2-logmonitor/6.2-1new.png)

2. Ở thanh điều hướng bên trái, chọn **Log groups.**

![VPC](/images/6.test/6.2-logmonitor/6.2-2new.png)

3. Chọn log group với function **CreateTaskFunction**.

![VPC](/images/6.test/6.2-logmonitor/6.2-3new.png)

Mỗi phiên bản của một Lambda function có một stream log riêng. Nếu một function được mở rộng thì phiên bản mới đó đồng thời có log stream riêng. Mỗi khi một environment được chọn để thực thi và một environment mới được tạo ra để đáp ứng một lượng invocation nhất định và nó sẽ tạo ra một log stream mới. Cách đặt tên cho các log streams là `YYYY/MM//DD [Function version] [Execution environment GUID]`

Ví dụ, đây là log stream của function **CreateTaskFunction**.

![VPC](/images/6.test/6.2-logmonitor/6.2-4new.png)

Trong những logs trên này có:

- **RequestId:** là một ID duy nhất được tạo ra cho mỗi lần gọi function. Nếu Lambda function gọi lại request thì ID này sẽ không thay đổi và giữ nguyên trong logs cho mỗi lần gọi lại tiếp theo.
- **Start/End:** các giá trị này đánh dấu một invocation, vì vậy những dòng log ở giữa các giá trị này đều thuộc về chung một invocation.
- **Duration:** thời gian thực thi của function.
- **Billed Duration:** phép toán làm tròn cho Duration.
- **Memory Size:** kích thước bộ nhớ được cấp phát cho function.
- **Max Memory Used:** kích thước bộ nhớ tối đa được sử dụng trong quá trình thực thi function.
- **Init Duration:** thời gian tính từ khi chạy phần **INIT.**

#### **Monitoring**

Metrics là dữ liệu số đo ở các khoảng thời gian khác nhau (time series data) và chỉ số service-level (request rate, error rate, duration, CPU, etc). Lambda tự động publish một lượng metrics cho Lambda functions.

Để theo dõi và quan sát các Lambda functions, các metrics quan trọng nhất là:

- **Errors:** Liệu rằng errors có phải do logic hoặc do runtime trong code hoặc do tương tác lỗi với Lambda service hoặc các services khác gây ra hay không. Những thứ này đều có thể do các yếu tố khác gây ra, chẳng hạn như là không được cấp quyền hoặc vượt quá resource limit.
- **Excution time:** Đo thời gian response chỉ cho chúng ta cái nhìn hạn chế về hiệu suất trong distributed applications. Điều quan trọng là nắm bắt và theo dõi hiệu suất ở các khoảng phần trăm (chẳng hạn như 95% và 99%) để đo lường hiệu suất cho ít nhất 5% và 1% requests.
- **Throttling:** Ứng dụng serverless sử dụng resource có thể mở rộng với Service Quotas để bảo đảm cho khách hàng. Throttling có thể cho chúng ta biết quotas được thiết lập không chính xác, có lỗi trong cấu trúc của hệ thống hoặc mức lưu lượng truy cập vượt quá giới hạn.

Tất cả Lambda functions tự động tích hợp với CloudWatch. Lambda tự động ghi lại nhiều metrics, nó luôn được publish lên CloudWatch metrics. Nếu bạn điều huwosng đến một function trong Lambda, phần **Monitor** của mục **Metrics** cho chúng ta xem nhanh về các CloudWatch metrics được tích hợp với một function.

Để có thể vào xem **Mornitor**, hãy vào **Lambda** console, chọn **Lambda function** mà bạn muốn xem. Vào thẻ **Monitor,** ở phần Metrics, chúng ta có thể xem các metrics của function. Xem minh họa ở hình bên dưới.

![VPC](/images/6.test/6.2-logmonitor/6.2-5new.png)

Tiếp theo, chúng ta sẽ thêm tính năng mới vào ứng dụng web của chúng ta với Amazon Rekognition.
