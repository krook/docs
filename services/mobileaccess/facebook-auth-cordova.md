---

copyright:
  years: 2015, 2016

---
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}

# Enabling Facebook authentication for Cordova apps
{: #facebook-auth-cordova}

Last updated: 29 September 2016
{: .last-updated}


To configure {{site.data.keyword.amafull}}  Cordova applications for Facebook authentication integration, make changes in native code of the Cordova application, in Java, Objective-C, or Swift. Configure each platform separately. This Cordova application must already be instrumented with the  {{site.data.keyword.amashort}} SDK. 


Use the native development environment to make changes in the native code, for example, in Android Studio or Xcode.
{:shortdesc}

## Before you begin
{: #facebook-auth-before}
You must have:
* A Cordova project that is instrumented with the {{site.data.keyword.amashort}} client SDK, see [Setting up the Cordova plug-in](https://console.{DomainName}/docs/services/mobileaccess/getting-started-cordova.html).
* An instance of a  {{site.data.keyword.Bluemix_notm}} application that is protected by {{site.data.keyword.amashort}} service. For more information about how to create a {{site.data.keyword.Bluemix_notm}} back-end application, see [Getting started](index.html).
* A Facebook App ID. For more information, see [Obtaining a Facebook App ID from the Facebook Developer Portal](https://console.{DomainName}/docs/services/mobileaccess/facebook-auth-overview.html#facebook-appID).



## Configuring the Android Platform
{: #facebook-auth-cordova-android}

The steps that are required to configure the Android Platform of a Cordova application for Facebook authentication integration are very similar to the steps that are required for native Android applications. For more information, see [Enabling Facebook authentication in Android apps](https://console.{DomainName}/docs/services/mobileaccess/facebook-auth-android.html). Complete the following steps:

* Configuring Facebook Application for Android Platform
* Configuring {{site.data.keyword.amashort}} for Facebook authentication

### Configuring {{site.data.keyword.amashort}} client SDK for Android

Configure the {{site.data.keyword.amashort}} client SDK for Android within your the Cordova application.

1. In your Android project folder, open the `build.gradle` file for the application module (**not** the project `build.gradle` file). Find the dependencies section, and add a new compile dependency for client SDK:

	```Gradle
 	    dependencies {
     		compile group: 'com.ibm.mobilefirstplatform.clientsdk.android',    
     		name:'facebookauthentication',
     		version: '1.+',
     		ext: 'aar',
     		transitive: true
     		// other dependencies  
		 }
	```
	
2. Synchronize your project with Gradle by clicking **Tools > Android > Sync Project with Gradle Files**.

3. Open the `android/res/values/strings.xml` file and add a `facebook_app_id` string that contains your Facebook App ID.

	```XML
	<resources>
		<string name="app_name">HelloCordova</string>
		<string name="launcher_name">@string/app_name</string>
		<string name="activity_name">@string/launcher_name</string>
		<string name="facebook_app_id">522733366802111</string>
	</resources>
	```
	
4. In the `AndroidManifest.xml` file of your Android project (`android/manifests/AndroidManifest.xml`):

	* Add required metadata for the Facebook SDK to the <application> element:

	```XML
	<application .......>
	
	  <meta-data
	      android:name="com.facebook.sdk.ApplicationId"
	      android:value="@string/facebook_app_id"/>
	
	  <activity ...../>
	  <activity ...../>
	</application>
	```

	* Add a Facebook Activity element under your existing activities:

	```XML
	<application .....>
		<activity ...../>
		<activity ...../>
	
		<activity   android:name="com.facebook.FacebookActivity"
	              android:configChanges="keyboard|keyboardHidden|screenLayout|screenSize|orientation"
	              android:theme="@android:style/Theme.Translucent.NoTitleBar"
	              android:label="@string/app_name" 
			    />
	</application>
	```

2. For Cordova applications initialize the {{site.data.keyword.amashort}} client SDK in your JavaScript code instead of in the Java code. The `FacebookAuthenticationManager`  API must still be registered in your native code. Add this code to the main activity `onCreate` method:  

	```Java
	FacebookAuthenticationManager.getInstance().registerDefaultAuthenticationListener(this);
	```

6. Add the following code to your Activity:

	```Java
	  @Override
	  protected void onActivityResult(int requestCode, int resultCode, Intent data) {
	      super.onActivityResult(requestCode, resultCode, data);
	      FacebookAuthenticationManager.getInstance()
	          .onActivityResultCalled(requestCode, resultCode, data);
	  }
	```

## Configuring the iOS platform
{: #facebook-auth-cordova-ios}

The steps required to configure iOS Platform of Cordova application for Facebook authentication integration are similar to the steps required for native iOS applications. The major difference is that currently Cordova CLI does not support the CocoaPods dependency manager. You must manually add files that are required for integrating with Facebook authentication. For more information, see  [Enabling Facebook authentication in iOS apps](https://console.{DomainName}/docs/services/mobileaccess/facebook-auth-ios.html). Complete the following steps:

* Configuring Facebook Application for iOS Platform
* Configuring {{site.data.keyword.amashort}} for Facebook authentication

### Manually installing the {{site.data.keyword.amashort}} SDK for Facebook authentication and Facebook SDK
{: #facebook-auth-cordova-ios-sdk}
1. Download the archive that contains the [{{site.data.keyword.Bluemix_notm}} Mobile Services SDK for iOS](https://hub.jazz.net/git/bluemixmobilesdk/imf-ios-sdk/archive?revstr=master).

1. Go to the `Sources/Authenticators/IMFFacebookAuthentication` directory and copy (drag and drop) all of the files to your iOS project in Xcode. Copy the following files:

	* IMFDefaultFacebookAuthenticationDelegate.h
	* IMFDefaultFacebookAuthenticationDelegate.m
	* IMFFacebookAuthenticationDelegate.h
	* IMFFacebookAuthenticationHandler.h
	* IMFFacebookAuthenticationHandler.m

	When prompted by Xcode, select **Copy files...**.

1. Download and install [Facebook SDK v3.19](https://developers.facebook.com/resources/facebook-ios-sdk-3.19.pkg).

1. The Facebook SDK will be installed into `~/Documents/FacebookSDK` directory. Navigate to that directory and copy (drag and drop) the `FacebookSDK.framework` file to your iOS project in Xcode.

1. Click your project root in Xcode and select **Build Phases**.

1. Add the `FacebookSDK.framework` file to the list of linked libraries in **Link binary with libraries**.

 See [Configuring iOS Platform for Facebook authentication](https://console.{DomainName}/docs/services/mobileaccess/facebook-auth-ios.html). Register the `IMFFacebookAuthenticationHandler` API in native code as described in the **Initializing the {{site.data.keyword.amashort}} client SDK** section. Do not initialize `IMFClient` in your native code.

Add the following line to the `application:openURL:sourceApplication:annotation` method of your application delegate. This code ensures that all Cordova plugins are notified of respective events.

```
[[ NSNotificationCenter defaultCenter] postNotification:
		[NSNotification notificationWithName:CDVPluginHandleOpenURLNotification object:url]];      
```
{: codeblock}

## Initializing the {{site.data.keyword.amashort}} client SDK
{: #facebook-auth-cordova-init}

Use the following JavaScript code in your Cordova application to initialize the {{site.data.keyword.amashort}} client SDK.

```JavaScript
BMSClient.initialize("applicationRoute", "applicationGUID");
```
{: codeblock}

Replace `applicationRoute` and `applicationGUID` with values obtained for **Route** and **App GUID** from **Mobile Options**.
	



##Initializing the {{site.data.keyword.amashort}} AuthorizationManager
Use the following JavaScript code in your Cordova application to initialize the {{site.data.keyword.amashort}} AuthorizationManager.
```JavaScript
MFPAuthorizationManager.initialize("tenantId");
```
{: codeblock}

Replace the `tenantId` value with the {{site.data.keyword.amashort}} service `tenantId`. This value that you get by clicking the **Show Credentials** button on the {{site.data.keyword.amashort}} service tile.



## Testing the authentication
{: #facebook-auth-cordova-test}
After the client SDK is initialized and Facebook authentication manager is registered, you can start making requests to your mobile back-end application.

### Before you begin
You must be using the {{site.data.keyword.mobilefirstbp}} boilerplate and already have a resource protected by {{site.data.keyword.amashort}} at the `/protected` endpoint. For more information, see [Protecting resources](https://console.{DomainName}/docs/services/mobileaccess/protecting-resources.html).

1. Try to send a request to protected endpoint of your newly-created mobile back-end application from your browser. Open the following URL: `{applicationRoute}/protected`. For example: `http://my-mobile-backend.mybluemix.net/protected`
<br/>The `/protected` endpoint of a mobile back-end application that was created with MobileFirst Services Starter boilerplate is protected with {{site.data.keyword.amashort}}. An `Unauthorized` message is returned in your browser. This message is returned because this endpoint can only be accessed by mobile applications that are instrumented with {{site.data.keyword.amashort}} client SDK.

1. Use your Cordova application to make request to the same endpoint. Add the following code after you initialize `BMSClient`:

	```JavaScript
	var success = function(data){
    	console.log("success", data);
    }
	var failure = function(error){
    	console.log("failure", error);
    }
	var request = new MFPRequest("/protected", MFPRequest.GET);
	request.send(success, failure);
	```
{: codeblock}

1. Run your application. A Facebook login screen pops up:

	![image](images/android-facebook-login.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	![image](images/ios-facebook-login.png)

	This screen might look slightly different if you do not have the Facebook app installed on your device, or if you are not currently logged in to Facebook.

1. Click **OK** to authorize {{site.data.keyword.amashort}}  to use your Facebook user identity for authentication purposes.

1. 	When your request succeeds, the following output is in the LogCat utility or Xcode console:

	![Facebook authentication success message for Android](images/android-facebook-login-success.png)

	![Facebook authentication success message for iOS ](images/ios-facebook-login-success.png)
