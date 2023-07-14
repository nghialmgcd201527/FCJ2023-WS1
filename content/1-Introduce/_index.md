---
title : "Introduction"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---
**Session Manager** is a function within the AWS System Manager service, SSM provides verifiable and secure version management without opening incoming ports, without Bastion Host or SSH key management. Session Manager also makes it easy to comply with corporate policies that require controlled access to instances, strict security practices, and fully auditable logs with instance access details, while still providing end-users with one-click cross-platform access to your managed instances.

By using Session Manager, you get the following advantages that traditional methods do not have:

- No need to open port 22 for SSH protocol, so it is more secure.
- Can be configured so that the connection does not need to go outside the internet, so it is more secure.
- No need to manage the server's private key to connect to SSH.
- Centralized management of users using AWS IAM.
- Access to the server easily and simply with one click.
- Faster access time than traditional methods like SSH
- Support many different operating systems such as Linux, Windows, MacOS
- Log the connection sessions and commands executed while connecting to the server.
  
With the above advantages, you can use Session Manager instead of using Bastion host technique to save us time and money when managing Bastion server. 