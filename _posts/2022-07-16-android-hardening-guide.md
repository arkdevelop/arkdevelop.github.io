---
title: Android Hardening Guide
layout: post
post-image: "/assets/img/blog/android_hardening_guide/post-image.jpg"
description: Follow this guide to become a pro in securing your Android device.
category: Android Security
tags:
- mobile
- android
- security
- best-practices
- guidance
- NIST
- CIS benchmark
- STIG
- signal
---

In this post, we'll walk through how to harden your Android device utilizing leading industry security best practices like CIS benchmarks, globally recognized cryptographic standards, and privacy-improving applications. This is one that anyone with an Android device should read, so without further ado, let's make a splash in your security.

---

## Table of Contents
- [Table of Contents](#table-of-contents)
- [1. Why Harden Your Android Device?](#1-why-harden-your-android-device)
- [2. Benchmarks & Baselines](#2-benchmarks--baselines)
  - [2.1. CIS Benchmark](#21-cis-benchmark)
  - [2.2. U.S. DoD STIG](#22-us-dod-stig)
- [3. Communications](#3-communications)
  - [3.1. Messaging](#31-messaging)
  - [3.2. Emails](#32-emails)
- [4. VPN](#4-vpn)
- [5. Other Apps & Protections](#5-other-apps--protections)
- [6. Checklist](#6-checklist)
- [References](#references)

---

## 1. Why Harden Your Android Device?
At the start of 2022, researchers at Proofpoint (2022) detected a 500% jump in mobile malware delivery attempts in Europe. It's no secret that hackers are noticing the increasing value of mobile devices in an expanding remote-work world and taking action. Despite this knowledge of increasing attacks, it's reported that over 80% of Android devices were susceptible to at least 1/25 vulnerabilities. This is no small number and the results of which could be devastating to you or your organization's sensitive data.

It is therefore of paramount importance to take all necessary precautions when handling and configuring your Android devices to ensure that they are secure and won't become another statistic. This page seeks to provide you, the reader, with all the necessary baseline security references and best practices to follow to guarantee your device is secure.

## 2. Benchmarks & Baselines
### 2.1. CIS Benchmark
The CIS, otherwise known as the Center for Internet Security develops and makes available documents like their CIS benchmarks for user and business consumption. If you're unfamiliar with what a CIS benchmark is, they are documents used globally by thousands of businesses which define prescriptive baseline security standards for hardening various systems including the Android operating system (OS). The Android OS CIS benchmark recommendations include controls like the following: Ensuring your device is kept up to date; Confirming your device is configured with an appropriate screen lock; Avoiding untrusted networks; etc.

It is therefore recommended to download and apply the CIS Android OS benchmark guidance to your Android device. You can find the CIS Android benchmark at the link below.
<https://www.cisecurity.org/benchmark/google_android>

However, it is important to always consider your individual or business context when applying security controls, including the potential trade-offs. You may find that not all the recommendations found in the CIS benchmark suit your needs like we did. For example, we at Deep Dive Security believe in a more privacy-centric view and therefore opted to not follow recommendation 1.19 (L1) Ensure 'Improve harmful app detection' is set to 'Enabled' (Manual) which would have sent usage data to Google. Similarly, CIS recommends disabling location on devices while simultaneously making it clear that the trade-off could be the inability to locate a lost device if followed. Users and security personnel should fully comprehend the trade-offs before opting to follow a recommendation. Consult the appropriate documentation or people for assistance in decision making.

### 2.2. U.S. DoD STIG
Like the CIS benchmark, the U.S. Department of Defence (DoD) maintains security technical implementation guides, otherwise known as STIGs. STIGs are, for all intents and purposes, the same as a CIS benchmark with one important caveat. STIGs only contain policy and security controls which have been selected for a U.S. DoD baseline. Considering both the CIS benchmark and the U.S. DoD STIG are system security baselines, you may be asking the question, which one do you use? Well, in some cases regulatory requirements will decide for you. More often than not though it will be a decision of whichever fits you or your company best. Of course, if you don't want to stick to one standard, you can always use both and customize from there based on your own requirements. For example, the CIS benchmark as of this writing recommends 1.2 (L1) Ensure 'Screen Lock' is set to 'Enabled' (Manual) but does not stipulate a minimum password length or type, whereas the U.S. DoD STIG recommends a minimum device password length of 6+ numeric (complex) characters. This could be an important distinction depending on your threat model given some country's laws allow for enforcement agencies to force users to unlock their devices when secured with biometrics, while not permitting such actions when secured with a password. Users can and should review their threat model and implement controls accordingly.

It is therefore recommended, when appropriate, to also download and apply the U.S. DoD Android STIG to your Android device. You can find the U.S. DoD Android STIG at the link below.
<https://public.cyber.mil/stigs/downloads/?_dl_facet_stigs=mobility>

## 3. Communications
In addition to security baseline configurations, we at Deep Dive Security believe that Android hardening should include some of the core user functions as well. The following guidance seeks to build on top of a baseline benchmark to improve user communication security.
### 3.1. Messaging
It is undeniable that one of the utmost core functions of an Android device is messaging capabilities. However, so many messaging apps still rely on insecure Short Message Service/Multimedia Messaging Service (SMS/MMS) technology which send a user's message in the clear over open networks; enter the signal protocol. The signal protocol, which since its inception has now been adopted by some of the world's largest companies like Google and Microsoft, is a messaging protocol that implements end-to-end (E2E) encryption for increased communication security. The signal protocol goes further by also providing participant consistency, destination validation, forward secrecy, future secrecy, causality preservation, message unlinkability, message repudiation, participation repudiation, and asynchronicity.

Due to the aforementioned features and functionality, the signal protocol has become a defacto standard for messaging security and is therefore a recommended addition to improving the security of a user's Android messaging and voice calls. User's can take advantage of the signal protocol through the [Signal app](https://play.google.com/store/apps/details?id=org.thoughtcrime.securesms) from the Play Store.
### 3.2. Emails
Globally, emails amass over 333 billion every day (Statista, 2022). Evidently, emails are a predominant use of Android devices and require due care to ensure they're secure. Therefore, emails are equally as important to secure as your messaging capabilities and should follow the same E2E encryption model. This page will focus on the end user and their associated email client on Android. Consequently, certain technologies are out of scope for this page like the Sender Policy Framework (SPF), DomainKeys Identified Mail (DKIM), and Domain-based Message Authentication, Reporting and Conformance (DMARC) which could assist organizations in preventing email spoofing and phishing. For more information on these technologies, please refer to NIST (2019a), *Trustworthy Email*. 

On the topic of email clients, they generally connect via Simple Mail Transport Protocol (SMTP) and receive via Post Office Protocol, Version 3 (POP3) or Internet Message Access Protocol (IMAP). In all cases, these should be secured using an up-to-date version of Transport Layer Security (TLS). TLS will assist in protecting metadata, email headers, and login details while the data is in transit. In addition to TLS, email bodies should be encrypted as well to protect the content both at rest and in transit using Secure/Multipurpose Internet Mail Extensions (S/MIME) or Pretty Good Privacy (OpenPGP). For some, S/MIME with a certificate that is signed by a known CA is the recommended approach; users should review their threat model to determine which method best suits their own requirements.

**Figure 1.**

*Email encryption and decryption process using PGP*

![Email encryption and decryption process using PGP](/assets/img/blog/android_hardening_guide/email_encryption_and_decryption.png "Email encryption and decryption process using PGP"){:height="250px" width="600px"}

Note. Email encryption and decryption process using PGP. From *What is PGP encryption and how does it work?* by Proton, 2019, August 8. (<https://proton.me/blog/what-is-pgp-encryption>).

Luckily, the vast majority of well-known email clients/companies on Android manage the burden of handling the encryption for you. For example, the company Proton (n.d.) which is well known for their privacy and based out of Switzerland, states that Proton Mail's emails sent between Proton users are always E2E encrypted and can be made E2E encrypted for non-Proton Mail recipients via their Password-protected emails or PGP. Likewise, the emails are claimed to be stored on Proton's servers using their zero-access encryption and thus cannot be read by Proton themselves. If interested, individuals can take advantage of Proton Mail through the [Proton Mail app](https://play.google.com/store/apps/details?id=ch.protonmail.android) from the Play Store. If opting to use a different email provider/client, users should confirm the necessary cryptographic standards are upheld.
## 4. VPN
If you're on this page, chances are that you've heard of a virtual private network (VPN) and have likely seen it on many other best practices or security guidance websites. While you may already be using a VPN to help you watch that newest hit show on a different country's version of Netflix, VPNs actually provide some important security benefits as well. The security benefits of a VPN are particularly useful if you're prone to using public Wi-Fi services or live in a predominantly censored state where you need to hide your activities from your internet service provider (ISP).

So how does it work? Well, a VPN works by creating an encrypted communications tunnel between you and the internet. What this means is that your ISP, or presumably any other snooper, will no longer be able to see the contents of your traffic because you've shifted your trust from the ISP, who were not encrypting your traffic, to the VPN provider, who is encrypting your traffic. As you've likely guessed, there are some downsides to this approach. For example, since you are now shifting your trust away from your ISP to a VPN provider, you must do your due diligence in confirming that the VPN provider meets your security requirements. If you are more privacy centric, you may look for a VPN that accepts cash or Monero cryptocurrency, offers a no-logging strategy, and resides outside of specific countries. This will all come down to you or your organization's specific threat model and should be carefully scrutinized.

On the more positive side, if we look at using a VPN from a public Wi-Fi and man-in-the-middle (MITM) scenario, a VPN protects your data from the MITM attack through encrypting your data in transit and thereby prevents the cleartext data from being intercepted. It is important to note here that decryption happens at the VPN server before the data continues to its destination. Thus, it's crucial that users continue to ensure the websites they visit are using an up-to-date version of TLS. 

The basic data flow of a VPN is as follows (VPNOverview, 2022): 
<ol>
    <li>1. The VPN software on your computer encrypts your data traffic and sends it to the VPN server through a secure connection. The data also goes through your Internet Service Provider, but they can no longer snoop because of the encryption.</li>
    <li>2. The encrypted data from your computer is decrypted by the VPN server.</li>
    <li>3. The VPN server will send your data on to the internet and receive a reply, which is meant for you, the user.</li>
    <li>4. The traffic is then encrypted again by the VPN-server and is sent back to you.</li>
    <li>5. The VPN-software on your device will decrypt the data so you can actually understand and use it.</li>
</ol>

See Figure 2 below for a visual representation of where encryption resides in a VPN's data flow.

**Figure 2.**

*VPN Architecture*

![VPN Architecture](/assets/img/blog/android_hardening_guide/vpn_architecture.png "VPN Architecture"){:height="250px" width="700px"}

Note. VPN technical explanation. From *What is a VPN? Virtual Private Networks 101* by Surfshark, n.d. (<https://surfshark.com/learn/what-is-vpn>).

## 5. Other Apps & Protections
At this point we've discussed baseline benchmark security, messaging and email standards, and network-based protections like VPNs. To take it a step further, we'll now look at a defence-in-depth approach by assuming compromise of the device has already taken place. What can you do to protect your data and apps from malicious attackers who have already bypassed your main Android lock screen?

For Samsung device users, the answer is a simple one, Samsung Secure Folder protected by Knox. Samsung Secure Folder utilizes Samsung Knox Workspace dual persona container technology to "separate, isolate, encrypt, and protect data" (Samsung, 2016). Samsung Secure Folder allows users to store files (including images) in a separate, password protected, location. Similarly, Samsung Secure Folder offers users the ability to import, and password protect specific apps away from their every day apps. An example use case could be for a user who utilizes an app which maintains a logged-in status. For this example, we'll say it's the social media application, SnapChat. Therefore, compromise of the main Android lock screen or if the user forgot their device was unlocked and went to the bathroom could also result in a compromise of the user's SnapChat account which could lead to impersonation attacks, data loss, or further compromise. However, if the user followed a defence-in-depth approach by utilizing Samsung Secure Folder and password protecting their SnapChat app, the malicious actor would need to also bypass that protection mechanism. User's can take advantage of Samsung's Secure Folder through the [Secure Folder app](https://play.google.com/store/apps/details?id=com.samsung.knox.securefolder) on the Play Store.

While Samsung Secure Folder is one way to add a defence-in-depth approach to your Android device, there are several apps through the Play Store which claim to offer the same functionality. It is important, if opting to go this route, to review the app's claims and documentation to confirm they are secure for use.

## 6. Checklist
To summarize with a quick checklist, users/organizations should confirm they have completed the following. 
<ul class="contains-task-list">
    <li class="task-list-item"><input type="checkbox" class="task-list-item-checkbox" disabled=""> 1. Evaluate, customize if needed, and apply the Android CIS benchmark</li>
    <li class="task-list-item"><input type="checkbox" class="task-list-item-checkbox" disabled=""> 2. Evaluate, customize if needed, and apply the U.S. DoD Android STIG</li>
    <li class="task-list-item"><input type="checkbox" class="task-list-item-checkbox" disabled=""> 3. Download, configure, and use a signal protocol supported messaging app</li>
    <li class="task-list-item"><input type="checkbox" class="task-list-item-checkbox" disabled=""> 4. Configure and use PGP or S/MIME for email body encryption</li>
    <li class="task-list-item"><input type="checkbox" class="task-list-item-checkbox" disabled=""> 5. Confirm SMTP, POP3, and IMAP are using TLS</li>
    <li class="task-list-item"><input type="checkbox" class="task-list-item-checkbox" disabled=""> 6. Evaluate, purchase, enable, and configure a VPN</li>
    <li class="task-list-item"><input type="checkbox" class="task-list-item-checkbox" disabled=""> 7. Apply a defence-in-depth approach by utilizing app lockers and/or secure file stores</li>
</ul>

## References
Center for Internet Security. (2022, April 4). *CIS Google Android Benchmark*. <https://www.cisecurity.org/benchmark/google_android>.

Google. (2022, February). *Messages End-to-End Encryption Overview*. <https://www.gstatic.com/messages/papers/messages_e2ee.pdf>.

NIST. (2007, February). *Guidelines on Electronic Mail Security*. <https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-45ver2.pdf>.

NIST. (2019a, February). *Trustworthy Email*. <https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-177r1.pdf>.

NIST. (2019b, August). *Guidelines for the Selection, Configuration, and Use of Transport Layer Security (TLS) Implementations*. <https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-52r2.pdf>.

Proofpoint. (2022, March 9). *Mobile Malware is Surging in Europe: A Look at the Biggest Threats*. <https://www.proofpoint.com/us/blog/email-and-cloud-threats/mobile-malware-surging-europe-look-biggest-threats>.

Proton. (n.d.). *Proton Mail encryption explained*. <https://proton.me/support/proton-mail-encryption-explained>.

Proton. (2019, August 8). *What is PGP encryption and how does it work?*. <https://proton.me/blog/what-is-pgp-encryption>.

Samsung. (2016, August). *White Paper: An Overview of the Samsung Knox Platform*. <https://kp-cdn.samsungknox.com/df4184593021d7b8fabfdfeff5c318ba.pdf>.

Signal. (n.d.). *Technical information*. <https://signal.org/docs/>.

Statista. (2022, July 26). *Number of sent and received e-mails per day worldwide from 2017 to 2025*. <https://www.statista.com/statistics/456500/daily-number-of-e-mails-worldwide/>.

Surfshark. (n.d.). *What is a VPN? Virtual Private Networks 101*. <https://surfshark.com/learn/what-is-vpn>.

U.S. DoD. (n.d.). *STIGs Document Library*. <https://public.cyber.mil/stigs/downloads/?_dl_facet_stigs=mobility>.

VPNOverview. (2022, May 19). *VPN Explained: How Does it Work? Why Would You Use It?*. <https://vpnoverview.com/vpn-information/what-is-a-vpn/>.