---

layout: default
title: Getting Started
permalink: /REST/index.html
collection: REST
collection_title : REST
collection_sections:
  - {title: Introduction, link: introduction}
  - {title: Endpoints, link: endpoints}
  - {title: Authentication, link: authentication}
  - {title: Accessing Resources, link: accessing-resources}

---

## Introduction
Pepper Talk exposes most of the functionality via REST services for integration with you backends. These are available as standard JSON API's secured against your tokens via OAuth2. Available API's are documented in the [API](/REST/api.html) section.

## Endpoints
All Pepper Talk resources are exposed behind SSL from the base URL https://hostedpepper.getpeppertalk.com/api/v1/. Resources are relative to this base URL.

## Authentication
REST services are secured similar to the Web SDK. Assuming that you have an account on [Pepper Talk Console](https://console.getpeppertalk.com/), generate a set of credentials for your app which is Web Auth enabled. All API's work on behalf of a user. You are expected to generate a bearer token for a user on your system. To generate a bearer token for use on API calls use the sso grant type from the *get_access_token* resource. 

### get\_access\_token
Relative URL: **get\_access\_token**

Method: **POST**

Data:

{% highlight javascript linenos %}
{
  grant_type: 'sso'
  client_id: pepperKitClientId
  timestamp: 1427788129447 // UNIX timestamp with millisecond precision
  user_id: <<userid>>
  sso_token: sso_token
  display_name: <<users full name>> //optional
  profile_photo: <<users profile picture>> //optional
}
{% endhighlight %}

The SSO token is generated as follows

{% highlight javascript linenos %}
shasum(pepperKitClientId + ":" + pepperKitSecret + ":" + timestamp + ":" + userid)
{% endhighlight %}

Response:

Content-Type: application/json

{% highlight javascript linenos %}
{
  "access_token":"MDVGMDkwRDItQTJFNC00MTZFLTg4NkUtNkZCNENEQzg5MzgxOmxVVHBxWmgzdlkxSHVGTE13b09uRFJVSUw3T3FZa1Rl",
  "expires_in":90,
  "resource_owner": <<userid>>,
  "scope":"",
  "token_type":"bearer"
}
{% endhighlight %}

## Accessing Resources
To access any resource pass in the bearer token in the **Authorization** header like this.

    Authorization: Bearer MDVGMDkwRDItQTJFNC00MTZFLTg4NkUtNkZCNENEQzg5MzgxOmxVVHBxWmgzdlkxSHVGTE13b09uRFJVSUw3T3FZa1Rl

If the bearer tokens are invalid or have expired a 403 status is returned on all calls.