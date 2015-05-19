---

layout: default
title: F.A.Q
permalink: /faq/index.html
collection: faq
collection_title : F.A.Q
collection_sections:
  - {title: General, link: general}
  - {title: Technology, link: technology}
  
---

# Frequently Asked Questions
## General

* What is PepperTalk?

    Pepper Talk is an in-app messaging SDK. It allows you to have standard messaging features built right into your app. You can enable messaging features between your users or between a group of users.

* Does PepperTalk support groups?

    Yes, we support groups. Please [see groups](/#groups) under concepts.

* What features do I get with PepperTalk?

    Pepper Talk supports all standard messaging features
  
      * Messaging across multiple devices
      * 1-1 messaging
      * Group messaging
      * Image sharing
      * Location sharing
      * Message status (delivery, read receipts)
      * Typing indicators
      * Custom data sharing
      * Device specific push notifications
      * Customisable UI


* How does PepperTalk work inside my app?

    Pepper Talk SDK's are available for iOS, Android and Web. You integrate them into your app as per the platform specific instructions.

* Can I have the service hosted in my own servers? Why is it better to not host it myself?

    No, we provide this as a hosted service. We manage the servers for you and you get to benefit from our expertise.

* Does PepperTalk handle notifications for me?

    Yes platform specific push notifications are provided.

* Can I customise the stock UI that comes with PepperTalk?

    Yes, we allow customisation of various aspects of the interface.

* How to authenticate a user on PepperTalk? Can I use email/phonenumber for login purposes?

    Pepper Talk does not do any user management. Its teh apps responsibility to authenticate users. Once a user is authenticated you pass in the authenticated userid to Pepper Talk and the userid will be created on demand on Pepper Talk.

* Can I have same account on multiple devices? What about syncing?

    Yes very much. The same user can be logged into multiple devices. All the messages will be synced across all the devices on which a user is logged in.

* Does PepperTalk store any data?

    Pepper Talk stores data transiently, ie; until the message is delivered to the users device. After that the data is expired from our servers.

* Does PepperTalk provide a feed of all messages?

    We provide a real time feed of the messages and the activity on the messages. The messages are provided an HTTP stream. Please see [message stream](https://github.com/Espreccino/PepperTalkMessageStream)

* Can I share custom data on PepperTalk?

    Yes you can.
    
* Can I have multiple apps under my account?

    Yes you can have any number of apps under your account.

* Can multiple apps in my account talk to each other?

    No that is not possible. Each app can communicate only with the users on that app alone.

* How does PepperTalk ensure security of data?

    All communication with Pepper Talk servers are SSL secured. We store data for the least possible time on our servers.

* Does PepperTalk support chat history retrieval?

    Pepper Talk maintains minimal chat history for public groups conversations. No other conversations are archived and retreivable. Public groups archives are pushed to the users automatically on user joining a group.

* How much does it cost to get PepperTalk?

    Please see our [pricing](http://getpeppertalk.com/#pricing) plans.

* What are Monthly Active Users? How are they counted?

    A user who connects atleast once in the billing cycle is counted as an active user. The same user could connect from multiple devices or multiple times and would still be counted as a single user.

* How do I try PepperTalk?

    Check out the platform of your choice, download the sample and see it in action. 


## Technology

* What about battery usage?
* What about network usage?
* How much size does it add to my app?
* How are remote notifications handled?
* How much load/traffic can PepperTalk servers handle? What about scaling issues?
* What Device/Platforms are supported?
* Can the same device be used for multiple logins?
* What are topics? How do I use them?
* Where are the images that are shared stored on server side?
* Does PepperTalk have a REST API which servers can use to send chat messages on behalf of the user?
