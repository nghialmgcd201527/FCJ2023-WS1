---
title: "Cấu hình cho API authorization: API Gateway"
date: "`r Sys.Date()`"
weight: 4
chapter: false
pre: " <b> 4. </b> "
---

#### Lambda Authorizer là gì?

Lambda authorizer là một tính năng của API Gateway mà nó sử dụng Lambda function để kiểm soát truy cập tới API của bạn. Đó là một cách để bổ sung bảo mật cho API của bạn.

#### Lambda Authorizer hoạt động như thế nào?

Khi client gửi request đến một trong những chức năng của API thì API Gateway sẽ gọi đến Lambda authorizer để xác thực client và trả về IAM policy để kiểm tra client đó có được quyền truy cập hay không.

API Gateway hỗ trợ các tiêu chuẩn mở cho các cách xác thực như OAuth và SAML.

Thông thường, intentity provider được sử dụng để xác thực người gửi request đến API. Identity provider phải chịu trách nhiệm xác minh và trả về danh tính của người đó. Sau đó, nó sẽ được chuyến đền Lambda authorizer, nơi chịu trách nhiệm kiểm soát quyền truy cập API của bạn.

Trong workshop này, chúng ta sẽ không triển khai identity provided, vì vậy, thay vào đó, chúng ta sẽ mock identity provider.

#### JSON Web Token (JWT)

Danh tính của người gọi API sẽ được biểu diễn bằng một [JSON Web Token (JWT)](https://en.wikipedia.org/wiki/JSON_Web_Token). Theo wikipedia, "A JWT is a compact, self-contained, signed JSON object. The JWT is typically used to represent an authenticated user's identity."

##### **Example JWT**

Một JWT token bao gồm 3 phần: header, payload và signature. Phần header và payload là những đối tượng dạng JSON. Phần signature là một chuỗi kí tự được sử dụng để xác thực phần header và payload.

##### **Header:**

Loại token (`typ`) là **JWT.** Thuật toán (`alg`) là **HS256**, là sự kết hợp giữa [HMAC](https://en.wikipedia.org/wiki/HMAC) cùng với [SHA-256](https://en.wikipedia.org/wiki/SHA-2)

```
{
  "typ": "JWT",
  "alg": "HS256"
}
```

##### **Payload:**

Thuộc tính **iss** là tên của identity provider. Thuộc tính **sub** là tên của người gửi request. Thuộc tính **scope** là quyền của người gửi request. Thuộc tính **jti** là unique identifier cho JWT. Thuộc tính **iat** là thời gian mà token được tạo ra. Thuộc tính **exp** là thời gian mà token hết hạn.

```
{
  "iss": "getting-started-with-serverless",
  "sub": "minhnghia",
  "scope": "admins",
  "jti": "02343566e-8ff3-4a2d-ac18-c4d28ed96881",
  "iat": 1633433217,
  "exp": 4070208800
}
```

##### **Signature:**

```
R2D2RfY_n7OAcuONbpPfxqBY5IppEiGLCCfeQ_wz_2w
```

Các giá trị này đều được mã hóa dạng base64 và nối với form của JWT bằng dấu ".", ví dụ như sau:

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJsb2dnZWRJbkFzIjoiYWRtaW4iLCJpYXQiOjE0MjI3Nzk2Mzh9.gzSraSYS8EXBxLN_oWnFSRgCzcmJmMjLiuyu5CSpyHI
```

Bạn sẽ thấy token được gửi đến API Gateway endpoint bằng ứng dụng web và sử dụng HTTP **Authorization** header.

#### Cấu hình Lambda Authorizer

Coppy đoạn code dưới đây và paste vào file **app.js** trong thư mục `sam/src/auth`.

```
const jwt = require('njwt')

exports.handler = function (event, context, callback) {
  console.info('received:', event)
  const token = event.authorizationToken.split(' ')[1]
  jwt.verify(token, 'secretphrase', (err, verifiedJwt) => {
    if (err) {
      console.log(err.message)
      callback('Error: Invalid token')
    } else {
      console.log(`Verified token: ${verifiedJwt}`)
      const resource = `${event.methodArn.split('/', 2).join('/')}/*`
      const policy = generatePolicy(verifiedJwt.body.sub, 'Allow', resource)
      console.log(`Generated policy: ${JSON.stringify(policy)}`)
      callback(null, policy)
    }
  })
}

const generatePolicy = function (principalId, effect, resource) {
  const authResponse = {}

  authResponse.principalId = principalId
  if (effect && resource) {
    const policyDocument = {}
    policyDocument.Version = '2012-10-17'
    policyDocument.Statement = []
    const statementOne = {}
    statementOne.Action = 'execute-api:Invoke'
    statementOne.Effect = effect
    statementOne.Resource = resource
    policyDocument.Statement[0] = statementOne
    authResponse.policyDocument = policyDocument
  }

  authResponse.context = {
    userId: 1,
    createdAt: new Date().toISOString()
  }
  return authResponse
}

```
