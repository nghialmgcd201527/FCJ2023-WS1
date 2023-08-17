---
title: "Initialize Amplify"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 5.3 </b> "
---

It's time to set up Amplify so that we can create the necessary backend services needed to support the app. In this step, you'll configure Amplify hosting.

Ensure you are in the root of the web project (`/webapp`):

```
cd ~/environment/serverless-tasks-webapp/webapp
```

Initialize Amplify:

```
amplify init
```

We will name the project **tasks** and press **Enter** to continue. It will display information about the project.

Next, it will ask if we want to initialize the project with the above information, enter **Y** and press **Enter**.

Then it will ask us to choose the authentication method we want, we select **AWS profile** and press **Enter**. And select the default profile that we configured in the previous step.

We will do as shown below.

![VPC](/images/5.deploy/5.3-initializeamplify/5.3-1new.png)

{{% notice tip %}}
The CLI can determine for itself the appropriate configuration for the project on which Amplify is initialized. In this workshop, CLI knows we are using Vue.js and provides proper configuration with app type, framework, source, distribution, build and start options.
{{% /notice %}}

When you initialize a new Amplify project, a few things happen:

- It will create a new directory named **amplify** in the root directory of the project and contain the initializations of the backend.
- It creates a file named **aws-exports.js** in the **src** directory of the project. This file contains the necessary configuration information for the services you create with Amplify, this is how Amplify users can get the necessary information of the backend services.
- Cloud projects created in the AWS Amplify Console can be accessed by running the `amplify console` command. The console provides a list of backend environments, resources implemented in the Amplify category, the current left page of deployments, and other information.

The Amplify initialization process is complete, we will receive the message as shown below.

![VPC](/images/5.deploy/5.3-initializeamplify/5.3-2.png)

Now that our Vue.js app is set up and Amplify is initialized, we're ready to add Amplify hosting in the next steps.
