---
layout: post
title: "AUTOSAR Crypto stack #1 Overview"
date:   2025-01-23 21:50:33
categories: [Automotive]
tags: [autosar, security, beginner]
last_modified_at: 2025-02-14
---

## Why Autosar crypto stack for the first post on my blog?

> - Automotive software, especially Autosar is my job.

> - Crypto stack is the most basic concept when implementing any security related feature in Autosar.

> - For Autosar, you might have seen a bunch of posts about other topic like RTE, OS, Com, NvM, Diagnostic,... but not so much about crypto stack. 
    
#### Another question, why there is such a difference?

### 1. It is not mandatory!
> - Yes, security feature is optional for the function of a normal system. The purpose is to protect the assets from attack of hacker.

> - If no protection is necessary, then the use of crypto stack is not needed.

### 2. The requirement for implementing security feature is project-specific.
> - The information is confidental.

> - It is hard to share about security with no certain example.

## Then, let's get started.

NOTE: All attached materials, such as images, are either created by me or sourced from publicly available resources provided by their respective creators (references will be included in the attachment section at the bottom of the post). I assure you that everything I share here does not contain any project-related information. This blog itself is entirely non-profit, think of it as a place to store and share knowledge. Enjoy! 😊

## I. Crypto stack in Autosar 

This picture shows the location of Crypto stack in Autosar.
<figure>
  <img src="/assets/img/blogs/1.1/crypto_stack_in_autosar.png" alt="Crypto stack">
  <figcaption>Crypto stack in AUTOSAR</figcaption>
</figure>

As we can see, it also has 3 layers like other stacks:
> - Service layer (CSM): provides crypto services for application to call via RTE ports. Others BSW modules or CDD can also use crypto services by calling CSM APIs.

> - Hardware abstraction layer (CryIf): This layer provides channels for mapping crypto services to lower crypto drivers. This will make the access from upper layer independent to lower layer.

> - Crypto driver (Crypto): Each driver is a way to implement different crypto solution, such as Software libraries, hardware extensions, other custom algorithms based on specific requirements,...

For a more detail point of view, let's take a look at this picture.
<figure>
  <img src="/assets/img/blogs/1.1/crypto_stack_example.png" alt="Crypto stack">
  <figcaption>Crypto stack in detail</figcaption>
</figure>

When any component wants to use a crypto related functionality, it will make a call to CSM.

- The CSM provides several crypto services for application SWCs based on detail requirements of security. Multiple requests could be executed parallel by asynchronous processing mechanism. We will discuss about this right after this post. Also several instances of a same crypto service could be implemented in an ECU. 

> For example: 2 AES-128 encryption instances with different keys, one executed by software logic and the other by hardware acceleration. Each one is called from an application SWC.

- Some BSW components can also utilize crypto service. 

> SecOC might protects data between inter-ECU communication via Message authentication code (MAC). The CSM API for calculating MAC can be implemented by configuration tools like Davinci configurator, along with more detail algorithms at the lower layer (HMAC, CMAC,...)

> UDS service 0x29 (Authentication) - which can be handled by DCM - ensure that only valid tester can access critical expected services in order to prevent tampering attacks or unauthorized alteration of ECU data. The authentication method and data is forwarded to crypto stack for calculating.

> If CDD requires crypto service usage, the overall steps is similar to other components. Crypto stack callback function can be configured to notify status of a processing crypto job.

All of the above was just some very general, basic knowledge about Autosar crypto stack. More details are reserved when we dig deep into this series. In the next posts, I will try my best to explain each core component in the stack, also some interesting components that are not mandatory but have specific use cases such as HSM, SecMod, KeyM,... Stay tunes!