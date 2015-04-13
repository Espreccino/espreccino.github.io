---

layout: default
title: Pepper Talk

---
# Pepper Talk Documentation

## Platform
* [iOS](/ios)
* [Android](/android)
* [Web](/web)
* [REST](/REST)

# Pepper Talk Concepts

Here are some of the concepts that we use on a regular basis within Pepper Talk. Please familiarize yourself with these as we use them on a regular basis within the documentation.

## User

All the SDK's work on behalf of a user. When initialising the SDK you are expected to pass in the user id. We do not require you to seed your user base on Pepper Talk. All our SDK's auto provision users when the SDK is initialized on their behalf. We also do not require any special user authentication. Its the app's responsibility to authenticate the user, you are expected to initialize Pepper Talk once you are done with your app specific authentication flow.

When picking a user id for use with Pepper Talk make sure that it is easily discoverable in your app. Ideally map it to the exact user id on your app. When initiating a chat you are expected to pass in the user or group with whom the chat should be started. This would probably be triggered by the user clicking on some UI element for chatting with a user. Choose user ids which allow you to easily identify the other party with existing information in your screens.

Same user can be active on multiple devices across various supported platforms. A message intended for the user will be delivered to all the devices on which the user has been active. All SDK's ensure multi device support for actions like read receipts, so that a message marked as read on one device is automatically marked as read on other devices too.

## Groups

Pepper Talk supports group communication. A group is identified by string starting with **"grp:"**. You group name should be unique and again like the user name should be easily available when a user wants to chat with the group. A message sent to a group is delivered to all the members of the group.

Currently a group does not have a limit on the number of users who can be part of the group. We provide three types of groups.

### Closed Groups

A closed group is an administrator controlled group. The creator of the group is the group administrator. Only an administrator can add members to the group. Any member can choose to leave the group if the user so desires. An administrator can also remove any member of the group.

### Public Groups

A public group is one in which any user can join or leave when the user so decides. An administrator can also add or remove users from the group

### Open Group

An open group is one in which the members of the group are administrator controlled, but any user can send a message to this group and members of the group can choose to reply to such a message which gets delivered to all members of the group and the sender.

## Participant

Participant is either a user or a group. A message addressed to a group has the group as a participant whereas one addressed to an user has the user as the participant.

## Topics

A chat conversation with a participant can have multiple threads. These are referred to as topics. A topic can have a topic\_id and a topic\_title. id is a unique string which identifies the thread and the title is a descriptive text for the topic.