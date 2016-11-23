# Vidcoin ANE - QuickStart Guide

![Vidcoin](https://d3rud9259azp35.cloudfront.net/documentation/Vidcoin-Logo.png)

ANE version: 1.8.1					
Packaged iOS SDK version: 1.4.1					
Packaged Android SDK version: 1.3.1					
Manager: https://manager.vidcoin.com			
Contact: publishers@vidcoin.com			

## Overview						
Vidcoin ANE enables you to broadcast videos in your apps in order to give users access to restricted content or to obtain virtual currency for free.
This document and the archive contain all the information you need in order to integrate our solution and start broadcasting videos. You can also contact us directly at publishers@vidcoin.com.

## Account Settings						
An approved and valid publisher account is necessary in order to use the SDK. If you do not have an account, please sign up directly on [https://manager.vidcoin.com](https://manager.vidcoin.com).
This also grants you access to statistics and detailed reports in our Publishers Back-Office.

### App Creation
You can access App Creation in the “App” menu of your publisher area. The creation of an app generates an “App ID” which will be mandatory in order to use the Vidcoin web SDK.

### Placement Creation			
A Placement represents the zone in your app where the videos will be available for broadcasting. There are different settings available for any app Placement:			
- Minimum Payout: corresponds to the minimum revenue that a video must generate in order to be offered to a user. We recommend you to set "No minimum payout" to get more videos. We will automatically give you the highest pricing.			
- Test Mode: when “Test Mode” is activated, the Placement will always display videos for users, without any restrictions. Test mode placements do not generate any revenue.		

Basic configuration allows users to access a specific part of the app by watching a video. Once the advertisement's guaranteed duration is reached, you can unlock the content.		

If you want to reward your users with items or currency, you will have more settings available : 			
- Reward name: name of your virtual currency.		
- You can choose 2 types of rewards for the user:		
	- Conversion rate: rate defining how much 1€ equals of your virtual currency. Please note that we recommend that you use a low exchange rate such that 1€ = 1000 of your virtual currency. This will prevent any problems of crediting users with small amounts of virtual currency.	
	- Fixed reward: amount of virtual currency the user will be rewarded for 1 view.	
	- Callback URL (Optional): URL that will be used to validate the transaction on your servers.		

## Setting up the framework in your project

*Info: the iOS framework was built using iOS SDK 10.0, and requires iOS 7.0 or greater as minimum deployment target. The Android framework was built using API 21 and Java 1.6, and requires a deployment target API equal to or greater than 10 (GINGERBREAD R1).*

### Step 1 : Getting the ANE
Download and open the zip file from Github: [https://github.com/VidCoin/VidCoin-AdobeAir-ANE](https://github.com/VidCoin/VidCoin-AdobeAir-ANE)

### Step 2: Adding the ANE to the project

#### Copy the ANE file in your project
Navigate to `VidCoin-ANE-version/ANE/` and add the following `.ane` files to the project in a specific folder of your choice (named `extensions` for example):
* VidCoin.ane
* VidCoin-PlayServices.ane
* VidCoin-SupportV4.ane
The last two provide support for Android projects. If you’re only targetting iOS, you don’t need to include them.

#### Add the ANE to the build

##### In Flash Builder 4.6 or Higher
* Open the project properties
* Select Flex Build Path from the side bar
* Select Native Extensions tab
* Select the add ANE button
* Browse to the VidCoin.ane file and select it
* If you’re targetting Android, repeat the operation for the two support ANEs (`VidCoin-PlayServices.ane` and `VidCoin-SupportV4.ane`)		
* Add the ANE for iOS:
	* Select Flex Build Packaging from the side bar
	* Select Apple iOS
	* Select Native Extensions in the tabs
	* Check Vidcoin.ane in the ANE list
* Add the ANE for Android:
	* Select Flex Build Packaging from the side bar
	* Select Google Android
	* Select Native Extensions in the tabs
	* Check Vidcoin.ane in the ANE list
	* If you’re targetting Android, repeat the operation for the two support ANEs (VidCoin-PlayServices.ane and VidCoin-SupportV4.ane)

##### In Flash Professional CS6 or Higher
* Go to `File > Publish Settings`
* Select the wrench icon to 'Script' for 'ActionScript Settings'
	* Select the Library Path tab
	* Click 'Browse for Native Extension (ANE) File' and choose `Vidcoin.ane`
	* If you’re targetting Android, repeat the operation for the two support ANEs (VidCoin-PlayServices.ane and VidCoin-SupportV4.ane)
* Select the wrench icon to ‘Target’ for ‘Player Settings’
	* Select the 'Permissions’ tab
	* Check the 'Manually manage permissions and manifest additions for this app' box

#### Edit your application’s xml descriptor to add `Vidcoin.ane`
Add the following lines to your xml file:
```xml
<extensions>
    <extensionID>com.vidcoin.sdk</extensionID>
	<!-- Uncomment if your app targets Android
	<extensionID>com.vidcoin.extensions.android.PlayServices</extensionID>
	<extensionID>com.vidcoin.extensions.android.SupportV4</extensionID>
	-->
</extensions>
```

#### iPhone Specifications
In order to authorize the SDK to toggle in fullscreen mode, add the following text in the <InfoAdditions> tag in the application’s xml descriptor in the iPhone tag :
```xml
<key>UIViewControllerBasedStatusBarAppearance</key>
<true/>
```

#### Android Specifications
Add the following lines between the `<manifest>`:
```xml
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
<uses-permission android:name="android.permission.WAKE_LOCK"/>
<application android:allowBackup="true">
	<activity
		android:name="com.vidcoin.sdkandroid.extensions.MovieActivity"
		android:launchMode="singleTop"
		android:configChanges="orientation|screenSize"
		android:theme="@style/VC__AppTheme.NoActionBar.FullScreen" />
	<activity
		android:name="com.vidcoin.sdkandroid.extensions.WebViewActivity"
		android:exported="false"
		android:parentActivityName="com.vidcoin.sdkandroid.extensions.MovieActivity"
		android:theme="@style/VC__AppTheme.NoActionBar.FullScreen" >
		<meta-data
			android:name="android.support.PARENT_ACTIVITY"
			android:value="com.vidcoin.sdkandroid.extensions.MovieActivity" />
	</activity>
	<activity
		android:name="com.vidcoin.sdkandroid.extensions.FullScreenPlayerActivity"
		android:theme="@style/VC__AppTheme.NoActionBar.FullScreen" />
	<service 
		android:name="com.vidcoin.sdkandroid.core.MediaDownloadService"
		android:exported="false" />
	<meta-data
		android:name="com.google.android.gms.version"
		android:value="4452000" />
</application>
```

If you’re seeing conflicts between Vidcoin & your own dependencies, feel free to reach out to your account manager or publishers@vidcoin.com to get a specific ANE free from all conflictual dependencies.

### Step 3: Start the framework
The recommended way to start Vidcoin is to place a call to `startWithGameId(gameID:String)` as soon as possible. For example, you can do as follows:

```actionscript
import com.vidcoin.extension.ane.VidCoinController;

public var vidcoin : VidCoinController;

private function init() : void {
	vidcoin = new VidCoinController();
	vidcoin.startWithGameId("yourGameID");
}
```

### Step 4: Optional step: Enable logging
You can enable extra logging by making a call to the following function:

```as
public function setLoggingEnabled(b:Boolean):void
```

Example:*
```as
vidcoin.setLoggingEnabled(true);
```

*Advice: set the verbose tag to `false` before releasing your app.*

### Step 5: Update the user information
It’s really important you send us as much information on your user as you can, so we can target the ads appropriately. The easy way to do this is the following:
```as
public function updateUserDictionary(infos:Dictionary):void
```

You can pass any of these 3 key-value pairs, all keys and values being String objects.

| Key  | Values | Description |
| :-------------: | :-------------: | :-------------: |
| VCUserGameID | String | This is the string you use to identify the user in your app. This will be used in case you implement a server callback. |
| VCUserBirthYear | An NSString object like ”1980” | The birth year of the user. |
| VCUserGenderKey | VCUserGenderMale or VCUserGenderFemale | A string to specify the user’s gender. Any other value will be ignored. |

*Example:*
```as
var dict:Dictionary = new Dictionary();
dict[VidCoinController.VCUserGameID] = "username";
dict[VidCoinController.VCUserBirthYear] = “1980”;
dict[VidCoinController.VCUserGenderKey] = VidCoinController.VCUserGenderMale;
// dict[VidCoinController.VCUserGenderKey] = VidCoinController.VCUserGenderFemale;
vidcoin.updateUserDictionary(dict);
```

### Step 6: Check if a video is available
You may want to know if a video is available for a given placement. For example, you shouldn’t display a button inviting the user to watch a video if there is none in cache. Calling the function `videoIsAvailableForPlacement()` is the way to go:
```as
public function videoIsAvailableForPlacement(placementCode:String):Boolean
```

This function will return a `BOOL`, equal to `true` if a video is ready to be played for the placement you passed as parameter, and `false` otherwise.

*Example:*
```as
if (vidcoin.videoIsAvailableForPlacement("yourPlacementCode")) {
	// Enable a play button
}
else {
	// Disable the play button	
}
```

### Step 7: Play a video
You can present a video to the user by calling the following function:
```as
public function playAdForPlacement(placementCode:String, animated:Boolean = true):void
```

*Example:*
```as
vidcoin.playAdForPlacement("yourPlacementCode", true);
```

If a video is ready to be played for this placement, the player will appear and the video will start. The `animated` parameter enables or disables the player’s animation.

### Step 8: Using VidCoinEvents (optional)
If you want to be notified of multiple things happening in the framework, you can use Vidcoin’s events. Your handler can then catch some (or all) of  the following events:

* This event will be raised regularly, whenever a change happens in the framework: if the list of available videos changed, if a new video is available for display, etc:
```as
VidCoinEvents.VCEventCampaignsUpdate
```

* This event will be raised just before the video player appears on screen. This is for example the perfect time to pause the audio of your game:
```as
VidCoinEvents.VCEventViewWillAppear
```

* This event is raised right after the video player left the screen. You may now resume the game audio if you paused it after the previous event:
```as
VidCoinEvents.VCEventViewDidDisappearWithInformation
```

You can access basic information about the video that was just watched in the viewInfo dictionary passed as a property of the event. Here are the values you can access:
```as
var statusCode:int = event.viewinfo[VidCoinController.VCKeyStatusCode] ;
var reward:int = event.viewInfo[VidCoinController.VCKeyReward];
```

The `statusCode` var can be compared to the integers `VidCoinController.VCStatusCodeSuccess` (the video was watched correctly), `VidCoinController.VCStatusCodeCancel` (the user canceled the video), and `VidCoinController.VCStatusCodeError` (something went wrong, and the user couldn’t watch the video).		
The `reward` informs you on the amount the user will be rewarded when the view is validated on the server-side.

* This event is raised when the server answers after the framework tried to validate the view:
```as
VidCoinEvents.VCEventDidValidateView
```

You can access the same information as the previous event, but in this case, the `status` value is either `VidCoinController.VCStatusCodeSuccess` or `VidCoinController.VCStatusCodeError`, depending on whether the server did validate the view or not. The value associated to the `reward` key contains the amount of your in-game currency that was actually credited to the user on the server side.		

Note that this event will always be raised after the previous event `VCEventViewDidDisappearWithInformation:`.

#### Full events example
```as
import com.vidcoin.extension.ane.events.VidCoinEvents;

vidcoin.addEventListener(VidCoinEvents.VIDCOIN,handleVidCoinEvents);

public function handleVidCoinEvents(event:VidCoinEvents):void{
	switch(event.code) {
		case VidCoinEvents.VCEventDidValidateView:
		case VidCoinEvents.VCEventViewDidDisappearWithInformation: {
			switch(event.viewInfo[VidCoinController.VCKeyStatusCode]) {
				case VidCoinController.VCStatusCodeSuccess: {
					// “SUCCESS”
					trace(event.viewInfo[VidCoinController.VCKeyStatus]);
					trace(event.viewInfo[VidCoinController.VCKeyReward]);
					break;
				}
				case VidCoinController.VCStatusCodeError:
				case VidCoinController.VCStatusCodeCancel: {
					// “ERROR” || “CANCEL”
					trace(event.viewInfo[VidCoinController.VCKeyStatus]);
					break;
				}
				default: {
					break;
				}
			}
			break;
		}
		case VidCoinEvents.VCEventCampaignsUpdate: {
			// Your code
			break;
		}	
		case VidCoinEvents.VCEventViewWillAppear: {
			// Your code
			break;
		}						
		default: {
			break;
		}
	}
}
```

### Step 9: Submitting to the iOS AppStore
When you submit your app for Apple’s approval, you will be asked if your app uses the Advertising Identifier. Vidcoin’s framework does use this identifier, in order to serve advertisements within your app:

![Image1](https://d3rud9259azp35.cloudfront.net/documentation/Vidcoin-ANE-1.png)

## Advice for integration
The integration of the Vidcoin solution must not deteriorate the user experience.
Because of certain parameters and restrictions (country, number of videos already viewed...) there may not always be available videos.

A non-blocking integration consists in offering Vidcoin to the user as an alternate solution to the classical running of the app.

Here is an example :
- The user is presented a given view of the app
- A button “Please sign-in to access your content” is presented
- Make the view subscribe to Vidcoin’s events, and implement the method triggered by the event called `vidcoinCampaignsUpdate`
- In this method, if a call to Vidcoin’s method `videoIsAvailableForPlacement` returns true for the given placement code, a second button can be presented: “Watch a video to access your content”
- If the call returns false, the second button should not be made visible.

This type of integration avoids proposing a user to watch a video if none is available, and does not damage the user experience. You should not expect videos to be always available.

The sample app gives a good example of a recommended integration.

## Server-side callback (optional)
If you want to use a server-side callback to credit your users, you can download our PHP SDK: https://github.com/VidCoin/VidCoin-PHP-SDK
