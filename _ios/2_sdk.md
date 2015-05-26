---

layout: default
title: SDK
permalink: /ios/sdk.html
collection: ios
collection_title : iOS
collection_sections:
  - {title: Glossary, link: glossary}
  - {title: Authentication, link: authentication}
  - {title: 1-1 Chat, link: 1-1-chat}
  - {title: Group Chat, link: group-chat}
  - {title: Custom Data Support, link: custom-data-support}
  - {title: Real-Time Peer-To-Peer Data Stream, link: real-time-peer-to-peer-data-stream}
  - {title: Push Notifications, link: push-notifications}
  - {title: In-App Notifications, link: in-app-notifications}

---

<a id="glossary"></a> 
##Glossary
* Current Logged In User  
A user can become current logged in user in PepperTalk. Any outgoing chat message will be sent on behalf of this user.
* Topics  
Pepper Talk exposes a topic for having multiple threads of conversations with the same user. A topic id can be any string that you choose. For example a listing app could use the id of the listing as the topic id. This would help the app to organize the chats among the same set of participants with a logical entity in the app. A topic title is also supported to give a descriptive value for the topic id.

<a id="authentication"></a> 
##Authentication
###Client Authentication
PepperTalk authenticates client apps. Client Authentication is done by setting the ClientId & Client Secret. A client app cannot use PepperTalk SDK without correct ClientID and Client Secret.

####Related API
	/** Pass clientId to identify the client app */
	@property (nonatomic, copy) NSString *clientId;

	/** Pass clientSecret to authorize the client app to use PepperTalk SDK*/
	@property (nonatomic, copy) NSString *clientSecret;
####Code Snippet
	PepperTalk *pepperTalkInstance = [PepperTalk sharedInstance];
    pepperTalkInstance.clientId = @"CLIENT_ID";
    pepperTalkInstance.clientSecret = @"CLIENT_SECRET";

###User Authentication
PepperTalk does not authenticate users. Since we only accept requests from authenticated client app, Login/Signup request for any user from the client app is assumed to be authorized and for an authenticated user.  

A user become current logged in user in PepperTalk by providing few details such as username & profile information. Once these details are provided PepperTalk will remember the current logged in user forever and will only log the user our of PepperTalk when explicitly asked to do so.

Profile Information is optional & can be updated later on.

####Related API
	/**
	 Initialise the SDK with logged in user's details. The SDK cannot be used used until initialisation passes successfully.
	 
	 @param username The logged in user's username
	 @param fullName The logged in user's full name
	 @param profilePicture The logged in user's profile picture url
	 @param completion Completion callback with results of operation
	 @return If operation could not be completed, it returns the error. Nil if the operation could complete
	 */
	- (NSError *) initialiseWithUsername:(NSString *)username
	                            fullName:(NSString *)fullName
	                      profilePicture:(NSString *)profilePicture
	                          completion:(void(^)(NSError *err))completion;
####Code Snippet
	NSError *err = [[PepperTalk sharedInstance] initialiseWithUsername:@"USERNAME" fullName:@"DISPLAY_NAME" profilePicture:@"DISPLAY_PIC_URL" completion:^(NSError *loginError) {
    
        if(loginError) {
            //Could not login succesfully
        }
    }];
    if(err) {
        //Could not complete the login request
    }

<a id="onetoonechat"></a> 
##1-1 Chat
### Initiate 1-1 Chat
####Related API
	/**
	 Present chat session view modally
	
	 @param participant Pass username of the user with whom chat session is to be initiated
	 @param topicId an id for the topic
	 @param topicTitle title for the topic, this will be displayed on the chat window, so ensure it contains a meaningful text representation of the topic
	 @param presentingViewController Pass the view controller which will be used to present the chat session modally
	 @return If operation could not be completed, it returns the error. Nil if the operation could complete successfully
	 */

	- (NSError *) presentChatSessionWithParticipant:(NSString *)participant
                                        topicId:(NSString *)topicId
                                     topicTitle:(NSString *)topicTitle
                       presentingViewController:(UIViewController *)presentingViewController;

	/**
 	 Create and return chat session view
	 
 	 @param participant Pass username of the user with whom chat session is to be initiated
	 @param topicId an id for the topic
	 @param topicTitle title for the topic, this will be displayed on the chat window, so ensure it contains a meaningful text representation of the topic
	 @param error If an error occurs, the error parameter will be set and the return value will be nil.
	 @return An instance of UIViewController which can be shown on screen in any way.
	 */
	- (UIViewController *) chatSessionWithParticipant:(NSString *)participant
                                          topicId:(NSString *)topicId
                                       topicTitle:(NSString *)topicTitle
                                            error:(NSError **)error;

####Code Snippet
	//Present Chat Session in navigation view stack
	UIViewController *chatSessionVC = [pepperTalkInstance chatSessionWithParticipant:@"PARTICIPANT_USERNAME"  topicId:@"SOME_TOPIC_ID" topicTitle:@"SOME_TOPIC_TITLE" error:NULL];
	[self.navigationController pushViewController:chatSessionVC animated:YES];
	
	//Present chat session modally
	[pepperTalkInstance presentChatSessionWithParticipant:@"PARTICIPANT_USERNAME" topicId:@"SOME_TOPIC_ID" topicTitle:@"SOME_TOPIC_TITLE" presentingViewController:self];

<a id="groupchat"></a> 
##Group Chat
###Create Group
####Related API
	/**
	 Create a new group
	 
	 @param groupId The id which you would like to use for the new group. It should contain a prefix 'grp:'. E.g 'grp:usc_friends'
	 @param groupName The name you would like to use for the group "USC College Friends"
	 @param profilePicture The groups profile picture url
	 @param members Array of member's user ids.
	 @param completion Completion callback with results of operation
	 @return If operation could not be completed, it returns the error. Nil if the operation could complete successfully
	 */
	- (NSError *) createGroupWithId:(NSString *)groupId
	                           name:(NSString *)groupName
	                 profilePicture:(NSString *)profilePicture
	                        members:(NSArray *)members
	                     completion:(void(^)(NSDictionary *groupInfo, NSError *err))completion;
	                    
###Create Public Group
####Related API
	/**
	 Create a new public group. Public groups are open to all the users and anybody can join the group.
	 
	 @param groupId The id which you would like to use for the new group. It should contain a prefix 'grp:'. E.g 'grp:usc_friends'
	 @param groupName The name you would like to use for the group "USC College Friends"
	 @param profilePicture The groups profile picture url
	 @param members Array of member's user ids.
	 @param completion Completion callback with results of operation
	 @return If operation could not be completed, it returns the error. Nil if the operation could complete successfully
	 */
	- (NSError *) createPublicGroupWithId:(NSString *)groupId
	                                 name:(NSString *)groupName
	                       profilePicture:(NSString *)profilePicture
	                              members:(NSArray *)members
	                           completion:(void(^)(NSDictionary *groupInfo, NSError *err))completion;

###Update Group Info
####Related API
	/**
	 Update group properties
	 
	 @param groupId The id of the group which you would like to update
	 @param newName Set newName if you would like to update name of the group. If set to nil, will not update the name
	 @param newProfilePicture Set profilePicture if you would like to update profile pic of the group. If set to nil, will not update the profile pic.
	 @param completion Completion callback with results of operation
	 @return If operation could not be completed, it returns the error. Nil if the operation could complete successfully
	 */
	- (NSError *) updateGroupWithId:(NSString *)groupId
	                           name:(NSString *)newName
	                 profilePicture:(NSString *)newProfilePicture
	                     completion:(void(^)(NSDictionary *groupInfo, NSError *err))completion;

###Add Group Members
####Related API
	/**
	 Add members to group
	 
	 @param groupId The id of the group which you would like to update
	 @param newMembers Pass array of user ids which are to be added as group members
	 @param completion Completion callback with results of operation
	 @return If operation could not be completed, it returns the error. Nil if the operation could complete successfully
	 */
	- (NSError *) addMembers:(NSArray *)newMembers
	           toGroupWithId:(NSString *)groupId
	              completion:(void(^)(NSDictionary *groupInfo, NSError *err))completion;

###Remove Group Members
####Related API
	/**
	 Remove members from group
	 
	 @param groupId The id of the group which you would like to update
	 @param membersToBeRemoved Pass array of user ids which are to be removed as group members
	 @param completion Completion callback with results of operation
	 @return If operation could not be completed, it returns the error. Nil if the operation could complete successfully
	 */
	- (NSError *) removeMembers:(NSArray *)membersToBeRemoved
	            fromGroupWithId:(NSString *)groupId
	                 completion:(void(^)(NSDictionary *groupInfo, NSError *err))completion;

###Delete Group
####Related API
	/**
	 Delete group
	 
	 @param groupId The id of the group which you would like to update
	 @param completion Completion callback with results of operation
	 @return If operation could not be completed, it returns the error. Nil if the operation could complete successfully
	 */
	- (NSError *) deleteGroupWithId:(NSString *)groupId
	                     completion:(void(^)(NSDictionary *groupInfo, NSError *err))completion;

<a id="customdatasupport"></a> 
##Custom Data Support
PepperTalk provides support for client apps to send custom data inside chat stream and to render it inside bubble interface on chat view.

###Send Custom Data
####Related API
	/**
	 Use this method to send custom message to any participant user or group in chat stream. The client on other end can then show a custom view inside the chat interface.
	 Max allowed size for custom data is 2KB.
	 @param customData Data to be passed to participant client. Only supported format is JSON
	 @param notificationText Text that will be shown in remote notifications. If text is set to nil, notification will not be sent out.
	 @param participant The participant whose clients should get the callback with the custom data
	 @param options Pass additional configurable options for the chat session. Currently we only support PTSessionOption_TopicId for this call.
	 @param completion Completion callback with results of operation
	 @return If operation could not be completed, it returns the error. Nil if the operation could complete.
	 */
	- (NSError *) sendCustomDataInChatStream:(NSDictionary *)customData
	                                 withText:(NSString *)notificationText
	                            toParticipant:(NSString *)participant
	                            sessionOptons:(NSDictionary *)options
	                               completion:(void(^)(NSError * err))completion;

###Render Custom Data On Other End
In order to render custom view for custom data, client app should set an object, that implement PTChatSessionCustomDataRendererProtocol, as customDataRenderer in globalConfigurator. The object will be issued calls to provide view which is to be rendered for a given custom data.
####Related API

##### To Set Object As Renderer. See PTChatSessionGlobalConfigurationProtocol
	/** Set a custom renderer object which confirms to PTChatSessionCustomDataRendererProtocol in order
	 to provide view for custom data in chat stream
	 */
	@property (assign, nonatomic) id<PTChatSessionCustomDataRendererProtocol> customDataRenderer;

##### To render view for given custom data. See PTChatSessionCustomDataRendererProtocol
	/** Actual view to be shown inside chat stream for given customData 
	 @param customData customData passed by client on the other end in chat stream
	 @return UIView which is to be rendered inside chat stream for given custom data
	 */
	- (UIView *) viewForMessageWithCustomData:(NSDictionary *)customData;
####Code Snippet
	[PepperTalk sharedInstance].globalConfigurator.customDataRenderer = self;

<a id="datastreamsupport"></a> 
##Real-Time Peer-To-Peer Data Stream
PepperTalk offers functionality to establish real time peer-peer data stream between clients.
####Related API
	/**
	 Use this method to send custom message to any participant user or group. The client at the other end will get a notification PTReceivedCustomDataNotification. The notification info can be queried for custom data, sender and timestamp. Use PTReceivedCustomDataNotification_CustomDataKey, PTReceivedCustomDataNotification_FromKey, PTReceivedCustomDataNotification_TimestampKey respectively to query.
	 Max allowed size for custom data is 2KB.
	 
	 @param customData Data to be passed to participant client. Only supported format is JSON
	 @param notificationText Text that will be shown in remote notifications. If text is set to nil, notification will not be sent out.
	 @param participant The participant whose clients should get the callback with the custom data
	 @param options Pass additional configurable options for the chat session. Currently we only support PTSessionOption_TopicId for this call.
	 @param completion Completion callback with results of operation
	 @return If operation could not be completed, it returns the error. Nil if the operation could complete
	 */
	- (NSError *) sendCustomData:(NSDictionary *)customData
	                    withText:(NSString *)notificationText
	               toParticipant:(NSString *)participant
	               sessionOptons:(NSDictionary *)options
	                  completion:(void(^)(NSError * err))completion;

	/**
	 Notification with name PTReceivedCustomDataNotification is posted to notify observers about incoming custom data. Notification userInfo has the following format:
	 {
	     PTReceivedCustomDataNotification_MsgDataKey : {
	        PTReceivedCustomDataNotification_MsgData_ToKey : # custom data receipient id
	        PTReceivedCustomDataNotification_MsgData_FromKey : # custom data sender id
	        PTReceivedCustomDataNotification_MsgData_TimestampKey : # custom data timestamp in epoch milliseconds
	        PTReceivedCustomDataNotification_MsgData_TopicIdKey : # topic id
	        PTReceivedCustomDataNotification_MsgData_TopicTitleKey : topic title
	     }
	     PTReceivedCustomDataNotification_CustomDataKey : # actual custom data from other end in form of NSDictionary
	 }
	 */
	extern NSString *const PTReceivedCustomDataNotification;
	extern NSString *const PTReceivedCustomDataNotification_MsgDataKey;
	extern NSString *const PTReceivedCustomDataNotification_MsgData_ToKey;
	extern NSString *const PTReceivedCustomDataNotification_MsgData_FromKey;
	extern NSString *const PTReceivedCustomDataNotification_MsgData_TimestampKey;
	extern NSString *const PTReceivedCustomDataNotification_MsgData_TopicIdKey;
	extern NSString *const PTReceivedCustomDataNotification_MsgData_TopicTitleKey;
	extern NSString *const PTReceivedCustomDataNotification_CustomDataKey;

<a id="pushnotifications"></a> 
##Push Notifications
PepperTalk offers Remote Push Notification Support.  
For Server Side Configurations check server documentation
###Register To Get Device Token
Check [Apple Documentation](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/IPhoneOSClientImp.html#//apple_ref/doc/uid/TP40008194-CH103-SW2) on how to register for remote notification

###Share Device Token With PepperTalk
####Related API
	/** To receive remote notifications, pass deviceToken to PepperTalk*/
	@property (nonatomic, copy) NSData *deviceToken;
####Code Snippet
	- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
	    
	    [PepperTalk sharedInstance].deviceToken = deviceToken;
	}

###Pass Notification to PepperTalk
####Related API
	/**
	 Forward the received remote notification to PepperTalk to be handled by us
	 
	 @param notification The dictionary containing the received notification.
	 @param presentingViewController Pass the view controller which will be used to present handled notification
	 @return If notification is handled by PepperTalk, returns YES else returns NO
	 */
	- (BOOL) handleRemoteNotification:(NSDictionary *)notification
	         presentingViewController:(UIViewController *)presentingViewController;
####Code Snippet
	- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {
	    
	    [[PepperTalk sharedInstance] handleRemoteNotification:userInfo presentingViewController:self.window.rootViewController];
	}

<a id="inappnotifications"></a> 
##In-App Notifications
PepperTalk provides support for in-app notifications. in-app notifications are a good way to get user's attention to a new incoming message. By default its turned off.
####Related API
	/**
	 Use this method to enable in-app notification. Disabled by default
	 
	 @param presentingViewController Pass the view controller which will be used to present in-app notifications. PepperTalk will retain the presentingViewController. Disable in-app notifications to release presentingViewController
	 */
	- (void) enableInAppNotificationsInViewController:(UIViewController *)presentingViewController;
	
	/**
	 Use this method to disable in-app notification.
	 */
	- (void) disableInAppNotifications;
####Code Snippet
	//To enable in-app notifications
	NSError *err = [[PepperTalk sharedInstance] initialiseWithUsername:@"USERNAME" fullName:@"DISPLAY_NAME" profilePicture:@"DISPLAY_PIC_URL" completion:^(NSError *loginError) {
        
        	[pepperTalkInstance enableInAppNotificationsInViewController:sharedInstance.window.rootViewController];
	}
	
	//To disable in-app notifications
	[pepperTalkInstance disableInAppNotifications];

<a id="uicustomizations"></a> 
##UI Customization
PepperTalk lets client app customize & skin the UI of the chat session.

###Chat Bubbles Color
####Related API
	/** Set outgoingBubbleColor to customize the outgoing message's bubble color */
	@property (strong, nonatomic) UIColor *outgoingBubbleColor;

	/** Set incomingBubbleColor to customize the incoming message's bubble color */
	@property (strong, nonatomic) UIColor *incomingBubbleColor;

####Code Snippet
	[PepperTalk sharedInstance].globalConfigurator.outgoingBubbleColor = [UIColor redColor];
	[PepperTalk sharedInstance].globalConfigurator.incomingBubbleColor = [UIColor blueColor];
###Navigation Bar Color
####Related API

	/** Set action toolbar's barTintColor */
	@property (strong, nonatomic) UIColor *actionToolbarBarTintColor;

	/** Set action toolbar's tint color */
	@property (strong, nonatomic) UIColor *actionToolbarTintColor;

####Code Snippet
	[PepperTalk sharedInstance].globalConfigurator. actionToolbarBarTintColor = [UIColor redColor];
	[PepperTalk sharedInstance].globalConfigurator. actionToolbarTintColor = [UIColor redColor];

###Navigation Bar Toolbar Buttons
####Related API
	/** Set action toolbar's custom buttons. Pass NSArray of UIToolBarButtonItems */
	@property (strong, nonatomic) NSArray *actionToolbarButtons;
####Code Snippet
	UIBarButtonItem *buttonItem1 = [[UIBarButtonItem alloc] initWithTitle:@"test1" style:UIBarButtonItemStylePlain target:self action:@selector(test1buttonpressed:)];
	UIBarButtonItem *buttonItem2 = [[UIBarButtonItem alloc] initWithTitle:@"test2" style:UIBarButtonItemStylePlain target:self action:@selector(test2buttonpressed:)];
	[PepperTalk sharedInstance].globalConfigurator.actionToolbarButtons = @[buttonItem1, buttonItem2];

<a id="photolocationsharing"></a> 
##Photo and Location Sharing Via Chat
Photo & Location sharing is supported by PepperTalk out of box.
###Photo Sharing
PepperTalk will request permission to access Photos app. If permission is denied then photo sharing from Photos app wont be available.

###Location Sharing
Starting with iOS 8, to enable location sharing in PepperTalk, you must set a string for the key `NSLocationWhenInUseUsageDescription` or `NSLocationAlwaysUsageDescription` in your app's Info.plist file. For more information refer [Apple Documentation on Location Keys](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18). If neither of the keys are found in the client's Info.plist file, then the location sharing opiton will not be available 

<a id="participantslist"></a> 
##Participants List
We provide out of box UI for participants list. The participants list can be filtered to show participants for certain topics only.
####Related API
	/**
	 Show list of participants with whom you have had conversation for this topic
	 
	 @param topicIds List of topicsIds which are to be applied as filter while showing participants.
	 Pass nil to disable filter
	 @param presentingViewController Pass the view controller which will be used to present handled notification
	 @return If operation could not be completed, it returns the error. Nil if the operation could complete successfully
	 */
	- (NSError *) presentAllParticipantsFilterByTopics:(NSArray *)topicIds
	                      presentingViewController:(UIViewController *)presentingViewController;

	/**
	 Get view which shows list of participants with whom you have had conversation for this topic
	 
	 @param topicIds List of topicsIds which are to be applied as filter while showing participants.
	 Pass nil to disable filter
	 @param error If an error occurs, the error parameter will be set and the return value will be nil.
	 @return If operation could not be completed, it returns nil. View if the operation could complete successfully
	 */
	- (UIViewController *) allParticipantsFilterByTopics:(NSArray *)topicIds error:(NSError **)error;

	/**
	 Get list of participants with whom you have had conversation for these topics
	 
	 @param topicIds List of topicsIds which are to be applied as filter while showing participants.
	 Pass nil to disable filter
	 @param error If an error occurs, the error parameter will be set and the return value will be nil.
	 @return If operation could not be completed, it returns nil. If operation succesfull find information in this format
	 {
	     # id of topic : {
	         users : {
	             # id of participant : {
	                 count   : # total messages in this topic for this user
	                 unread  : # total unread messages within those messages
	                 last_message_timestamp: # unix timestamp in millis for the last message in them
	                 user_name : # user name
	                 user_id   : # id of the user
	             }
	         }
	         topic_title : # description of the topic
	         topic_id    : # id of the topic
	     }
	 }
	 */
	- (NSDictionary *) allParticipantsSummaryFilterByTopics:(NSArray *)topicIds error:(NSError **)error;

<a id="topicslist"></a> 
##Topics List
We provide out of box UI for topics list. The topics list can be filtered to show topics for certain participants only.
####Related API
	/**
	 Show list of topics under which you have had conversation with participant
	 
	 @param participants List of participants which are to be applied as filter while showing topics.
	 Pass nil to disable filter
	 @param presentingViewController Pass the view controller which will be used to present handled notification
	 @return If operation could not be completed, it returns the error. Nil if the operation could complete successfully
	 */
	- (NSError *) presentAllTopicsFilterByParticipants:(NSArray *)participants
	                      presentingViewController:(UIViewController *)presentingViewController;

	/**
	 Get view which shows list of topics under which you have had conversation with participant
	 
	 @param participants List of participants which are to be applied as filter while showing topics.
	 Pass nil to disable filter
	 @param error If an error occurs, the error parameter will be set and the return value will be nil.
	 @return If operation could not be completed, it returns nil. View if the operation could complete successfully
	 */
	- (UIViewController *) allTopicsFilterByParticipants:(NSArray *)participants error:(NSError **)error;

	/**
	
	 Get list of topics under which you have had conversation with these participants
	 
	 @param participants List of participants which are to be applied as filter while showing topics.
	 Pass nil to disable filter
	 @param error If an error occurs, the error parameter will be set and the return value will be nil.
	 @return If operation could not be completed, it returns nil. If operation succesfull find information in this format
	 {
	     # id of user : {
	         topics : {
	             # id of topic : {
	                 count   : # total messages in this topic for this user
	                 unread  : # total unread messages within those messages
	                 last_message_timestamp: # unix timestamp in millis for the last message in them
	                 topic_title : # description of the topic
	                 topic_id    : # id of the topic
	             }
	         }
	         user_name : # user name
	         user_id   : # id of the user
	     }
	 }
	 */
	- (NSDictionary *) allTopicsSummaryFilterByParticipants:(NSArray *)participants error:(NSError **)error;

<a id="unreadcount"></a> 
##Unread Count
###Unread Count For Conversation With A Participant
####Related API
	/**
	 Get unread notifications count for a participant
	 
	 @param participant The participant whose unread notifications count is to be returned
	 @param topicIds List of topics which are to be applied as filter while showing unread notifications count. Pass nil to disable filter
	 @return Nil if the operation could not complete successfully. If operation succesfull, information returned is in following format:
	 {
	     PTUnreadCountQueryTopicsKey : {
	         # topic id : # unread count for the topic,
	         # topic id : # unread count for the topic,
	     }
	     PTUnreadCountQueryTotalUnreadCountKey : # total unread count across all topics
	 }
	 */
	- (NSDictionary *) unreadNotificationCountForParticipant:(NSString *)participant
	                                          filterByTopics:(NSArray *)topicIds;
                                          
###Unread Count For Conversation Under A Topic
####Related API
	/**
	 Get unread notifications count for a topic
	 
	 @param topicId The topicId whose unread notifications count is to be returned
	 @param participants List of participants which are to be applied as filter while showing unread notifications count. Pass nil to disable filter
	 @return Nil if the operation could not complete successfully. If operation succesfull, information returned is in following format:
	 {
	    PTUnreadCountQueryParticipantsKey : {
	        # participant id : # unread count for the participant,
	        # participant id : # unread count for the participant,
	    }
	    PTUnreadCountQueryTotalUnreadCountKey : # total unread count across all participants
	 }
	 */
	- (NSDictionary *) unreadNotificationCountForTopic:(NSString *)topicId
	                              filterByParticipants:(NSArray *)participants;

<a id="blockunblockparticipants"></a> 
##Block & Unblock Participants
###Block Participants
####Related API
	/**
	 Block a list of participants, users or groups. Once a participant is blocked none of the messages from that participant will be delivered to the user. If the participant is a group all messages to the group sent by anybody will be dropped.
	 
	 @param participantsToBeBlocked Pass array of participants which are to be blocked
	 @param completion Completion callback with results of operation
	 @return If operation could not be completed, it returns the error. Nil if the operation could complete successfully
	 */
	- (NSError *) blockParticipants:(NSArray *)participantsToBeBlocked
	                     completion:(void(^)(NSDictionary *userInfo, NSError *err))completion

###Unblock Participants
####Related API
	/**
	 Unblock a list of participants, users or groups.
	 
	 @param participantsToBeBlocked Pass array of participants which are to be unblocked
	 @param completion Completion callback with results of operation
	 @return If operation could not be completed, it returns the error. Nil if the operation could complete successfully
	 */
	- (NSError *) unblockParticipants:(NSArray *)participantsToBeUnblocked
	                       completion:(void(^)(NSDictionary *userInfo, NSError *err))completion

<a id="muteunmuteparticipants"></a> 
##Mute & Unmute Participants
###Mute Participants
####Related API
	/**
	 Mute a list of participants, users or groups. Once a participant is muted none of the messages from that participant will be notified, messages will be delivered to the users but remote notifications will be disabled.
	 
	 @param participantsToBeMuted Pass array of participants which are to be muted
	 @param completion Completion callback with results of operation
	 @return If operation could not be completed, it returns the error. Nil if the operation could complete successfully
	 */
	- (NSError *) muteParticipants:(NSArray *)participantsToBeMuted
	                    completion:(void(^)(NSDictionary *userInfo, NSError *err))completion

###Unmute Participants
####Related API
	/**
	 Unmute a list of participants, users or groups.
	 
	 @param participantsToBeUnmuted Pass array of participants which are to be unmuted
	 @param completion Completion callback with results of operation
	 @return If operation could not be completed, it returns the error. Nil if the operation could complete successfully
	 */
	- (NSError *) unmuteParticipants:(NSArray *)participantsToBeUnmuted
	                      completion:(void(^)(NSDictionary *userInfo, NSError *err))completion
