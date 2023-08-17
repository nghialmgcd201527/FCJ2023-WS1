---
title: "Build the web application"
date: "`r Sys.Date()`"
weight: 4
chapter: false
pre: " <b> 5.4 </b> "
---

{{% notice warning %}}
Ensure you are in the root of the web project (`/webapp`):

```
cd ~/environment/serverless-tasks-webapp/webapp

```

{{% /notice %}}

Install the dependencies:

```
npm install
```

You should see output like this:

![VPC](/images/5.deploy/5.4-buildapp/5.4-1.png)

{{% notice info %}}
You can safely ignore any `npm install` warnings about security vulnerabilities for the purposes of this workshop. In a production environment, you should work to mitigate these.
{{% /notice %}}

Build the web application:

```
npm run build
```

You should see output like this:

![VPC](/images/5.deploy/5.4-buildapp/5.4-2.png)
