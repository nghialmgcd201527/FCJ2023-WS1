---
title: "Configure the API Gateway endpoint"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 5.2 </b> "
---

After running the `sam deploy --guided` command in the previous step, you will get the URL of the API Gateway endpoint.

The web application requires configuration for this endpoint so that it knows where to send requests.

Copy the URL of the API Gateway endpoint of our application, go to the `/webapp/src` directory, open the file **main.js.** Paste that URL into the value of `axios.defaults.baseURL` as shown.

{{% notice warning %}}
Note that you copied and included the name of the stage **/v1** in that URL and removed the **/** at the end of the URL.
{{% /notice %}}

![VPC](/images/5.deploy/5.2-apigateway/5.2-1new.png)

The line `Vue.config.productionTip = false` will disable the tips displayed in Vue console, it will optimize in production environment.

Next, we will set the default URL for all Axios requests. It will be the endpoint URL where the API request is sent to.

In general, the code above initializes the Vue.js application by creating a Vue instance, configuring the base URL for Axios requests. It also turns off the tips displayed in the Vue console.
