---

layout: default
title: API
permalink: /REST/api.html
collection: REST
collection_title : REST
collection_sections:
  - {title: Base URL, link: base-url}
  - {title: Sending Messages, link: sending-messages}
  - {title: Group Management, link: group-management}
  - {title: Group Membership, link: group-membership}

---

## Base URL
All Pepper Talk resources are exposed behind SSL from the base URL **https://hostedpepper.getpeppertalk.com/api/v1/**. Resources are relative to this base URL.

## Sending Messages
Resource: **client/message/send**

Method: **POST**

Content-Type: **application/json**

Data:
{% highlight javascript linenos %}
{
  "text": "Test message",
  "to" : "receiver@yourdomain.com",
  "from": "sender@yourdomain.com"
  "topic_id": "352e723a-5f3f-455e-a1ee-548bac188ada",
  "topic_title": "A topic title",
  "images": [{
    "img_src": "https://maps.googleapis.com/maps/api/staticmap?center=22.39762,68.91713&zoom=6&size=430x300",
    "img_caption": "My Image"
  }],
  "locations": [{
      "name":"My current location",
      "location": {
        "lat":22.8946639,
        "lng":67.5970372
      },
      "map_thumbnail":"https://maps.googleapis.com/maps/api/staticmap?center=22.39762,68.91713&zoom=6&size=430x300",
  }],
  "custom_data": {
    "optional": true
  }
}
{% endhighlight %}

Parameters:

  * text - Message text, required field.
  * to - Recipient of the message, it could be a user id or a group id, required field
  * from - Sender of the message, overriden and set to the authenicated user 
  * topic_id - Optional, topic id for this message, use this to have threads of conversation with the same person
  * topic_title - Optional, a descriptive title for this topic
  * images - Optional array of images to be attached with this message, each array element should have these two fields
    * img_src - URL for the image
    * img_caption - Caption for the image
  * locations - Optional array of locations to be associated with this message, each location element is of the following format
    * name - description for this location
    * location - an object with
      * lat - lattitude of the location as a float
      * lng - longitude of the locaiton as a float
    * map_thumbnail - a thumbnail image to display for this location, ideally a Google Maps static image API link
  * custom_data - User defined data to be attached with this image, optional, any valid json element can be associated

Response:

    200 OK
    Content-Type: application/json
    {
      "status":"sent",
      "id":"6439ca0a-3234-4e52-a66a-dc41dbc4e6fe"
    }    

Error codes:

  * 401 - User does not exist
  * 403 - Token invalid

## Group Management
Resource: **client/group/:group_id**

### Group ID
A group id requires a prefix of **grp:** the rest of the group id can be any string. A group id should be unique for your app.

### Create a group
Method: **POST**

Content-Type: **application/json**

Data:
{% highlight javascript linenos %}
{
  "group_name": "Test group 1",
  "profile_photo": "http://cdn.yourdomain.com/images/group_profile.jpg",
  "group_members": ["member1@yourdomain.com", "member2@yourdomain.com", "member3@yourdomain.com"],
  "public_group" : false
  "open_group" : false
}
{% endhighlight %}

Parameters:

  * group_name - A descriptive name for the group
  * profile_photo - An image URL for use as the groups profile pic
  * group_members - An array of user ids who should be part of the group, the creator is added by default to the group
  * public_group - Optional, whether this is a public group
  * open_group - Optional, whether this is an open group

Response:

    200 OK
    Content-Type: application/json
    {
      "public_group":false,
      "profile_photo":"http://cdn.yourdomain.com/images/group_profile.jpg",
      "owner":"user@yourdomain.com",
      "open_group":false,
      "name":"Test group 1",
      "group_members":["member1@yourdomain.com", "member2@yourdomain.com", "member3@yourdomain.com", "user@yourdomain.com"],
      "group_id":"grp:test_group_1.1"
    }    

Error codes:

  * 400 - If group id is not valid
  * 401 - If group already exists
  * 403 - Token invalid

### Update a group properties
Method: **PUT**

Content-Type: **application/json**

Data:
{% highlight javascript linenos %}
{
  "group_name": "Test group 2",
  "profile_photo": "http://cdn.yourdomain.com/images/group_profile.jpg"
}
{% endhighlight %}

Parameters:

  * group_name - A descriptive name for the group
  * profile_photo - An image URL for use as the groups profile pic

Response:

    200 OK
    Content-Type: application/json
    {
      "public_group":false,
      "profile_photo":"http://cdn.yourdomain.com/images/group_profile.jpg",
      "owner":"user@yourdomain.com",
      "open_group":false,
      "name":"Test group 2",
      "group_members":["member1@yourdomain.com", "member2@yourdomain.com", "member3@yourdomain.com", "user@yourdomain.com"],
      "group_id":"grp:test_group_1.1"
    }    

Error codes:

  * 400 - If group id is not valid, user does not have access to group
  * 403 - Token invalid
  * 404 - If group does not exist


### Get a group information
Method: **GET**

Response:

    200 OK
    Content-Type: application/json
    {
      "public_group":false,
      "profile_photo":"http://cdn.yourdomain.com/images/group_profile.jpg",
      "owner":"user@yourdomain.com",
      "open_group":false,
      "name":"Test group 2",
      "group_members":["member1@yourdomain.com", "member2@yourdomain.com", "member3@yourdomain.com", "user@yourdomain.com"],
      "group_id":"grp:test_group_1.1"
    }    

Error codes:

  * 400 - If group id is not valid, user does not have access to group
  * 403 - Token invalid
  * 404 - If group does not exist

### Delete a group
Method: **DELETE**

Response:

    200 OK

Error codes:

  * 400 - If group id is not valid, user does not have access to group
  * 403 - Token invalid
  * 404 - If group does not exist


## Group Membership
Resource: **client/group/:group_id/members**

### Add members to a group
Method: **PUT**

Content-Type: **application/json**

Data:
{% highlight javascript linenos %}
{
  "group_members": ["member4@yourdomain.com", "member5@yourdomain.com"],
}
{% endhighlight %}

Parameters:

  * group_members - An array of user ids who should be added to the group

Response:

    200 OK
    Content-Type: application/json
    {
      "public_group":false,
      "profile_photo":"http://cdn.yourdomain.com/images/group_profile.jpg",
      "owner":"user@yourdomain.com",
      "open_group":false,
      "name":"Test group 1",
      "group_members":["member1@yourdomain.com", "member2@yourdomain.com", "member3@yourdomain.com", "user@yourdomain.com", "member4@yourdomain.com", "member5@yourdomain.com"],
      "group_id":"grp:test_group_1.1"
    }    

Error codes:

  * 400 - If group id is not valid, user does not have access to group
  * 403 - Token invalid
  * 404 - If group does not exist

### Remove members from a group
Method: **DELETE**

Content-Type: **application/json**

Data:
{% highlight javascript linenos %}
{
  "group_members": ["member4@yourdomain.com", "member5@yourdomain.com"],
}
{% endhighlight %}

Parameters:

  * group_members - An array of user ids who should be removed from the group

Response:

    200 OK
    Content-Type: application/json
    {
      "public_group":false,
      "profile_photo":"http://cdn.yourdomain.com/images/group_profile.jpg",
      "owner":"user@yourdomain.com",
      "open_group":false,
      "name":"Test group 1",
      "group_members":["member1@yourdomain.com", "member2@yourdomain.com", "member3@yourdomain.com", "user@yourdomain.com"],
      "group_id":"grp:test_group_1.1"
    }    

Error codes:

  * 400 - If group id is not valid, user does not have access to group
  * 403 - Token invalid
  * 404 - If group does not exist
