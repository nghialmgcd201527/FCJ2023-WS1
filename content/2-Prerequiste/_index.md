---
title : "Preparation "
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

{{% notice info %}}
You need to create 1 Linux instance on the public subnet and 1 Window instance on the private subnet to perform this lab.
{{% /notice %}}

To learn how to create EC2 instances and VPCs with public/private subnets, you can refer to the lab:
  - [About Amazon EC2](https://000004.awsstudygroup.com/en/)
  - [Works with Amazon VPC](https://000003.awsstudygroup.com/en/)

In order to use System Manager to manage our window instances in particular and our instances in general on AWS, we need to give permission to our instances to be able to work with System Manager. In this preparation, we will also proceed to create an IAM Role to grant permissions to instances that can work with System Manager.

### Content
  - [Prepare VPC and EC2](2.1-createec2/)
  - [Create IAM Role](2.2-createiamrole/)