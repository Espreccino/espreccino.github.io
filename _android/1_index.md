---

layout: default
title: Getting Started
permalink: /android/index.html
collection: android
collection_title : Android
collection_sections:
  - {title: Installing, link: installing}
  - {title: Initialize Pepper Talk, link: initialize-pepper-talk}
  - {title: Initiate Chat, link: initiate-chat}
  - {title: Sending Custom Data, link: sending-custom-data}
  - {title: Callbacks, link: callbacks}
  - {title: Push Notifications, link: push-notifications}

---

## Installing
*Pepper Talk* is available as a gradle dependency. To pull in the dependency please add the following to your build.gradle

```groovy
dependencies {
    ...
    compile 'com.espreccino:peppertalk:0.4.14'
}
```

## Initialize Pepper Talk

```java
 PepperTalk.getInstance(context)
                .init(clientId,
                        clientSecret,
                        userId)
                .connect();
```

## Initiate Chat

```java
PepperTalk.getInstance(context)
                    .chatWithParticipant(userId)
                    .topicId(topicId)
                    .topicTitle("Let ride!")
                    .start();
```

## Sending Custom Data


```java
  JSONObject object = new JSONObject();
  object.put("hello", "its me!");
  PepperTalk.getInstance(getActivity())
          .sendCustomData(toUser,
                  text,
                  topicId,
                  topicTitle,
                  object, new JSONCallback() {
                      @Override
                      public void onSuccess(JSONObject jsonObject) {
                        ...
                      }
                      @Override
                      public void onFail(PepperTalkError error) {
                        ...
                      }
                  });
```

## Callbacks
### MessageListener

This callback is triggered whenever a message arrives for the user. Please note that the callback is invoked on a background thread and you will have to switch to the UI thread to do any UI updates.

```java
    /**
     * Message Listener
     * New incoming.
     * Ideally used to update unread count on the UI
     */
    public interface MessageListener {
        public void onNewMessage(String userId, String topicId, int unreadCount);
    }
```

### ConnectionListener

This callback is triggered to notify the host application of the connection status.

```java
    /**
     * Listen to Peppertalk connection status
     */
    public interface ConnectionListener {
        public void onConnecting(int status);
        public void onConnected();
        public void onConnectionFailed(PepperTalkError e);
    }
```

### CustomDataListener


## Push Notifications

## 