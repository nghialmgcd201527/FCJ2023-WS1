---
title: "Deploy the application"
date: "`r Sys.Date()`"
weight: 6
chapter: false
pre: " <b> 5.6 </b> "
---

Run the following command to publish your app.

```
amplify publish

```

And if you get the output like the picture, you have successfully deployed your application to the web.

![VPC](/images/5.deploy/5.6-deployapp/5.6-1.png)

After publishing, at the bottom of the terminal page will display the URL of our app with the domain **amplifyapp.com**

You can login with any username and password you want because there is no authentication on the backend.

![VPC](/images/5.deploy/5.6-deployapp/5.6-2.png)

{{% notice tip %}}

If you want to learn how to add authentication to your app, [go here](https://docs.amplify.aws/lib/auth/getting-started/q/platform/js/)

{{%/notice%}}

The main page of our application will look like this.

![VPC](/images/5.deploy/5.6-deployapp/5.6-3.png)

Whenever we want to change something in the application and publish it, just run the `amplify publish` command again. We won't need to modify the web application, just modify the SAM template and Lambda functions.

To manage the application and configure hosting in Amplify Console, run the command `amplify console`.
