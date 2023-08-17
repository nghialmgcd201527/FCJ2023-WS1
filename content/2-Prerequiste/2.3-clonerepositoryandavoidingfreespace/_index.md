---
title: "Clone the Git repository and avoid free space issues with Cloud9"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 2.3 </b> "
---

At the terminal page on Cloud9, run the following command to clone the Git repository **serverless-tasks-webapp**:

```
git clone https://github.com/aws-samples/serverless-tasks-webapp

```

After running the above command, we will get as shown. We should see the **serverless-tasks-webapp** folder created.

![VPC](/images/2.prerequisite/2.3-clonerepo/clonerepo-1.png)

#### Avoid Cloud9 Free Space Issues

{{% notice info %}}
By default, the free space of a Cloud9 instance is only about 2GB. Use the script below to avoid running out of space and problems during the workshop.
{{% /notice %}}

First, we will update to the latest version of the AWS CLI:

```
pip install --user --upgrade awscli aws-sam-cli

```

If the update is successful, we will see as shown below:

![VPC](/images/2.prerequisite/2.3-clonerepo/clonerepo-2.png)

Go to the **serverless-tasks-webapp directory,** check the size of the current volume with the following command:

```
cd serverless-tasks-webapp
df -h

```

We will get the result as shown below:

![VPC](/images/2.prerequisite/2.3-clonerepo/2.3-3.png)

filesystem at the path /dev/nvme0n1p1 is the volume we are using. We will see that the free space of this volume is 3.5G. To avoid running out of space during the workshop, we will increase the capacity of this volume to 20G.

Increase Amazon EBS volume size to 20G:

```
bash resize.sh 20

```

We will see output like this:

![VPC](/images/2.prerequisite/2.3-clonerepo/clonerepo-4.png)

```
df -h

```

Now check the size of the current volume with the following command:

![VPC](/images/2.prerequisite/2.3-clonerepo/2.3-5.png)

As we can see in the path /dev/nvme0n1p1, the free space of the current volume has increased to 14G.
