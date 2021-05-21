---
title: Dockerfile Security Best Practices
layout: post
post-image: "https://raw.githubusercontent.com/thedevslot/WhatATheme/master/assets/images/SamplePost.png?token=AHMQUEPC4IFADOF5VG4QVN26Z64GG"
description: A compilation of Dockerfile best practices and security guidance.
tags:
- sample
- post
- test
---

This post will walk users through Dockerfile security best practices including but not limited to multi-stage builds, use of appropriate instructions, linting, and more.

---

# 1. Guidance
## 1.1. User USER Instruction
To ensure containers are not run as root, the USER instruction should be used whenever possible. Avoiding the use of root will assist in preventing host privilege escalation attacks which could be detrimental to the security of the system. To aid users with this task, some images include a built-in user. For example, Node.js includes their node user as seen below.
`USER node`
If the built-in user is not required, it should be removed. Using the same Node.js example from above, this would be completed through adding the following to the Dockerfile.
`# For debian based images use: \nRUN userdel -r node\n# For alpine based images use: \nRUN deluser --remove-home node`

## 1.2. Use Minimal Dockerfile
### 1.2.1. Use Minimal Base Image
Images should be minimal and only include the necessary packages to successfully run the application. In doing so, the attach surface will be significantly reduced by removing unnecessary and potentially vulnerable packages.
Aiding developers in this effort, multiple sources have developed base images that only include the core necessities. [Distroless](#) and [Alpine](#) are two of the most common and recommended base images to use for creating minimal containers.

### 1.2.2. Use Minimal Ports
In addition to using minimal images, only necessary ports should be exposed. Use of the EXPOSE instruction will assist in outlining for users which ports are intended to be published. However, it is important to note that the EXPOSE instruction is purely for comment and/or documentation purposes. Therefore, it will not prevent exposure of additional ports at runtime.
`EXPOSE 80/tcp`

## 1.3. Use a Linter
A linter is a tool which statically analyzes content to flag programming errors, bugs, style errors, and more to assist in preventing undesirable outcomes. [Hadolint](#) is a commonly recommended Dockerfile linter which can aid in ensuring the user's Dockerfile follows best-practices like confirming the user has explicitly tagged a version for their base image. It is therefore recommended that a linter is used while writing a Dockerfile whenever possible.