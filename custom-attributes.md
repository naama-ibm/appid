---

copyright:
  years: 2017, 2018
lastupdated: "2018-11-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:pre: .pre}
{:tip: .tip}
{:screen: .screen}

# Accessing custom user attributes
{: #custom}

With {{site.data.keyword.appid_full}}, you can save and access custom attributes.
{: shortdesc}

**Why would I want to save additional attributes about a user?**

Attributes are pieces of information about your users. By saving them, you can create profiles on your users that allow you to personalize their experience. The more attributes that are added to their profile, the more personalized their app can be. Check out this blog to see how creating user profiles can make a difference: <a href="https://www.ibm.com/blogs/bluemix/2017/03/introducing-ibm-bluemix-app-id-authentication-profiles-service-app-developers/" target="_blank">Introducing {{site.data.keyword.appid_short_notm}}<img src="../../icons/launch-glyph.svg" alt="External link icon"></a>.

</br>

**How are the attributes saved?**

Depending on your configuration, attributes are encrypted and saved as part of a user profile when a user interacts with your application. The interaction could be a user signing in or setting a preference in your app.

</br>

**Is there a limit to the amount of information that can be stored for each user?**

You can store 100KB of information for each user.

</br>

**Are there any security considerations I should make?**

By default, custom attributes are modifiable and can be updated by using an {{site.data.keyword.appid_short_notm}} access token from a client application. This means that without taking proper precautions either the user or the application can update custom attributes immediately following the first user sign in, provided that they have access to an access token. This can potentially lead to unintended consequences. For example, a user could change their role from user to admin which might expose administrative privileges to malicious users.

To prevent your users from changing the attributes that you give them, set **Change custom attributes from the app** to **Off** on the **Profiles** tab of the {{site.data.keyword.appid_short_notm}} dashboard. By default, it is set to **On**.
{: tip}

</br>


## Accessing with the iOS SDK
{: #ios}

 You can access custom attributes by passing an access token through the following API methods.

  ```swift
  func setAttribute(key: String, value: String, completionHandler: @escaping(Error?, [String:Any]?) -> Void)
  func setAttribute(key: String, value: String, accessTokenString: String, completionHandler: @escaping(Error?, [String:Any]?) -> Void)

  func getAttribute(key: String, completionHandler: @escaping(Error?, [String:Any]?) -> Void)
  func getAttribute(key: String, accessTokenString: String, completionHandler: @escaping(Error?, [String:Any]?) -> Void)

  func getAttributes(completionHandler: @escaping(Error?, [String:Any]?) -> Void)
  func getAttributes(accessTokenString: String, completionHandler: @escaping(Error?, [String:Any]?) -> Void)

  func deleteAttribute(key: String, completionHandler: @escaping(Error?, [String:Any]?) -> Void)
  func deleteAttribute(key: String, accessTokenString: String, completionHandler: @escaping(Error?, [String:Any]?) -> Void)
  ```
  {: pre}

When an access token is not explicitly passed, {{site.data.keyword.appid_short_notm}} uses the last received token.

For example, you can call the following code to set a new attribute, or override an existing one.

  ```swift
	AppID.sharedInstance.userProfileManager?.setAttribute("key", "value") { (error, result) in
		guard let result = result, error == nil else {
	  		return // an error has occurred
		}
		// attributes recieved as a Dictionary
	})
  ```
  {: pre}

For more information about working in iOS Swift, check out the <a href="https://github.com/ibm-cloud-security/appid-clientsdk-swift" target="_blank">SDK <img src="../../icons/launch-glyph.svg" alt="External link icon"></a>.
{: tip}

</br>


## Accessing with the Android SDK
{: #android}

You can access custom attributes by passing an access token through the following API methods.

```java
void setAttribute(@NonNull String name, @NonNull String value, UserAttributeResponseListener listener);
void setAttribute(@NonNull String name, @NonNull String value, @NonNull AccessToken accessToken, UserAttributeResponseListener listener);

void getAttribute(@NonNull String name, UserAttributeResponseListener listener);
void getAttribute(@NonNull String name, @NonNull AccessToken accessToken, UserAttributeResponseListener listener);

void deleteAttribute(@NonNull String name, UserAttributeResponseListener listener);
void deleteAttribute(@NonNull String name, @NonNull AccessToken accessToken, UserAttributeResponseListener listener);

void getAllAttributes(@NonNull UserAttributeResponseListener listener);
void getAllAttributes(@NonNull AccessToken accessToken, @NonNull UserAttributeResponseListener listener);
```
{: pre}

When an access token is not explicitly passed, {{site.data.keyword.appid_short_notm}} uses the last received token.

For example, you can call the following code to set a new attribute, or override an existing one.

```java
appId.getUserProfileManager().setAttribute(name, value, useThisToken, new UserProfileResponseListener() {
	@Override
	public void onSuccess(JSONObject attributes) {
		// attributes received in JSON format on successful response
	}

	@Override
	public void onFailure(UserAttributesException e) {
		// exception occurred
	}
});
```
{: pre}

For more information about working in Android, check out the <a href="https://github.com/ibm-cloud-security/appid-clientsdk-android" target="_blank">SDK <img src="../../icons/launch-glyph.svg" alt="External link icon"></a>.
{: tip}

</br>

## Accessing with the Node.js Server SDK
{: #node}

You can access custom attributes by passing an access token through the following API methods.

  ```javascript
	function getAllAttributes(accessTokenString) {}
	function getAttribute(accessTokenString, key) {}
	function setAttribute(accessTokenString, key, value) {}
	function deleteAttribute(accessTokenString, name) {}
  ```
  {: pre}

  Example usage:

  ```javascript

	const userProfileManager = require("ibmcloud-appid").UserProfileManager;
	userProfileManager.init();

	var accessToken = req.session[WebAppStrategy.AUTH_CONTEXT].accessToken;

	userProfileManager.setAttribute(accessToken, name, value).then(function (attributes) {
		// attributes returned as dictionary
	});
  ```
  {: pre}

For more information about working in Node.js Server Sdk, check out the <a href="https://github.com/ibm-cloud-security/appid-serversdk-nodejs" target="_blank">SDK <img src="../../icons/launch-glyph.svg" alt="External link icon"></a>.
{: tip}

</br>

## Accessing with the Swift Server SDK
{: #swift}

You can access custom attributes by passing an access token through the following API methods.

  ```swift
  func getAllAttributes(accessToken: String, completionHandler: (Swift.Error?, [String: Any]?) -> Void)
  func getAttribute(accessToken: String, attributeName: String, completionHandler: (Swift.Error?, [String: Any]?) -> Void)
  func setAttribute(accessToken: String, attributeName: String, attributeValue : "abc", completionHandler: (Swift.Error?, [String: Any]?) -> Void)
  func deleteAllAttributes(accessToken: String, completionHandler: (Swift.Error?, [String: Any]?) -> Void)
  ```
  {: pre}

  Example usage:

  ```swift

	let userProfileManager = UserProfileManager(options: options)
	let accesstoken = "access token"

	userProfileManager.setAttribute(accessToken: accessToken, attributeName: "name", attributeValue : "abc") { (error, response) in
		guard let response = response, error == error else {
			return // an error has occurred
		}
		// attributes recieved as a Dictionary
	}
  ```

  {: pre}

For more information about working in Swift Server Sdk, check out the <a href="https://github.com/ibm-cloud-security/appid-serversdk-swift" target="_blank">SDK <img src="../../icons/launch-glyph.svg" alt="External link icon"></a>.
{: tip}


## Accessing with the API
{: #api}

Didn't find an SDK for the language that your app is written in? No problem! You can integrate the service by using the APIs.
{: shortdesc}

{{site.data.keyword.appid_short_notm}} provides a <a href="https://appid-profiles.ng.bluemix.net/swagger-ui/index.html#/Attributes" target="_blank">REST API <img src="../../icons/launch-glyph.svg" alt="External link icon"></a> that allows log in, either anonymously or by authenticating, with a supported [identity provider](/docs/services/appid/manageidp.html).
