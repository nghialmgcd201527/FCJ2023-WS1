---
title: "Ensure Node.js Version"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 2.2 </b> "
---

Cloud9 đã cài đặt sẵn Node.js nhưng có thể nó sẽ không tương thích với thời điểm mình làm bài workshop này. Để đảm bảo được điều này, các bạn hãy cài đặt Node.js v16 (codename **Gallium**) bằng cách chạy lệnh sau trong terminal của Cloud9.

Chúng ta hãy kiểm tra phiên bản Node.js hiện tại đang chạy trên Cloud9 bằng lệnh sau:

```
node --version

```

Đây là phiên bản Node.js mà mình đang sử dụng trong workshop này.

![VPC](/images/2.prerequisite/2.2-ensurenodejsversion/ensurenodejs-1.png)

Nếu phiên bản Node.js mà bạn đang sử dụng là v16.x thì bạn có thể bỏ qua bước này và đến phần **Clone Git repository.** Nếu phiên bản mà bạn đang sử dụng khác phiên bản trên thì hãy theo dõi các bước sau để cài đặt phiên bản Node.js phù hợp với workshop này bằng trang terminal vừa tạo.

Ở trang terminal trên Cloud9, hãy chạy lệnh sau để đảm bảo rằng bạn đã cài đặt phiên bản mới nhất của [Node.js Version Manager (nvm)](https://github.com/nvm-sh/nvm) (ở thời điểm mình viết workshop, đang là version 0.39.0).

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.2/install.sh | bash

```

Tiếp theo chúng ta sẽ cài đặt Node.js v16 "Gallium":

```
nvm install 'lts/gallium'

```

Cuối cùng, chúng ta sẽ chọn phiên bản Node.js trên là phiên bản mặc định:

```
nvm alias default 'lts/gallium'

```

Hãy xem lại phiên bản Node.js đang chạy trên Cloud9 bằng lệnh sau:

```
node --version

```

Kết quả chúng ta cần là bắt đầu bằng `v16`.
