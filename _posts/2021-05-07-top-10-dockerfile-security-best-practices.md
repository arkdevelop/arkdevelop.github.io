---
title: Top 10 Dockerfile Security Best Practices for a More Secure Container
layout: post
post-image: "/assets/images/dockerfile-security-best-practices.jpg"
description: A compilation of Dockerfile best practices and security guidance.
category: Containers
tags:
- docker
- dockerfile
- security
- best-practices
- guidance
---

In this post, we'll walk through what a Dockerfile is and how to create one following leading industry security best practices including but not limited to multi-stage builds, creating minimal images, use of appropriate instructions to minimize number of layers, linting, what to avoid, and more. So lets dive deep into Dockerfile security.

---

## Table of Contents
- [1. Guidance](#1-guidance)

    - [1.1. Use USER Instruction](#11-use-user-instruction)

    - [1.2. Use Minimal Base Image](#12-use-minimal-base-image)

    - [1.3. Use Minimal Ports](#13-use-minimal-ports)

    - [1.4. Use Trusted and Secure Base Images](#14-use-trusted-and-secure-base-images)

        - [1.4.1. Check for Vulnerabilities](#141-check-for-vulnerabilities)

        - [1.4.2. Use Signed Images](#142-use-signed-images)

    - [1.5. Use a Linter](#15-use-a-linter)

    - [1.6. Avoid Using Latest Tag](#16-avoid-using-latest-tag)

    - [1.7. Group RUN, COPY, and ADD Instructions](#17-group-run-copy-and-add-instructions)

    - [1.8. Multi-stage Building](#18-multi-stage-building)

    - [1.9. Avoid Including Secrets or Credentials](#19-avoid-including-secrets-or-credentials)

    - [1.10. Use .dockerignore](#110-use-dockerignore)

- [References](#references)

---

## 1. Guidance
### 1.1. Use USER Instruction
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

### 1.2. Use Minimal Dockerfile Base Image
Images should be minimal and only include the necessary packages to successfully run the application. In doing so, the attack surface will be significantly reduced through removing unnecessary and potentially vulnerable packages.
Aiding developers in this effort, multiple sources have developed base images that only include the core necessities. [Distroless](https://github.com/GoogleContainerTools/distroless) and [Alpine](https://hub.docker.com/_/alpine) are two of the most common and recommended base images to use for creating minimal containers.

### 1.3. Use Minimal Ports
In addition to using minimal images, only necessary ports should be exposed. Use of the EXPOSE instruction will assist in outlining for users which ports are intended to be published. However, it is important to note that the EXPOSE instruction is purely for comment and/or documentation purposes. Therefore, it will not prevent exposure of additional ports at runtime.

```dockerfile
EXPOSE 80/tcp
```

### 1.4. Use Trusted and Secure Base Images
Docker images should come from a trusted source, be signed, and be vulnerability scanned to achieve an appropriate level of risk mitigation. While there are a number of tools available to obtain such results, Docker comes prepackaged with the docker scan command thus offering a quick and easy way for users to scan images.

#### 1.4.1. Check for Vulnerabilities
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

#### 1.4.2. Use Signed Images
Docker images should be signed from a trusted source before use. One way to ensure only signed images are used is to enforce Docker Content Trust. On the client, it can be enforced on a per session basis through using the following command.
```bash
$ export DOCKER_CONTENT_TRUST=1
```


After Docker Content Trust has been set, if the client tries to pull an untrusted image, it will be presented with an error such as the following.

```bash
$ docker pull node:14
No valid trust data for 14
```


To check already pulled images, Docker offers the docker trust inspect command as shown below.

```powershell
PS user> docker trust inspect --pretty node:14

No signatures for node:14

Administrative keys for node:14

  Repository Key:       d3a0845e6d36c6c058ae6d2bc718b32ead4b51f2a6fa81b341ba2df72f1823c9
  Root Key:     be46625d7c6a0afe24bbbce6a92114d691e32ae921cf14f3feb2f970e7a77337
```

### 1.5. Use a Linter
A linter is a tool which statically analyzes content to flag programming errors, bugs, style errors, and more to assist in preventing undesirable outcomes. [Hadolint](https://github.com/hadolint/hadolint) is a commonly recommended Dockerfile linter which can aid in ensuring the user's Dockerfile follows best-practices, like confirming the user has explicitly tagged a version for their base image. It is therefore recommended that a linter is used while writing a Dockerfile whenever possible.

**Figure 1**

*Hadolint example*

![Hadolint example](/assets/images/hadolint_example.png "Hadolint linter"){:height="1920px" width="720px"}

### 1.6. Avoid Using Latest Tag
As seen in section [1.4. Use a Linter](#14-use-a-linter), Hadolint recommends avoiding the use of the latest tag in Dockerfiles. Users should avoid using the latest tag due to the potential for breaking functionality or introducing unknowns into the environment in the future should the image update. Instead of using the latest tag, users should opt to specify an explicit release tag.

```dockerfile
FROM node:14
```

### 1.7. Group RUN, COPY, and ADD Instructions
RUN, COPY, and ADD instructions in Dockerfiles each create a new layer when invoked. It is therefore a best practice to group these instructions onto a single line whenever possible to reduce the number of container layers.

### 1.8. Multi-stage Building
Multi-stage building in Dockerfiles offers a way to make intermediate builds, otherwise known as stages. Stages allow users to copy the resulting build from a previous stage and use it in a subsequent stage. Use of multi-stage building will reduce the attack surface and potential vulnerabilities of a build by limiting the packages found in the final image.

For example, a user may want to have a stage which utilizes build tools, but have no requirements for those build tools in the final image. Through multi-stage building, they would start a stage with the FROM "base_image" as "name_of_stage1" instruction, then copy the result from that stage in their next FROM stage using the COPY --from="name_of_stage1" instruction. See the following Dockerfile for an example using Node:10 as the build environment followed by Google's Distroless Node:10 for the final image.

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

### 1.9. Avoid Including Secrets or Credentials
Docker uses layer caching which essentially means that all of the layers are still present in the final image. Certain commands like docker history will show the layers present in an image and how they were built. See Figure 2 below for an example using Node:14. 

```powershell
docker history node:14
```

**Figure 2**

*Docker history of Node:14*

![Docker history of Node:14](/assets/images/docker_history_node_14.png "Docker history of Node 14"){:height="1920px" width="720px"}

This becomes especially concerning when it comes to using secrets or credentials in a Dockerfile since any user with access to the image will be able to see the contents. To overcome the security concern, it is paramount that users never include secrets or credentials in their Dockerfile whether written in plaintext in the Dockerfile directly, passed in as a file, or passed in as a build argument.

Users should instead opt to use tooling like [Docker BuildKit](https://docs.docker.com/develop/develop-images/build_enhancements/) with the --secret command line option to pass in required secret information.

For example, in the Dockerfile the user would access the secret through the RUN command as seen below.

```dockerfile
RUN --mount=type=secret,id=secret cat /run/secrets/secret
```

During the build, the secret would be passed in through the --secret flag.

```powershell
docker build --no-cache --progress=plain --secret id=secret,src=secret.txt .
```

### 1.10. Use .dockerignore
A .dockerignore file is similar to a .gitignore by which developers can specify files or directories to exclude from the build context, therefore preventing it from being included in the final image. .dockerignore files are particularly useful for explicitly excluding sensitive files and directories like credential files, backups, logs, and more. The below example ensures that a .git folder, a logs folder, and all files ending with the .md extension except for the README.md file are excluded from the build context. See [Docker's .dockerignore documentation](https://docs.docker.com/engine/reference/builder/#dockerignore-file) for further information.

```dockerignore
.git
logs
*.md
!README.md
```

## References
Center for Internet Security. (n.d.). *CIS Docker Benchmarks*. <https://www.cisecurity.org/benchmark/docker/>.

Docker. (n.d.-a). *Best Practices for Writing Dockerfiles*. <https://docs.docker.com/develop/develop-images/dockerfile_best-practices/>.

Docker. (n.d.-b). *Build Images with BuildKit*. <https://docs.docker.com/develop/develop-images/build_enhancements/>.

Docker. (n.d.-c). *Content Trust in Docker* <https://docs.docker.com/engine/security/trust/>.

Docker. (n.d.-d). *Vulnerability Scanning for Docker Local Images* <https://docs.docker.com/engine/scan/>.

Gartmann, D. (2018, July 10). *Quick Wins to Secure your Docker Containers*. <https://www.equalexperts.com/blog/tech-focus/quick-wins-to-secure-your-docker-containers/>.

GoogleContainerTools. (2020, July 28). *GitHub Repository*. 

<https://github.com/GoogleContainerTools/distroless/blob/main/examples/nodejs/Dockerfile>.

Iradier, A. (2021, March 9). *Top 20 Dockerfile Best Practices*. <https://sysdig.com/blog/docker-file-best-practices/>.

Nodejs. (2020, November 2). *Docker and Node.js Best Practices*. <https://github.com/nodejs/docker-node/blob/main/docs/BestPractices.md>.