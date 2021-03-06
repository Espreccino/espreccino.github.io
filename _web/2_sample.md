---

layout: default
title: Sample Application
permalink: /web/sample.html
collection: web
collection_title : Web SDK

---

## Introduction
This is a sample app to demonstrate the JS SDK for Pepper Talk available on our [Pepper Talk Web SDK Example](https://github.com/Espreccino/PepperTalkWebSDKExample) Github repo. It contains both the server and the client components. Its built as an Express service for the server and Angular app for the client.

## Quick Start
* git clone *this repo*
* npm install
* bower install
* grunt
* Get the client id and client secret from the Pepper Talk dashboard
* Setup the following environment variables in your shell with client id and client secret
  * PEPPERTALK\_CLIENT\_ID
  * PEPPERTALK\_SECRET
* grunt server
* Access the app at http://localhost:8989/app/

Please refer to the accompanying [detailed documentation](http://espreccino.github.io/PepperTalkWebSDKExample/)
