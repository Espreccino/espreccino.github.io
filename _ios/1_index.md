---

layout: default
title: Getting Started
permalink: /ios/index.html
collection: ios
collection_title : iOS
collection_sections:
  - {title: Getting Started, link: getting-started}
  - {title: Requirements, link: requirements}
  - {title: SDK Integration, link: sdk-integration}

---

<a id="gettingstarted"></a> 
##Getting Started
The primary purpose of the SDK is to allow developers to embed in-app chat messaging in iOS applications.

<a id="requirements"></a> 
## Requirements
This SDK is compatible with applications running on devices with iOS 7.0 or higher.

<a id="sdkintegration"></a> 
## SDK Integration
### Download SDK
1. Manual 
    * Download [PepperTalk.embeddedframework](https://github.com/Espreccino/PepperTalkSDK-iOS/tree/master/PepperTalk.embeddedframework) framework and [Dependencies Resources](https://github.com/Espreccino/PepperTalkSDK-iOS/tree/master/Dependencies%20Resources) folder from github
    * Move the content into your project directory. We usually put 3rd-party code into a subdirectory named `Vendor`
  
2. Cocoapods
    * If Cocoapods not installed, refer [Cocoapods Guide](http://guides.cocoapods.org/using/getting-started.html#installation) to install Cocoapods
    * Add "pod PepperTalkSDK-iOS" to your podfile
    * Run pod install
  
### Xcode Setup (Skip this step if using CocoaPods)

1. Drag & drop `PepperTalk.embeddedframework` from your project directory to your Xcode project.

2. Similar to above, our projects have a group `Vendor`, so we drop it there.

3. Select `Create groups for any added folders` and set the checkmark for your target. Then click `Finish`.

4. Select your project in the `Project Navigator` (⌘+1).

5. Select your app target.

6. Select the tab `Build Phases`.

7. Expand `Link Binary With Libraries`.

8. Add the following system frameworks, if they are missing:
    - `Accelerate`
    - `AssetsLibrary`
    - `CoreData`
    - `CoreGraphics`
    - `CoreImage`
    - `CoreLocation`
    - `Foundation`
    - `ImageIO`
    - `MapKit`
    - `MessageUI`
    - `MobileCoreServices`
    - `QuartzCore`
    - `Security`
    - `SystemConfiguration`
    - `UIKit`

9. Add the following libraries, if they are missing:
    - `libicucore.dylib`

10. Select the tab `Build Settings`.

11. Add -ObjC to Build Settings->Other Linker Flags if already not present

### Generate ClientID and Client Secret
Generate Client ID & Client Secret to authenticate your application with PepperTalk. Follow these steps to generate Client ID & Client Secret:
* Go to [PepperTalk Console](https://console.getpeppertalk.com/dashboard/signup)
* Fill in the details and signup
* Validate your email address by clicking on the link you get in your email inbox
* Create a new application by selecting the "New Application" option from the menu on left hand side
* Enter the Application Description
* Optionally, select and enter push notification related information to support remote OS notifications
* Find Client ID & Client Secret in the 'Clients' tab on your application page

### Modify Code 

#### Objective-C
1. Open your `AppDelegate.m` file.

2. Add the following line at the top of the file below your own #import statements:

        #import <PepperTalk/PepperTalkSDK.h>

3. Search for the method `application:didFinishLaunchingWithOptions:`

4. Add the following lines to setup PepperTalk:

        [PepperTalk sharedInstance].clientId = @"YOUR_CLIENT_ID";
        [PepperTalk sharedInstance].clientSecret = @"YOUR_CLIENT_SECRET";
        [[PepperTalk sharedInstance] enableInAppNotificationsInViewController:self.window.rootViewController]; //To enable in-app notifications

5. To enable Remote Notifications
    * Enable remote notifications as described in [Apple Documentation](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/IPhoneOSClientImp.html#//apple_ref/doc/uid/TP40008194-CH103-SW2)
    * Search for method `application:didRegisterForRemoteNotificationsWithDeviceToken`
    * Add following line of code
      * `[PepperTalk sharedInstance].deviceToken = deviceToken;`
    * Search for method `application:didReceiveRemoteNotification:`
    * Add following line of code
      * `[[PepperTalk sharedInstance] handleRemoteNotification:userInfo presentingViewController:self.window.rootViewController];`

6. Starting with iOS 8, to enable location sharing in PepperTalk, you must set a string for the key `NSLocationWhenInUseUsageDescription` or `NSLocationAlwaysUsageDescription` in your app's Info.plist file. For more information refer [Apple Documentation](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18). If neither of the keys are found in the client's Info.plist file, then the location sharing opiton will not be available.

