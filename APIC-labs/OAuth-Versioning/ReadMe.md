# IBM API Connect

## Add OAuth Security to your API and use Lifecycle Controls to Version Your API

[Return to main APIC lab page](../ReadMe.md#lab-abstracts)

<span style="color: red">Lab prerequisite:</span> "The Developer Portal Experience"

---

# Table of Contents
- [1. Introduction](#introduction)

- [2. Configure a New OAuth 2.0 Provider API](#configure_oauth)
	* [2a. Configure Authentication URL User Registry](#configure_registry)

- [3. Create an OAuth Service](#create_oauth_service)

- [4. Add the OAuth Service to the Sandbox Catalog](#add_service)

- [5. Create a New Version of the Customer API](#new_version)
	* [5a. Add OAuth Security to the Customer API](#oauth_customer)

- [6. Create a New Product](#create_product)

- [7. Stage the Product to your API Manager Environment](#stage_product)

- [8. Supersede Version 1.0.0 of the Product](#supersede)

- [9. Test the OAuth Configuration](#test_oauth)

---

# 1. Introduction <a name="introduction"></a>

In this lab, we will secure the Customer Database API that was created in the "Create and Secure an API to Proxy an Existing REST Web Service" lab to protect the resources exposed by IBM API Connect. Consumers of our API will be required to obtain and provide a valid OAuth token before they can invoke the Customer Database API. In order for the changes to take effect, we must publish the APIs to the Developer Portal and make them available for the API Consumers.  The Customer 1.0.0 version is already running and has active subscribers.

API lifecycle management capabilities is an essential part of the API Management platform. The API lifecycle includes the following stages:

1.  Plan and design the API

2.  Develop the API

3.  Test the API

4.  Deploy (publish) the API

5.  Retire and deprecate the API

In this tutorial, you will explore the following key capabilities:

-   Configure an OAuth 2.0 service (the Resource Owner Password grant type)

-   Clone a new version of an API

-   Secure the new version of your API

-   Create a new API Product

-   Replace the old Product

-   Test the OAuth API in the Developer Portal

# 2. Configure a New OAuth 2.0 Provider API <a name="configure_oauth"></a>

IBM API Connect is a full-featured OAuth 2.0 provider. The OAuth exchange works like any other API call, and thus we treat it as its own API.

In this section, you will create a new OAuth provider API, configure which grant type to use, and configure how it will authenticate user credentials.

# 2a. Configure Authentication URL User Registry <a name="configure_registry"></a>

In order to configure user authentication, you must first define the registry to use, which may be LDAP, local user registry, or an authentication URL. For this lab, you will implement an Authentication
URL.

If you are already logged in API Connect and continuing from the previous lab, go directly to step 4.  
1\. To access Cloud Pak for Integration Platform UI as described in Access Environment section [here](../../access-env.md). 

![alt text][pic104]

2\. Navigate to the API Connect instance, api-cluster instance (Type API Management).

![alt text][pic10a]

3\. If this is your first time logging in, the login page is presented. Click **Cloud Pak User Registry**.

![alt text][pic11a]

4\. In the left menu, click on **Resources**.  As you hover over the icon, you will see the item name.

![alt text][pic9]

5\. Make sure **User registries** is selected and click **Create**.

![alt text][pic10]

6\. Click on **Authentication URL user registry**.

![alt text][pic11]

7\. Enter **App Registry** for the **Title**.  
Build the **Url** as follow: "https://httpbin-app-httpbin.<*your cluster url*>/basic-auth/studentx/password" where your cluster url is the part of Red Hat url from ".app.<*cluster id*>.cloud.techzone.ibm.com". For example if your Red Hat url is "https://console-openshift-console.apps.66418360858a4d001e34fc72.cloud.techzone.ibm.com", the url will be "http://httpbin-app-httpbin.apps.66418360858a4d001e34fc72.cloud.techzone.ibm.com/basic-auth/studenx/password".  
**App Registry** for the **Display name**.  Click **Save**.

![alt text][pic12]

[pic0]: images/0.png
[pic1]: images/1.png
[pic2]: images/2.png
[pic3]: images/3.png
[pic4]: images/4.png
[pic5]: images/5.png
[pic6]: images/6.png
[pic7]: images/7.png
[pic8]: images/8.png
[pic9]: images/9.png
[pic10]: images/10.png
[pic11]: images/11.png
[pic12]: images/12.png
[pic10a]: images/10a.png
[pic11a]: images/11a.png
[pic104]: images/104.png

# 3. Create an OAuth Service <a name="create_oauth_service"></a>

1\. You should still be in **Resources**.  If not, in the left menu, click **Resources**.

![alt text][pic13]

2\. Click on **OAuth providers**.

![alt text][pic14]

3\. Click **Add** and select **Native OAuth provider** from the drop down.

![alt text][pic15]

![alt text][pic16]

4\. Enter **oauth** for the **Title** and select **DataPower API Gateway** for the **Gateway Type**.  Click **Next**.

![alt text][pic17]

5\. The Configuration screen will show the default Authorize and Token paths.  For **Supported grant types**, select **Resource owner - Password** and deselect **Access code**.  For **Supported client types**, select **Confidential**.  Click **Next**.

![alt text][pic18]

6\. One scope, **sample&#95;scope&#95;1**, is automatically created.  

![alt text][pic19]

7\. Replace **sample&#95;scope&#95;1** with **customer** for Scope **Name** and replace **Sample scope definition 1** with **Access to Customer API** for Scope **Description**.  Click **Next**.

![alt text][pic20]

8\. Accept the defaults (**App Registry** for **Authenticate application users using**) and click **Next**.

![alt text][pic21]

9\. Review the OAuth configuration and click **Finish**.

![alt text][pic22]

10\. Click **Save**.

![alt text][pic68]

[pic13]: images/13.png
[pic14]: images/14.png
[pic15]: images/15.png
[pic16]: images/16.png
[pic17]: images/17.png
[pic18]: images/18.png
[pic19]: images/19.png
[pic20]: images/20.png
[pic21]: images/21.png
[pic22]: images/22.png
[pic68]: images/68.png

# 4. Add the OAuth Service to the Sandbox Catalog <a name="add_service"></a>

1\. In the left menu, click on **Manage**.

![alt text][pic23]

2\. Click on **Sandbox**

![alt text][pic24]

3\. In the top menu, click on **Catalog settings**.

![alt text][pic25]

4\. Click on **API user registries**.

![alt text][pic26]

5\. Click **Edit**.

![alt text][pic27]

6\. Select **App Registry** and click **Save**.

![alt text][pic28]

7\. Click on **OAuth providers**.

![alt text][pic29]

8\. Click **Edit**

![alt text][pic30]

9\. Select **oauth** and click **Save**.

![alt text][pic31]

[pic23]: images/23.png
[pic24]: images/24.png
[pic25]: images/25.png
[pic26]: images/26.png
[pic27]: images/27.png
[pic28]: images/28.png
[pic29]: images/29.png
[pic30]: images/30.png
[pic31]: images/31.png

# 5. Create a New Version of the Customer API <a name="new_version"></a>

IBM API Connect supports multiple versions of APIs.  We will create a new version of the Customer API before making any changes that would break functionality for existing consumers. 

First, we will save the API as a new version.

1\. In the left menu, click on **Develop**.

![alt text][pic32]

2\. Confirm the **APIs** tab is selected and click on the **3-dot** menu next to **Customer Database** and select **Save as New Version** from the drop down menu.

![alt text][pic33]

3\. Enter **2.0.0** for the **Save as New Version** and click **Submit**.

![alt text][pic34]

## 5a. Add OAuth Security to the Customer API <a name="oauth_customer"></a>

We will modify the security policy for our new API version to tell it to use the OAuth 2.0 provider.

1\. Confirm the **APIs** tab is selected and click on version **2.0.0** of **Customer Database**.

![alt text][pic35]

2\. Click on the **+** next to **Security Schemes**.

![alt text][pic36]

3\. Enter **oauth-1** for the **Security Definition Name (Key)** and select **oauth2** from the **Security Definition Type** drop down menu.  When you select oauth2 from the drop down menu, it will enable additional configuration options.

![alt text][pic37]

4\.Select **Resource Owner - Password** from the **Flow** drop down menu and click **Create**.

![alt text][pic38]

5\. Click **Save**.

![alt text][pic69]

Once saved, you will see an indicator window appear that shows that **Your API has been updated**.  Click on the **X** to close the window.

![alt text][pic70]

6\. In the **Design** tab, find the **Security** section.  You will likely have to scroll up.  Expand the **Security** section clicking on the + if it is not already expanded.  

![alt text][pic39]

7\. Select **oauth-1** and click on **Scopes** and select **customer**.  Click **Create**.

![alt text][pic90]

8\. Click **Save**.

![alt text][pic69]

9\. Once saved, you will see an indicator window appear that shows that **Your API has been updated**.  Click on the **X** to close the window.

![alt text][pic70]

[pic32]: images/32.png
[pic33]: images/33.png
[pic34]: images/34.png
[pic35]: images/35.png
[pic36]: images/36.png
[pic37]: images/37.png
[pic38]: images/38.png
[pic39]: images/39.png
[pic40]: images/40.png
[pic41]: images/41.png
[pic42]: images/42.png
[pic69]: images/69.png
[pic70]: images/70.png
[pic90]: images/90.png
[pic91]: images/91.png

# 6. Create a New Product <a name="create_product"></a>

In IBM API Connect, Plans and APIs are grouped together in Products, with which you can manage the availability and visibility of APIs and Plans.

Products allow related APIs to be bundled together for subscribers.

1\. In the left menu, click on **Develop**.

![alt text][pic43]

2\. Click on **Products**

![alt text][pic44]

3\. Click **Add** and select **Product** from the drop down menu.

![alt text][pic45]

![alt text][pic46]

4\. Select **New product** and click **Next**.

![alt text][pic47]

5\. Enter **Customer** for the **Title** and change the **Version** to **2.0.0**.  Click **Next**.

![alt text][pic48]

6\. Select the **Customer Database** API that is version **2.0.0** and click **Next**.

![alt text][pic49]

7\. We are going to create some new Plans.  Click **Add**

![alt text][pic50]

8\. Replace **Plan 1** with **Gold** for the **Title** and **500** for the **Rate limit**.

![alt text][pic51]

9\. Click on the **Trash can** icon next to the **Default plan** to delete it.

![alt text][pic52]

10\. Click **Add** to add another Plan.

![alt text][pic53]

11\. Replace **Plan 2** with **Silver** for the **Title** and **250** for the **Rate limit**.

![alt text][pic54]

12\. Click **Add** to add another Plan.

![alt text][pic53]

13\. Replace **Plan 3** with **Bronze** for the **Title** and **2** for the **Rate limit** and change the drop down from **hour** to **minute**.  Click **Next**.

![alt text][pic55]

14\. Select **Public** for **Visibility** and **Authenticated** for **Subscribability**.  Click **Next**.

![alt text][pic56]

15\. Your Product will be created and associated with the required objects such as APIs and Plans.  Click **Done**.

![alt text][pic57]

16\. Click on **Products**.

![alt text][pic58]

[pic43]: images/43.png
[pic44]: images/44.png
[pic45]: images/45.png
[pic46]: images/46.png
[pic47]: images/47.png
[pic48]: images/48.png
[pic49]: images/49.png
[pic50]: images/50.png
[pic51]: images/51.png
[pic52]: images/52.png
[pic53]: images/53.png
[pic54]: images/54.png
[pic55]: images/55.png
[pic56]: images/56.png
[pic57]: images/57.png
[pic58]: images/58.png

# 7. Stage the Product to your API Manager Environment <a name="stage_product"></a>

Before an API Product can be published, we must first stage that Product to a Catalog. When a Product is in the staged state, it is not yet visible to, or subscribable by developers. However, it can be reviewed by the API Product Manager and published once it has been determined that the API Product is ready to be consumed.

If you stage a Product to a Catalog, editing and then restaging that Product through the Products tab of API Designer or API Manager will affect changes to the staged version.

1\. Click on the **3-dot** menu next to version **2.0.0** of the **Customer** Product and select **Stage**.

![alt text][pic59]

2\. Select **Sandbox** for the **Stage to** catalog and click **Next**.

Note:  IBM API Connect allows you to publish products to specific gateways associated with the Catalog.

![alt text][pic60]

3\. Accept the default values for **Visibility** and **Subscribability** and click **Stage**.

![alt text][pic100]

[pic59]: images/59.png
[pic60]: images/60.png
[pic100]: images/100.png


# 8. Supersede Version 1.0.0 of the Product <a name="supersede"></a>

IBM API Connect provides capabilities for managing the lifecycle of your API Products. There are various states which an API Product can reside in, as well as controls around when you can move an API Product from one state to another. In this section, you will explore how to replace a running version of an API Product with a new one.

1\. In the left menu, click **Manage**.

![alt text][pic61]

2\. Click on **Sandbox**.

![alt text][pic62]

3\. The **Products** tab will list all of the API Products that this Catalog is currently managing.  Confirm version **2.0.0** of the **Customer** Product is **Staged** and version **1.0.0** of the Customer Product is **Published**.

![alt text][pic63]

4\. Click on the **3-dot** menu next to version **1.0.0** of the **Customer** Product and click **Supersede**.

![alt text][pic64]

5\. Select **customer 2.0.0** and click **Next**.

![alt text][pic65]

6\.  In order to maintain our consumers' entitlements, we need to migrate their Plan subscriptions.

The **customer 1.0.0** Product has a Plan called **Default Plan**.  The **customer 2.0.0** Product does not have a Default Plan.  Therefore, we need to choose how we are going to move the subscribers.  Select **Bronze** from the **Target** drop down menu and click **Supersede**.

![alt text][pic66]

7\. IBM API Connect will take care of deprecating version 1.0.0 of the Product and publishing version 2.0.0.

![alt text][pic67]

[pic61]: images/61.png
[pic62]: images/62.png
[pic63]: images/63.png
[pic64]: images/64.png
[pic65]: images/65.png
[pic66]: images/66.png
[pic67]: images/67.png

# 9. Test the OAuth Configuration <a name="test_oauth"></a>

In this section, you will test the new version of the API to ensure that OAuth is working properly.

1\. In the left menu, click **Develop**.

![alt text][pic1a]

2\. Click on **Customer Database v 2.0.0** API.

![alt text][pic2a]

3\. In the top menu, click on the **Test** tab and copy **X-IBM-Client-Secret** value that will be use for testing.

![alt text][pic3a]

4\. In the top menu, click on the **Explorer** tab.
In the Explorer tab you simulate an API Call from outside the cluster.

![alt text][pic4a]

5\. In the left menu, under Overview, click on the operation **GET/customers**

![alt text][pic5a]

6\. Click on **Try it**.

![alt text][pic6a]

7\. As **Security** choose **oauth1** from the drop down menu.
As **API-key** leave the default value.


![alt text][pic7a]

8\. Fullfill **API Secret** with the value copied from step 3 in Test tab.
As **Username** use studentx and as **Password** use password.
Check the scope **client**.
Recall that when we configured the OAuth API, we provided an Authentication URL as the method for validating the user credentials.

![alt text][pic8a]

9.\ You're now ready to test, clicking on **Get Token**
The Explorer will call out to the OAuth Token URL with your client credentials and user credentials.  The OAuth API will intercept the request, validate the credentials, and generate a token. 

![alt text][pic9a]


[pic71]: images/71.png
[pic72]: images/72.png
[pic73]: images/73.png
[pic74]: images/74.png
[pic76]: images/76.png
[pic77]: images/77.png
[pic78]: images/78.png
[pic79]: images/79.png
[pic80]: images/80.png
[pic81]: images/81.png
[pic82]: images/82.png
[pic83]: images/83.png
[pic84]: images/84.png
[pic85]: images/85.png
[pic86]: images/86.png
[pic87]: images/87.png
[pic88]: images/88.png
[pic89]: images/89.png
[pic92]: images/92.png
[pic93]: images/93.png
[pic94]: images/94.png
[pic95]: images/95.png
[pic1a]: images/1a.png
[pic2a]: images/2a.png
[pic3a]: images/3a.png
[pic4a]: images/4a.png
[pic5a]: images/5a.png
[pic6a]: images/6a.png
[pic7a]: images/7a.png
[pic8a]: images/8a.png
[pic9a]: images/9a.png

## Summary

Congratulations, you have completed the **Add OAuth Security to your API and use Lifecycle Controls to Version Your API** lab. Throughout the lab, you learned how to:

-   Configure an OAuth 2.0 service with the Resource Owner Password grant type

-   Clone a new version of an API

-   Secure the new version of your API

[Return to main APIC lab page](../ReadMe.md#lab-abstracts)
