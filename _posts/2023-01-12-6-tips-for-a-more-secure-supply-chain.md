---
title: 6 Tips for a More Secure Supply Chain
layout: post
post-image: "/assets/img/blog/6-tips-for-a-more-secure-supply-chain/post-image.png"
description: Implement these 6 tips to secure your software supply chain from attacks.
category: DevSecOps
tags:
- software engineering
- software development
- supply chain
- CI/CD
- security
- best-practices
- guidance
- NIST
- OWASP
- Mitre
- DevSecOps
---

Software supply chain security is a critical concern for organizations today, as they continue to rely on a wide variety of software applications and services. With the rise of cloud computing, open-source software, and third-party vendors, the attack surface for software-based threats has grown exponentially. As a result, organizations must take proactive steps to protect their supply chains from a variety of potential threats including malware, counterfeit software, and vulnerabilities in third-party components.

In this post, we will explore the best practices and industry standards for protecting supply chains, with a focus on the National Institute of Standards and Technology (NIST), Supply-Chain Levels for Software Artifacts (SLSA), the Center for Internet Security’s (CIS) Software Supply Chain Security Guide, Microsoft, the Software Supply Chain Assurance (SSCA) framework, and GitHub. If the SolarWinds hack scared you, then this is a post you won’t want to miss; so, let’s get started.

---

## Table of Contents
- [1. Supply Chain Threats](#1-supply-chain-threats)
- [2. Best Practices for Protecting Software Supply Chains](#2-best-practices-for-protecting-software-supply-chains)
    - [2.1. Conduct Regular Security Assessments](#21-conduct-regular-security-assessments)
    - [2.2. Establish a Secure Development Process](#22-establish-a-secure-development-process)
    - [2.3. Keep Software and Dependencies Up to Date](#23-keep-software-and-dependencies-up-to-date)
    - [2.4. Use Secure Software Distribution Channels](#24-use-secure-software-distribution-channels)
    - [2.5. Vulnerability Monitoring and Scanning](#25-vulnerability-monitoring-and-scanning)
        - [2.5.1. Monitor for Third-Party Vulnerabilities](#251-monitor-for-third-party-vulnerabilities)
        - [2.5.2. Implement Software Composition Analysis (SCA)](#252-implement-software-composition-analysis-sca)
        - [2.5.3. Implement Static Application Security Testing (SAST) and Dynamic Application Security Testing (DAST)](#253-implement-static-application-security-testing-sast-and-dynamic-application-security-testing-dast)
    - [2.6. Use Supply-Chain Levels for Software Artifacts (SLSA)](#26-use-supply-chain-levels-for-software-artifacts-slsa)
- [3. Conclusion](#3-conclusion)
- [References](#references)

---

## 1. Supply Chain Threats
The first step in protecting a software supply chain is to understand the different types of threats that organizations are facing against it. NIST’s Cybersecurity Framework (CSF) identifies the following types of software supply chain threats:

<ul>
    <li>Tampering: This type of threat involves unauthorized changes to software or its associated data. Examples include the insertion of malware into software, or the modification of software to bypass security controls.</li>
    <li>Counterfeit software: This type of threat involves the distribution of fraudulent software or software that has been tampered with in some way. Examples include software that has been cracked or pirated, or software that has been bundled with malware.</li>
    <li>Vulnerabilities in third-party components: This type of threat involves vulnerabilities in software components that are developed and maintained by third parties. Examples include open-source software libraries, frameworks, and other components that are widely used by many organizations.</li>
</ul>

Like NIST, Mitre also has a Supply Chain Compromise identification and available mitigations including the following tactics:

<ul>
    <li><a href="https://attack.mitre.org/techniques/T1195/" target="_blank">T1195: Supply Chain Compromise</a></li>
    <ul>
        <li>T1195.001: Compromise Software Dependencies and Development Tools</li>
        <li>T1195.002: Compromise Software Supply Chain</li>
        <li>T1195.003: Compromise Hardware Supply Chain</li>
    </ul>
</ul>

Note that this is not an exhaustive list, and organizations should stay up to date on emerging threats and technologies that could introduce additional attack vectors.

## 2. Best Practices for Protecting Software Supply Chains
Once an organization understands its threat model, it can take steps to protect its software supply chain using a combination of best practices and industry standards. Some of the most important best practices include the following.

### 2.1. Conduct Regular Security Assessments
Conducting regular security assessments is essential for identifying vulnerabilities in software and taking steps to address them. The <a href="https://www.nist.gov/itl/publications-0/nist-special-publication-800-series-general-information" target="_blank">NIST SP 800 series</a> recommends organizations conduct regular assessments of their software supply chain and has a guideline for assessments that can be used as a starting point.

### 2.2. Establish a Secure Development Process
Establishing a secure development process is essential for ensuring that software is developed in a way that minimizes the risk of vulnerabilities. Microsoft (n.d.) recommends a secure software development life cycle (SSDLC) that includes activities such as threat modeling, code reviews, application security testing, and penetration testing. While there are a number of these frameworks available like the <a href="https://www.bsa.org/reports/updated-bsa-framework-for-secure-software" target="_blank">BSA Framework</a>, <a href="https://www.microsoft.com/en-us/securityengineering/sdl" target="_blank">Microsoft’s SDL</a>, <a href="https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-160v1r1.pdf" target="_blank">NIST 800–64</a>, <a href="https://www.iso.org/standard/44378.html" target="_blank">ISO/IEC 27034</a>, and others, <a href="https://csrc.nist.gov/Projects/ssdf" target="_blank">NIST’s Secure Software Development Framework (SSDF)</a> maps to many of them and is a great one-stop-shop for establishing a secure development process. The framework that you or your organization decides on will depend a lot on your own requirements including what industry you’re in.

**Figure 1.**

*Microsoft SDL*

![Microsoft SDL](/assets/img/blog/6-tips-for-a-more-secure-supply-chain/microsoft_sdl.webp "Microsoft SDL"){:height="100%" width="100%"}

Note. Software composition analysis (SCA) architecture diagram. From *What is Software Composition Analysis (SCA)?* by Palo Alto Networks, n.d. (<https://www.paloaltonetworks.com/cyberpedia/what-is-sca>).

### 2.3. Keep Software and Dependencies Up to Date
Keeping software and dependencies up to date is essential for ensuring that organizations are protected against known vulnerabilities. NIST’s CSF (n.d.-b) recommends organizations implement a software patch management process. Additionally, CIS provides guidelines for software patch management that organizations can use as a starting point.

### 2.4. Use Secure Software Distribution Channels
NIST’s CSF (n.d.-b) recommends organizations use verified software distribution channels. Using secure software distribution channels is essential for ensuring that organizations are only using software that has been obtained from a trusted source. Examples of secure software distribution channels include private registries that have validated the source of the stored images through means like signature verification. This could include a capability like <a href="https://docs.docker.com/engine/security/trust/" target="_blank">Docker’s Content Trust</a>.

### 2.5. Vulnerability Monitoring and Scanning
Vulnerability scanning is a mitigation identified in the Mitre ATT&CK supply chain compromise mitigations and can assist in reducing a supply chain’s attack surface through remediating vulnerabilities which are found. There are a few methods for finding vulnerabilities throughout the supply chain which have been identified in subsequent sections below.

#### 2.5.1. Monitor for Third-Party Vulnerabilities
Monitoring for third-party vulnerabilities is essential for identifying vulnerabilities in third-party components and taking steps to address them. The SSCA framework, created by the National Cybersecurity Center of Excellence (NCCoE), provides guidelines for managing third-party software vulnerabilities. NIST’s CSF also recommends that organizations monitor for vulnerabilities in third-party software and has a guideline for vulnerability management. Additionally, there are many vulnerability management tools and services available, such as GitHub’s Dependabot, that organizations can use to automate the process of identifying and addressing vulnerabilities in third-party components.

#### 2.5.2. Implement Software Composition Analysis (SCA)
GitHub’s Dependabot, as mentioned above, is categorized as a software composition analysis (SCA) tool. Software composition analysis is a process that helps organizations to identify the open-source and third-party components in their applications and understand the associated security risks. SCA provides an inventory of all the components and their versions, vulnerabilities, licenses, and other information, otherwise known as a software bill of materials (SBoM). This can help organizations in identifying and addressing vulnerabilities in third-party software and complying with legal requirements regarding open-source software. SCA can be integrated in an SDLC and similarly be used as a continuous process to monitor a supply chain.

**Figure 2.**

*SCA Architecture Diagram*

![SCA Architecture](/assets/img/blog/6-tips-for-a-more-secure-supply-chain/sca_architecture.webp "SCA Architecture"){:height="100%" width="100%"}

Note. Software composition analysis (SCA) architecture diagram. From *What is Software Composition Analysis (SCA)?* by Palo Alto Networks, n.d. (<https://www.paloaltonetworks.com/cyberpedia/what-is-sca>).

#### 2.5.3. Implement Static Application Security Testing (SAST) and Dynamic Application Security Testing (DAST)
Static Application Security Testing (SAST) and Dynamic Application Security Testing (DAST) are two methods for identifying vulnerabilities in software. SAST is a process that analyzes the source code of an application and looks for potential vulnerabilities. DAST, on the other hand, is a process that looks for vulnerabilities by running an application and interacting with it in a way that simulates an attacker. Both SAST and DAST can provide valuable information about the security of an application, but they are best used together in a comprehensive testing strategy. Organizations should look to implement both SAST and DAST in their CI/CD pipelines to ensure comprehensive application security testing (AST) is conducted. Note, however, that SAST and DAST do not replace penetration testing.

### 2.6. Use Supply-Chain Levels for Software Artifacts (SLSA)
Supply-Chain Levels for Software Artifacts (SLSA) is a framework that classifies software supply chain risks based on the level at which they occur. SLSA defines four levels of the software supply chain:

<ol>
    <li>1. Development environment: This level includes the tools, libraries, and frameworks used to develop software. Vulnerabilities in the development environment can lead to vulnerabilities in the software that is developed.</li>
    <li>2. Build environment: This level includes the tools and processes used to build software from source code. Vulnerabilities in the build environment can lead to vulnerabilities in the software that is built.</li>
    <li>3. Deployment environment: This level includes the infrastructure used to deploy and run software. Vulnerabilities in the deployment environment can lead to vulnerabilities in the software that is running.</li>
    <li>4. Distribution environment: This level includes the channels used to distribute software to customers. Vulnerabilities in the distribution environment can lead to customers receiving compromised software.</li>
</ol>

The SLSA framework helps organizations in understanding the risks at different levels of the software supply chain and assists with prioritizing them. Understanding the risks at different levels of the supply chain enables organizations to implement appropriate controls and countermeasures that can effectively reduce the risks. This framework can be used in conjunction with other software security frameworks, like NIST’s CSF and the SSCA framework to get a comprehensive picture of an organization’s supply chain risks.

One example of implementing the SLSA framework is to identify the risks within the distribution level by using a software artifact repository like GitHub and using its security features like the aforementioned Dependabot. It is a continuous process and allows organizations to identify risks early and mitigate them before the software reaches the end-user.

## 3. Conclusion
Supply chain security is an important consideration for any organization that relies on a wide variety of software applications and services. By understanding the types of threats that organizations are facing, implementing best practices, and using industry standards, organizations can take proactive steps to protect their environments from a variety of potential threats. Some key references and best practices include: NIST’s Cybersecurity Framework, Microsoft’s SDL, the SSCA framework, CIS’ Software Supply Chain Security Guide, and Mitre. It is important to note that this is not an exhaustive list and organizations should always stay updated with the latest threat trends, vulnerabilities, and countermeasures to reduce their potential attack surface.

## References
CIS. (2022, June). *CIS Software Supply Chain Security Guide*. <https://www.cisecurity.org/insights/white-papers/cis-software-supply-chain-security-guide>.

Docker. (n.d.). *Content Trust in Docker*. <https://docs.docker.com/engine/security/trust/>.

Microsoft. (n.d.). *What are the Microsoft SDL Practices?*. <https://www.microsoft.com/en-us/securityengineering/sdl/practices>.

Microsoft. (2022, December 9). *Microsoft Security Development Lifecycle*. <https://learn.microsoft.com/en-us/windows/security/threat-protection/msft-security-dev-lifecycle>.

Mitre. (2022, April 28). *Supply Chain Compromise*. <https://attack.mitre.org/techniques/T1195/>.

NIST. (n.d.-a). *Cybersecurity Supply Chain Risk Management*. <https://csrc.nist.gov/Projects/cyber-supply-chain-risk-management>.

NIST. (n.d.-b). *Cybersecurity Framework*. <https://www.nist.gov/cyberframework>.

OWASP. (n.d.). *OWASP Software Component Verification Standard*. <https://owasp.org/www-project-software-component-verification-standard/>.

Palo Alto Networks. (n.d.) *What is Software Composition Analysis (SCA)?*. <https://www.paloaltonetworks.com/cyberpedia/what-is-sca>.

RedHat. (2022, December 14). *What is Software Supply Chain Security*. <https://www.redhat.com/en/topics/security/what-is-software-supply-chain-security>.

SLSA. (n.d.). *Safeguarding Artifact Integrity Across any Software Supply Chain*. <https://slsa.dev/>.