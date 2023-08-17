---
title: "Configure API authorization: API Gateway"
date: "`r Sys.Date()`"
weight: 4
chapter: false
pre: " <b> 4. </b> "
---

#### What is Lambda Authorizer?

A Lambda authorizer is an API Gateway feature that uses a Lambda function to control access to your API. It is a way to add additional security to your API.

#### How does a Lambda Authorizer work?

When a client makes a request to one of your API's methods, API Gateway calls your Lambda authorizer, which takes the caller's identity as input and returns an [IAM policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) as output.

API Gateway supports open standards for authentication strategies such as [OAuth](https://en.wikipedia.org/wiki/OAuth) and [SAML](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language).

Typically, an identity provider is used to authenticate the caller. The identity provider is responsible for verifying the caller's identity and returning the caller's identity. The caller's identity is then passed to the Lambda authorizer, which is responsible for controlling access to your API.

In this workshop, we won't be implementing the the identity provider needed to vend security tokens, so we will instead be mocking the identity provider. More on this later.

#### JSON Web Token (JWT)

The caller's identity is represented by a [JSON Web Token (JWT)](https://en.wikipedia.org/wiki/JSON_Web_Token). A JWT is a compact, self-contained, signed JSON object. The JWT is typically used to represent an authenticated user's identity.

##### **Example JWT**

A JWT token consists of three parts: a header, a payload, and a signature. The header and payload are JSON objects. The signature is a digital signature that is used to verify the header and payload.

##### **Header:**

The type (`typ`) of token is **JWT.** The algorithm (`alg`) is **HS256**, which is [HMAC](https://en.wikipedia.org/wiki/HMAC) with [SHA-256](https://en.wikipedia.org/wiki/SHA-2).

```
{
  "typ": "JWT",
  "alg": "HS256"
}
```

##### **Payload:**

The **iss** attribute stores the identity provider's name. the **sub** attribute stores the caller's identity. The **scope** attribute stores the caller's permissions. The **jti** attribute is a unique identifier for the JWT. The **iat** attribute stores the time at which the JWT was issued. The **exp** attribute stores the time at which the JWT expires.

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

You will see this token passed by the web application to the API Gateway endpoint using an HTTP **Authorization** header.

#### Configure a Lambda Authorizer

Copy the code below and paste it into the **app.js** file in the `sam/src/auth` directory.

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

The code above will verify and authenticate the requests sent to the API using JSON Web Token (JWT) and return an IAM policy for that authenticated person.

Starting off, we'll import the **njwt** library, which is used for JWT authentication.

**exports.handler** is the entry point of the Lambda function. It will accept an event, a context and a callback. Event contains information about the request sent to the API. Context contains information about the Lambda function. The callback will be called when the Lambda function completes.

The JWT token is extracted from the authorizationToken in the request header. There will be a split method used to remove the **Bearer** prefix and retrieve the actual token.

The `jwt.verify` function is used to verify the token using **secretphrase**. If the token is invalid, the Lambda function will return an error. If the token is valid, the authenticated JWT object is retrieved and disposed of.

The **resource** variable is set to represent the resources requested from the API request.

The `generatePolicy` function is used to generate an IAM policy. It will take the value of **sub (subject)** from the body of the JWT, convert **effect** to **Allow** and use the **resource** variable to clarify that policy.

In summary, the code above is a simple token-based authentication method, it is used to describe how to use the token to authenticate the request. The token is passed using the header of **Authorization.** Then we will verify the token using (secretphrase). It's a very common passphrase, applied in web application. We are simply mock identity provider. In practice, we will use a reputable identity provider, such as Amazon Cognito, to verify the user's identity.

#### IAM Policy Document

This is an example IAM policy document that is returned to the caller. Here, principal **minhnghia** is granted access to the API Gateway method.

```
{
    "principalId": "minhnghia",
    "policyDocument": {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Action": "execute-api:Invoke",
                "Effect": "Allow",
                "Resource": "arn:aws:execute-api:us-east-1:0123456789012:dn3fidb5ng0/v1/*"
            }
        ]
    },
    "context": {
        "userId": 1,
        "createdAt": "2021-10-01T19:53:34.594Z"
    }
}

```

**principalId** represents the indentify of the user assigned with the request. In this code, it is assigned the value **minhnghia**, this IAM policy is assigned to this user with the same identifier as **minhnghia**.

object **policyDocument** defines the permissions assigned in the policy. **Version** is the version of the currently used policy language `2012-10-17`. **Statement** is an array of permissions assigned to the user. In this code, it has only one permission, **execute-api:Invoke**. **Effect** is either **Allow** or **Deny**. **Resource** is a unique identifier for a particular resource. In this code, it is an identifier for an API Gateway endpoint.

**context** is an object containing additional information about the user. It can be used to pass additional information about the user to the Lambda function. In this code, it contains two pieces of information, **userId** and **createdAt**. This is beneficial in debugging and checking the requests of that user.
