---
title: "Introduction"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1. </b> "
---

The application architecture uses **AWS Lambda, Amazon API Gateway, Amazon DynamoDB, Amazon Simple Storage Service (S3), and AWS Amplify Console.** Amplify Console provides continuous deployment and hosting of the static web resources including HTML, CSS, JavaScript, and image files which are loaded in the user's browser. JavaScript executed in the browser sends and receives data from a public backend API built using Lambda and API Gateway. DynamoDB provides a persistence layer where data can be stored by the API's Lambda function. S3 is used to store uploaded images. Finally, Amazon Rekognition is used to detect and label objects in those images.

![VPC](/images/1.intro/1-1.png)
