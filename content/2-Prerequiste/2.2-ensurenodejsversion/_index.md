---
title: "Ensure Node.js Version"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 2.2 </b> "
---

Cloud9 has Node.js pre-installed but it may not be compatible with the time I do this workshop. To ensure this, install Node.js v16 (codename **Gallium**) by running the following command in the Cloud9 terminal.

Let's check the current Node.js version running on Cloud9 with the following command:

```
node --version

```

This is the version of Node.js that I am using in this workshop.

![VPC](/images/2.prerequisite/2.2-ensurenodejsversion/ensurenodejs-1.png)

If the version of Node.js you are using is v16.x then you can skip this step and go to [**Clone Git repository**](/2-prerequiste/2.3-clonerepositoryandavoidingfreespace). If the version you are using is different from the above then follow along Follow these steps to install the right version of Node.js for this workshop using the terminal page you just created.

At the terminal page on Cloud9, run the following command to make sure you have the latest version of [Node.js Version Manager (nvm)](https://github.com/nvm-sh/nvm) installed (At the time I wrote the workshop, it was version 0.39.0).

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.2/install.sh | bash

```

Next we will install Node.js v16 "Gallium":

```
nvm install 'lts/gallium'

```

Finally, we will choose the above Node.js version as the default version:

```
nvm alias default 'lts/gallium'

```

Review the Node.js instance running on Cloud9 with the following command:

```
node --version

```

The result we need is to start with `v16`.
