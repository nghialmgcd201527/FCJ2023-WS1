---
title: "Add Amplify hosting"
date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: " <b> 5.5 </b> "
---

You've successfully built your first app with Amplify! Now that you've built something, it's time to deploy it to the web with Amplify Console!

All of your static web content including HTML, CSS, JavaScript, images and other files will be managed by AWS Amplify Console. Your end users will then access your site using the public website URL exposed by AWS Amplify Console. You don't need to run any web servers or use other services in order to make your site available.

{{% notice tip %}}
For most real applications you'll want to use a custom domain to host your site. If you're interested in using your own domain, [hãy theo dõi cách thiết lập custom domain trên Amplify](https://docs.aws.amazon.com/amplify/latest/userguide/custom-domains.html)
{{% /notice %}}

Run the command below to add Amplify Hosting to your web application:

```
amplify add hosting

```

When it asks you to choose the plugin to execute, select **Hosting with Amplify Console**. Next, choose the deployment method, you choose **Manual deployment.** So we have added Amplify Hosting to our web application, please refer to the image below.

![VPC](/images/5.deploy/5.5-amplifyhosting/5.5-1.png)

In the next step, we will publish your app to the web.
