---

copyright:
  years: 2017, 2018
lastupdated: "2018-11-14"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:pre: .pre}
{:tip: .tip}
{:screen: .screen}
{:codeblock: .codeblock}

# Adding attributes before user sign in
{: #sign-in}

With {{site.data.keyword.appid_full}}, you can start building a profile for users that you know are going to need access to your app, prior to their initial sign-in.
{: shortdesc}

To learn more about the types of attributes, check out [Understanding user profiles](user-profile.html). To learn more about custom attributes and their security considerations, check out [Custom attributes](custom-attributes.html).
{: tip}

**Why would I want to add information about a user to my app before they sign in for the first time?**

Consider an application where you use {{site.data.keyword.appid_short_notm}} to federate existing users from your SAML identity provider. You might want certain users to have `admin` access immediately upon signing into the application for the first time. To make this happen, you can use the preregistration endpoint to set a custom `admin` attribute for those users and grant them access to the administration console without any further action on your part. Be sure to consider the [security issues](custom-attributes.html) that can arise by changing the default setting.

**How are users identified?**

You can identify your users by using one of the following:

* The user's unique ID, called the **GUID**, in the identity provider. Although this identifier always exists and is guaranteed to be unique, it is not always readily available or easy to understand. For instance, Cloud Directory uses a random 16 byte random GUID.
* If available, the user's **email**.

**How do I know which identity provider gives what information?**

Check out the following table to see which type of identity information that you can use.

<table>
  <thead>
    <tr>
      <th>Identity provider</th>
      <th>GUID</th>
      <th>Email</th>
      <th>Sub</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Cloud Directory</td>
      <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
      <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
      <td> </td>
    </tr>
    <tr>
      <td>Facebook</td>
      <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
      <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
      <td> </td>
    </tr>
    <tr>
      <td>Google</td>
      <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
      <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
      <td> </td>
    </tr>
    <tr>
      <td>SAML</td>
      <td></td>
      <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
      <td> </td>
    </tr>
    <tr>
      <td>Custom</td>
      <td> </td>
      <td> </td>
      <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
    </tr>
  </tbody>
</table>

**Is Cloud Directory handled differently?**

In order to ensure the integrity of the preregistered user attributes, Cloud Directory places additional requirements on its users. Preregistration can only occur when email validation is enabled and verified. If you preregister a Cloud Directory user with specific attributes, then those attributes are intended for a specific person. If the email is not verified first, it is possible for another user to claim the email address and any attributes that are assigned to it.

How do I do that?

1. Set Cloud Directory to email and password mode. You can do this through the UI in the general settings on the **Cloud Directory** tab. You can also set it through the [management APIs](https://appid-management.ng.bluemix.net/swagger-ui/#!/Cloud_Directory_Users/createCloudDirectoryUser).

2. Verify the users email address to confirm their identity in one of the following ways:

  * To verify a users identity through email, set **Email verification** to **On** in the **Cloud Directory** tab of the service dashboard. If a user is added by you and signs in to your app without first verifying their email, the sign in completes successfully, but their predefined attribute is deleted.
  * To verify users manually you must be an administrator and use the Cloud Directory [management APIs](https://appid-management.ng.bluemix.net/swagger-ui/#!/Cloud_Directory_Users/createCloudDirectoryUser). When creating or updating a user, you should explicitly set the `status` field to `CONFIRMED` within your user data payload.

**Is there anything special that I need to do when using a custom identity provider?**

When you add user information to your application in advance, you can use any unique identifier that is provided by the authentication flow. The identifier must _exactly_ match the `sub` of the signed JSON Web Token that is sent during the authorization request. If the identifier does not match, then the profile that you want to add is not linked successfully.



## Adding user information to your app
{: #add}

Now that you've learned about the process and considered your security implications, try adding a user.

**Before you begin:**

To add custom attributes for a specific user with the [/users](https://appid-management.ng.bluemix.net/swagger-ui/#!/Users/users_search_user_profile) Management API endpoint, you must know the following information:

* Which identity provider that the user is going to use to sign in.
* The user's unique identifier that is provided by the identity provider.

When a user signs into your app for the first time, {{site.data.keyword.appid_short_notm}} searches for the user. If found, the user inherits the identity that you assigned. If the user is not found, then a new user is created based on the information that is provided by the identity provider.

**To add a user:**

1. Log into IBM Cloud.
  ```
  ibmcloud login
  ```
  {: pre}

2. Find your IAM token by running the following command.
  ```
  ibmcloud iam oauth-tokens
  ```
  {: pre}

3. Make a POST request to the `/users` endpoint that contains a description of the user and the attributes that you want to set as a JSON object.

  Header:
  ```
  POST {management-url}/management/v4/{tenantId}/users
       Host: <management-server-url>
       Authorization: 'Bearer <IAM_TOKEN>'
       Content-Type: application/json
  ```
  {: pre}

  Body:
  ```
   {
       "idp": "<Identity Provider>",
       "idp-identity": "<User's unique identifier>",
       "profile": {
           "attributes": {
             "mealPreference":"vegeterian"
           }
       }
   }
  ```
  {: pre}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the components</th>
    </thead>
    <tbody>
      <tr>
        <td><code><em>idp</em></code></td>
        <td>The identity provider that the user will authenticate with. Options include: `saml`, `cloud_directory`, `facebook`, `google`, `appid_custom`, `ibmid`.</td>
      </tr>
      <tr>
        <td><code><em>idp-identity</em></code></td>
        <td>The unique identifier provided by the identity provider.</td>
      </tr>
      <tr>
        <td><code><em>profile</em></code></td>
        <td>The user's profile containing the custom attribute JSON mapping.</td>
      </tr>
    </tbody>
  </table>

  Example request:
  ```
  $ curl --request POST \
       --url 'https://{Management_URI}/users \
       --header 'Authorization: Bearer {IAM_TOKEN}' \
       --header 'Content-Type: application/json' \
       --data '{"idp": "saml", "idp-identity": "user@ibm.com", "profile": { "attributes": { "role": "admin",
       "frequent_flyer_points": 1000 }}}'
  ```
  {: screen}

3. Verify that registration was successful in one of the following ways:
  * Check for the user ID in the response.
    ```
    {
        "id": "<{{site.data.keyword.appid_short_notm}} User Id>"
    }
    ```
    {: screen}
  * Check for the user profile that was created.

Keep in mind that a user's predefined attributes are empty until their first authentication, but the user is, for all intents and purposes, a fully authenticated user. You can use their unique ID just as you would someone who had already signed in. For instance, you can modify, search, or delete the profile.

Now that you have associated a user with specific attributes, try [accessing attributes](/docs/services/appid/custom-attributes.html)!


</br>
