---
title: Dockerfile Security Best Practices
layout: post
post-image: "/me/assets/images/dockerfile-security-best-practices.jpg"
description: A compilation of Dockerfile best practices and security guidance.
tags:
- docker
- dockerfile
- security
- best-practices
- guidance
---

In this post, we'll walk users through what a Dockerfile is and how to create one following leading industry best practices including but not limited to multi-stage builds, creating minimal images, use of appropriate instructions to minimize number of layers, linting, what to avoid, and more.

---

# 1. Guidance
## 1.1. Use USER Instruction
To ensure containers are not run as root, the USER instruction should be used whenever possible. Avoiding the use of root will assist in preventing host privilege escalation attacks which could be detrimental to the security of the system. To aid users with this task, some images include a built-in user. For example, Node.js includes their node user as seen below.

```dockerfile
USER node
```

If the built-in user is not required, it should be removed. Using the same Node.js example from above, this would be completed through adding the following to the Dockerfile.
```dockerfile
# For debian based images use:
RUN userdel -r node

# For alpine based images use:
RUN deluser --remove-home node
```

## 1.2. Use Minimal Dockerfile
### 1.2.1. Use Minimal Base Image
Images should be minimal and only include the necessary packages to successfully run the application. In doing so, the attack surface will be significantly reduced through removing unnecessary and potentially vulnerable packages.
Aiding developers in this effort, multiple sources have developed base images that only include the core necessities. [Distroless](https://github.com/GoogleContainerTools/distroless) and [Alpine](https://hub.docker.com/_/alpine) are two of the most common and recommended base images to use for creating minimal containers.

### 1.2.2. Use Minimal Ports
In addition to using minimal images, only necessary ports should be exposed. Use of the EXPOSE instruction will assist in outlining for users which ports are intended to be published. However, it is important to note that the EXPOSE instruction is purely for comment and/or documentation purposes. Therefore, it will not prevent exposure of additional ports at runtime.

```dockerfile
EXPOSE 80/tcp
```

## 1.3. Use Trusted Base Images
* Base images should come from a trusted source, be signed (e.g. Docker Content Trust), and vulnerability scanned.

```powershell
PS user> docker scan node:14
\ Analyzing container dependencies for node:14
\ Querying vulnerabilities database...

...

Package manager:   deb
Project name:      docker-image|node
Docker image:      node:14
Platform:          linux/amd64

Tested 413 dependencies for known vulnerabilities, found 548 vulnerabilities.
```

```powershell
PS user> docker trust inspect --pretty node:14

No signatures for node:14

Administrative keys for node:14

  Repository Key:       d3a0845e6d36c6c058ae6d2bc718b32ead4b51f2a6fa81b341ba2df72f1823c9
  Root Key:     be46625d7c6a0afe24bbbce6a92114d691e32ae921cf14f3feb2f970e7a77337
```

## 1.4. Use a Linter
A linter is a tool which statically analyzes content to flag programming errors, bugs, style errors, and more to assist in preventing undesirable outcomes. [Hadolint](https://github.com/hadolint/hadolint) is a commonly recommended Dockerfile linter which can aid in ensuring the user's Dockerfile follows best-practices, like confirming the user has explicitly tagged a version for their base image. It is therefore recommended that a linter is used while writing a Dockerfile whenever possible.

**Figure 1**

*Hadolint example*

![Test Image](/me/assets/images/hadolint_example.png)

## 1.5. Multi-stage Building
```dockerfile
FROM node:10 as build-env
COPY ./app
WORKDIR /app

RUN npm ci --only=production

FROM gcr.io/distroless/nodejs:10
COPY --from=build-env /app /app
WORKDIR /app
CMD ["app.js"]
```

## 1.6. Avoid Including Secrets or Credentials

## 1.7. Use .dockerignore
* Use a .dockerignore file to explicitly exclude undesirable files and directories from the build (e.g. Credential files, backups, etc.)

## 1.8. Group RUN, COPY, and ADD Instructions
* RUN, COPY, and ADD instructions should be grouped onto a single line whenever possible to reduce the number of container layers

## 1.9. Avoid Using Latest Tag
* Avoid using the latest tag on images as it could break the application later

# References
Center for Internet Security. (n.d.). *CIS Docker Benchmarks*. <https://www.cisecurity.org/benchmark/docker/>.

Docker. (n.d.-a). *Best Practices for Writing Dockerfiles*. <https://docs.docker.com/develop/develop-images/dockerfile_best-practices/>.

Docker. (n.d.-b). *Build Images with BuildKit*. <https://docs.docker.com/develop/develop-images/build_enhancements/>.

Docker. (n.d.-c). *Content Trust in Docker* <https://docs.docker.com/engine/security/trust/>

Docker. (n.d.-d). *Vulnerability Scanning for Docker Local Images* <https://docs.docker.com/engine/scan/>

Gartmann, D. (2018, July 10). *Quick Wins to Secure your Docker Containers*. <https://www.equalexperts.com/blog/tech-focus/quick-wins-to-secure-your-docker-containers/>.

GoogleContainerTools. (2020, July 28). *GitHub Repository*. 

<https://github.com/GoogleContainerTools/distroless/blob/main/examples/nodejs/Dockerfile>.

Iradier, A. (2021, March 9). *Top 20 Dockerfile Best Practices*. <https://sysdig.com/blog/docker-file-best-practices/>.

Nodejs. (2020, November 2). *Docker and Node.js Best Practices*. <https://github.com/nodejs/docker-node/blob/main/docs/BestPractices.md>.