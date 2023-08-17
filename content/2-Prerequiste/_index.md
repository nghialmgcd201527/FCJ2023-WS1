---
title: "Prerequisite"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

{{% notice warning %}}
Remember that Cloud9 workspaces should only be created by an IAM user (or assigned to an appropriate IAM role) with Admin privileges, not a root user.
{{% /notice %}}

#### **Cloud9 Workspace**

We often use the Integrated Development Environment (IDE) locally, in this workshop we will use Cloud9. It is an IDE that runs in the cloud using a browser, including the important, essential features in the local IDE that we often use such as writing, running, debugging code. Cloud9 already comes with bundles of files like JavaScript, Python, NodeJS and [others here](https://aws.amazon.com/cloud9/)

> For AWS services' faster response, we recommend you to select the nearest Region during this workshop.

#### **Region Support for Amazon Rekognition**

Note that not all services are available in all regions. Later in this workshop, we will be using [Amazon Rekognition](https://aws.amazon.com/rekognition/), which is available only in the following regions::

- US East (Ohio) us-east-2
- US East (N. Virginia) us-east-1
- US West (N. California) us-west-1
- US West (Oregon) us-west-2
- Asia Pacific (Mumbai) ap-south-1
- Asia Pacific (Seoul) ap-northeast-2
- Asia Pacific (Singapore) ap-southeast-1
- Asia Pacific (Sydney) ap-southeast-2
- Asia Pacific (Tokyo) ap-northeast-1
- Canada (Central) ca-central-1
- Europe (Frankfurt) eu-central-1
- Europe (Ireland) eu-west-1
- Europe (London) eu-west-2

See more Amazon Rekognition [latest updates](https://docs.aws.amazon.com/general/latest/gr/rekognition.html).

### Nội dung

- [Create a Cloud9 Workspace](2.1-createcloud9workspace/)
- [Ensure Node.js Version](2.2-ensurenodejsversion/)
- [Clone the Git repository and avoid free space issues with Cloud9](2.3-clonerepositoryandavoidingfreespace/)
- [Install Amplify CLI](2.4-installamplifycli/)
