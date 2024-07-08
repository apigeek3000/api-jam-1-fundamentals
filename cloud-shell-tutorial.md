Apigee API Jam Lab 1: API Management Fundamentals 2024
============================================================

Introduction
------------

Welcome to Google's Apigee API Jam on Apigee API Management Fundamentals. This hands-on lab introduces you to fundamental API Management concepts and the Apigee API Management Platform. It focuses on giving you an overall understanding of how to build a successful API program with a well managed developer ecosystem. In this lab, you will learn how to manage APIs across all phases of the API Lifecycle - including API design, API Security, Developer consumption, as well as API Analytics and Monitoring.

![Intro Image](https://cdn.qwiklabs.com/ylUv7088nSqD0hFbA73zIWxy0MwTOkpNfoqoDn3R8%2FY%3D)

### Lab Objectives

In this lab, you will learn how to perform the following tasks:

1.  API Design & Creation - Design and create an API Proxy from an OpenAPI specification.
2.  API Products, Apps & API Keys - Secure APIs with API Keys, bundle APIs into API Products and understand the association between Apps, API Products & API Keys.
3.  Rate Limiting & Dynamic Response - Control API Consumption based on API Product tier quotas.
4.  App Developer Experience - Publish an API Products Catalog on a self-service Developer Portal. Test the App Developer on-boarding experience and self-service API consumption, and restrict access to resources using Teams and Audience entitlements.
5.  API Analytics - Measure API Program success using Apigee Analytics.

Part 1 - Design & Create an API Proxy with OpenAPI Specification
----------------------------------------------------------------

_Persona : API Team_

### **Use case**

You have a requirement to create an API endpoint for an existing service, to handle requests from various applications/API clients. You have decided to follow a "design first" approach and have built a specification for your API, using OpenAPI specification. Using this specification, you wish to implement your API, generate interactive API documentation, generate API test cases, etc.

### **How can Apigee help?**

Apigee enables you to quickly build and expose an API. You do this by creating an [API proxy](https://cloud.google.com/apigee/docs/api-platform/fundamentals/understanding-apis-and-api-proxies#whatisanapiproxy), which provides a facade for the backend service and data that you want to expose as an API endpoint. The API proxy decouples your backend service implementation from the API endpoint that developers consume. This shields developers from future changes to your services as well as implementation complexities. As you update services, developers will be insulated from those changes, and can continue to call the API, uninterrupted. On Apigee, the API Proxy is also where runtime policy configuration is applied for API Management capabilities. For further information, please see: [Understanding APIs and API Proxies](https://cloud.google.com/apigee/docs/api-platform/fundamentals/understanding-apis-and-api-proxies).

![Image of Apigee Proxy flow execution order](https://cdn.qwiklabs.com/W4bQ7HnHfxHMVOIbcL1dOEF8l0TBBQyAarE2B4q74Hk%3D)![Image of Apigee Proxy flow execution order](https://cdn.qwiklabs.com/zt40q5AtaJeZjKOLkOLdesNTTrL6rEaTA1T%2BtSkSMzQ%3D)

Apigee also supports the [OpenAPI specification](https://github.com/OAI/OpenAPI-Specification) out of the box, allowing you to auto-generate API Proxies and associated documentation. To design and create an OpenAPI specification, you can use an editor/IDE of your choice, there are many [options to choose from](https://openapi.tools/#gui-editors).

In this part of the lab, we will learn how to create an API proxy from an OpenAPI spec, that routes inbound requests to an existing HTTP service.

### **Part 1A - Create an API Proxy**

1.  First, download [this Open API Specification YAML file](https://storage.googleapis.com/apigeexlabs/apijam-lab1/hipster-products-backend-spec-v3.yaml) for the sample API backend description. You will use this file to generate an API proxy, and later in the lab, to document your API.
    
2.  Back to the GCP Console, enter "apigee" in the Search Panel and choose **Apigee API Management**.
    
    ![search-apigee.png](https://cdn.qwiklabs.com/yRh%2Bb8X3uI8%2FbyokilH5TtQeKUYt%2BajXQmcJC3yFLao%3D)
    
    You will be redirect to the Apigee Overview page.
    
    ![part1-apigee-overview-page.png](https://cdn.qwiklabs.com/ac5Irt%2FApAjKWTTzuwmewj9c7Nkd5zN2y94Kr%2FupsBM%3D)
3.  To create an Apigee API Proxy from an OpenAPI Specification. Click on **Proxy development → API Proxies** from the side navigation menu. You might notice there are some proxies have been pre-created. We shall ignore them for now as we'll be creating our own proxy.
    
    ![part1-create-proxy-menu.png](https://cdn.qwiklabs.com/GyBme%2FYBXdAzShvtH1bmlVgLLLHmYl3csuz4NGBLtTU%3D)
4.  Click the **Create** button. In the **Create a proxy** page, click on **Proxy template** dropdown, **you may need scroll down**, and choose **Reverse proxy (Most common)** under the **OpenAPI spec template**.
    
    ![part1-choose-openapispec-template-with-scroll.png](https://cdn.qwiklabs.com/JcL%2FxtaKIqBADdc%2FLdztZwRQL8lrwM%2B%2FMgD2v0f%2FOBs%3D)
5.  Browse the OpenAPI Spec file you downloaded in step 1 above. (The file name should be **hipster-products-backend-spec-v3.yaml**). Then click **NEXT**. Notice that, Apigee UI recognizes the information stated in the OpenAPI spec file and automatically populated the details. We're going to replace them with the following values:
    
    *   Proxy Name: `Hipster-Products-API`
        
    *   Base path: `/v1/hipster-products-api`
        
        **Note:** Please make sure to manually update the **Base path** as suggested below to include the prefix **/v1**.
    *   Target (Existing API): `https://storage.googleapis.com/apigeexlabs/apijam-lab1/`
        
        **IMPORTANT**: In real life, you should enter the Target (Existing API) with the **value of the hostname or IP address of your backend**. For demonstration and purpose, in this lab we have hosted a json file on the GCS bucket. ![part1-createproxy-details.png](https://cdn.qwiklabs.com/Jxk3L6ThEfqZjiP1pAfweUayknUxmADnpstpQ6j3a5g%3D)
    
    Verify the values and click **NEXT**.
    
6.  In the **Flows (optional) section**, you can select which API operations / flows, from the list configured in the OpenAPI Spec. Select all and then click "**NEXT**".
    
    ![part1-select-allflows](https://cdn.qwiklabs.com/O7RR0VN0%2BBdaENGlT6LhS9cNTXfHurCBkY65Eftyp6I%3D) **Note:** Although we choose to import both flows to Apigee Proxy, we'll be only implementing the details for **/products** in this lab.
7.  In the **Deploy (optional) section**, Ensure that the **eval** environment is selected to deploy this proxy to, and click **OK** and then click **Create**.
    
    ![part1-deploy-to-env.png](https://cdn.qwiklabs.com/THT%2BDjamfpuEyoRmY0p0eI3WwWbiDedH8%2B2WkAeBapQ%3D)
8.  You can monitor the deployment progress/status on the bottom right corner. It typically takes about 30 seconds or so for the deployment. You should see the Proxy detail page as follow once the **Hipster-Products-API** has been successfully deployed.
    
    ![part1-successfully-deploy.png](https://cdn.qwiklabs.com/4JaXFuIqSHiItZLtY2CRgNoUPG76yWLcOsTFMhonlv4%3D)

### **Part 1B - Test and debug the API Proxy**

Let us test the newly built API proxy. You can use any HTTP client like cURL, Postman, or your browser URL.

1.  **Get API Hostname**
    
    Before we test the API, to get the hostname details of the proxy, navigate to **Management → Environments** in the left navigation bar. Then click on **ENVIRONMENT GROUPS**. Copy the Host Name value assigned to the "**eval-group**" environment group.
    
    **Note**: If you're using a previously provisioned Apigee org, then you can use the environment group already set up within your org. ![part1-gethostname.png](https://cdn.qwiklabs.com/uoF0P3KTK1oAatH7797wPW4d6PAh14DjhlRFVc35vZs%3D)
    
    This is your **API hostname**. You will use this hostname to invoke your API throughout this lab. Copy it onto a text pad for later use.
    
2.  **Test Using cURL** To test the API using curl or a similar command line HTTP client, use the following command:
    
    ```bash
    curl -X GET "https://{{API hostname}}/v1/hipster-products-api/products"
    ```
    
    Alternatively, you can put that URL in new Chrome tab and test the API from there. If all goes well, you should be able to get the list of Hipster Products in JSON format.
    
    ![part1-curl-success.png](https://cdn.qwiklabs.com/XTyZor5bBgvG9jik2kcn0QAEwdB6n%2FKn90WmOjk5I3g%3D)
    
    If you encounter the 404 Error, refer to the **Troubleshooting Tips** below. Otherwise, please IGNORE them.
    
    **Troubleshooting Tips**  
    Check the following tips:
    
    *   Your API proxy deployment has successfully completed with the values provided. To check, navigate to the **Deployments** section under **Proxy Development**→ **API Proxies**
    *   You've manually updated the **Base Path** to include the **/v1** prefix. To check, from within the Hipster-Products-API proxy **Overview** tab, Under the Revisions section, locate the latest revision, click View (under Endpoint Summary).  
        ![part1-endpoint-summary-view-trimmed.png](https://cdn.qwiklabs.com/Hq9vmk7i7Wm4B%2F4zT06wcXQqPU%2FFqK%2FClNYc3CUGvBk%3D)  
        Cross check if the default Basepath of the Proxy endpoint is configured correctly with **/v1/hipster-products-api**.  
        ![part1-view-revision-endpoint-summary.png](https://cdn.qwiklabs.com/NS9CdMlvBnHTT5zM11ez8WarDKeNIZ35y4%2FNjDeDQ4I%3D)
    *   In case you need to update the base path, go back to the **Develop** tab, and on the left menu, select the default under the Proxy Endpoint. You may need to scroll a bit down on the XML main pane, and update the **Base Path** under the **HTTPProxyConnection**. Save it as a new revision & redeploy your proxy's new revision. ![troubleshoot-edit-prefix-trimmed.png](https://cdn.qwiklabs.com/5vw1n2rPDdTbmdwMBmyo1ygDcz3RBVlA%2FoIQaBe28VQ%3D)  
        If you encounter a warning about routing change, click Proceed.
    
3.  **Using Apigee Debug**
    
    There might be a time where we need to debug for an error or to find potential performance bottleneck. Apigee comes with a handy debugging / tracing capability, referred as Apigee Debug. This capability comes with a user interface allowing you to visualize the API flows in [Gantt chart](https://en.wikipedia.org/wiki/Gantt_chart). Follow these steps to test using the Apigee Debug:
    
    *   Navigate to your API proxy's **Debug** tab. then click **START DEBUG SESSION**.
    *   Ensure that the deployed API revision is selected in the **Env** dropdown. Then click the **START** button. ![debug-start.png](https://cdn.qwiklabs.com/KneXqgV3O3oOvRwKxRBG1Xa2NgWYRzzzWfmfzs4Wc5A%3D)
    *   Wait for the debug session to start.
    *   Make a request to the following URL with tool like POSTMAN or curl command: Modify the URL to use your org's and environment's API hostname - also, don't forget to make sure **/products** is appended to the end of the URL.
    ```
    https://{{API hostname}}/v1/hipster-products-api/products
    ```
    *   You will see that the API proxy received the request and sent back a HTTP status 200 response which was logged by the debug session. Feel free to click the request 200 record, scroll the middle section on to the response, and you will see the response details in the debug session. ![debug-view-content.png](https://cdn.qwiklabs.com/5Sq64xufBZ0FoucBcCGbidMQO3bXTMIFjhkE1Jat9Xk%3D)

### **Part 1C - Export / Import the API Proxy (Revision) Locally**

Any changes made to a proxy will result in a revision. Let's save the API Proxy (Revision) locally as a Zip Bundle in case you need to reuse it in another occassion.

1.  To save the API Proxy, navigate to the **Develop** tab of your proxy, and click the **Export revision** option under the "tripple dots" button next to "Save" button. ![export-revision.png](https://cdn.qwiklabs.com/xhtoSBusjavfrKxX85rR36R%2BdiE%2FxOWJ%2FXWJ%2BP%2B7WNY%3D)
    
    You should then be able to save the zip bundle locally.
    
2.  Similarly, you can Import a revision to a proxy by choosing **Import revision** option.
    

### **Part 1 - Summary**

That completes this first part of this hands-on lab. So far you've learned how to use Apigee to proxy an existing backend using an OpenAPI Specification and the Apigee proxy wizard. You also tested the API Proxy with CURL and Apigee Debug tool. Finally, you could export an API Proxy Revision locally.

#### **References**

##### **Apigee Documentation**

Useful Apigee documentation links on API Proxies:

*   [Build a simple API Proxy from an OpenAPI Spec](https://cloud.google.com/apigee/docs/api-platform/tutorials/create-api-proxy-openapi-spec)
*   [Apigee's API Deployment Lifecycle](https://cloud.google.com/apigee/docs/api-platform/fundamentals/api-development-lifecycle)

#### **Questions for discussions**

1.  What exactly the Deploy step is for? Can I create a proxy without deploying it to an environment?
2.  Is it a mandatory to place /v1 as prefix in the API Proxy's base path? What is the impact, if we don't do that?
3.  Can we create a API proxy without any backend?
4.  Refering to the Apigee Debug tool in Part 1B, notice that there's a dropdown called Filter, what is the purpose of it?

In your other window from where you started this lab click **Check my progress** to verify that the proxy has been created. API proxy has been created.

Part 2 - API Security and API Producer/Consumer Relationship on Apigee
----------------------------------------------------------------------

_Persona : API Team_

### **Use case**

You have an API that you want to secure and expose for consumption by different Apps (API consumers). In addition to setting up authorized access to the API, you also want to be able to identify Apps by their API Key and control which Apps can make calls to the API. You may also want to customize API behavior based on the calling App, or measure API consumption by different Apps through Apigee Analytics.

### **How can Apigee help?**

#### **API Proxy - API Product - App Relationship**

In order to secure an API Proxy on Apigee, we need to first understand the **relationship between API Proxies, API Products, and Apps**.

While the API Proxy allows you to expose the API endpoint according to API design specification, it also serves to decouple the API backend (target service) from the front end (client Apps), and in turn API production from consumption. This is accomplished by creating "[API Products](https://cloud.google.com/apigee/docs/api-platform/publish/what-api-product?hl=en)", which are configurations that bundle one or more API proxies, and define how the APIs can be consumed. The API Product configuration may contain metadata that defines rules for consumption of the bundles API. These rules may include allowed consumption quota (Eg. 100 API calls per minute), visibility (Public vs Private vs Internal), API resource restrictions (Eg. Only /products resource URL may be called, but not /products/{product ID}), which API deployment environment the caller is allowed to access (Eg. test, prod), etc.

Once API Products are created, client Apps can then subscribe to them. On subscription, Apigee automatically generates and provisions an API Key/Secret pair for the App. These credentials can then be used to call the bundled API resources with authentication and authorization, from within App code.

![image alt text](https://cdn.qwiklabs.com/rpYjuZfKop35oFwoOUo8Jay8DCxSrEB4UQnnm4XCYZU%3D)

#### **API Proxy Configuration**

While Apigee provides multiple ways of securing an API and authorizing API calls - including [API Keys](https://cloud.google.com/apigee/docs/api-platform/security/api-keys?hl=en), [OAuth](https://cloud.google.com/apigee/docs/api-platform/security/oauth/oauth-home?hl=en), [JWT](https://cloud.google.com/apigee/docs/api-platform/reference/policies/jwt-policies-overview?hl=en), and [SAML](https://cloud.google.com/apigee/docs/api-platform/security/saml?hl=en) - this lab will focus on using simple API Key verification to secure an API.

Within the API Proxy, the [Verify API Key Policy](https://cloud.google.com/apigee/docs/api-platform/reference/policies/verify-api-key-policy?hl=en) can be used to authenticate and authorize incoming API calls, based on API Key verification. As a result of successful API Key verification, the Verify API Key Policy also populates the API Proxy runtime context with details about the App making the call, the App developer, the API product associated with the call, and so on. This context can then be used to parameterize other policies applied, in order to affect API behavior such as quota enforcement or routing based on the client App. The data can also be extracted and used to gain insights through Apigee Analytics.

In this part of the lab, you will:

*   Configure a **Verify API Key** Policy for an existing, insecure API proxy, and use the Apigee Debug tool to see the policy in action.
*   Bundle the API Proxy into an API Product.
*   Register a Developer and an App within your Org, that subscribes to the API Product, to test authorized consumption of the API.

### **Part 2A - Add a Verify API Key Policy to the API proxy**

1.  In your Apigee menu in GCP console, navigate the API proxies from the last part (**Proxy development**), choose the API Proxy which you created in Part 1 (the name should be **Hipster-Products-API**) and click the "**Develop**" tab of the proxy.
    
    ![part2-proxy-develop.png](https://cdn.qwiklabs.com/V3bkADZkvUeuV0NfjZKvu1O2em9aY%2FhxB%2B6h4ba6dDI%3D)
2.  Under the **Proxy endpoints** tree menu, click the **default**. Then UI in the should change to the API flow diagram. You may need to resize the editor by drag down the pane or zoom-out your browser to be able to see full diagram. ![part2-resize-editor.png](https://cdn.qwiklabs.com/Cv%2FDrMdK6EQIHGKWvv1lZWH%2Bn7iNcIsdYC75Wn3hVD4%3D)
    
    Now, pay attention to the flow arrow starting from **API Client => Proxy Endpoint (Request) => Target Endpoint (default) => Proxy Endpoint (Response) => API Client**. This diagram visualizes the flow from the API client being processed by Apigee's API Proxy.
    
3.  Still on the **Proxy endpoint** => **default** tree menu, Click **(+)** on the Request Preflow. Choose **Create new policy** option, and then choose **Verify API Key** policies in the "Select policy" dropdown. And provide the following values:
    
    *   Name: ```Verify-API-Key-1```
    *   Display Name: `Verify-API-Key-1`
    
    Then click ADD.
    
    ![part2-verify-apikey.png](https://cdn.qwiklabs.com/e5iF7oCelV5cnIS%2B0LcKs34OUdRpn8SFnoHzXfNLP0A%3D)
    
    **Note**: If you perform this lab with [new / updated attribute PAYG billing model](https://cloud.google.com/apigee/docs/api-platform/reference/pay-as-you-go-updated-overview), you may notice that there's slight UI difference in the **Create new policy** step, which display [Standard and Extensibe Policies type](https://cloud.google.com/blog/products/api-management/updates-to-apigee-api-management-pricing-models).
    
4.  With the **Verify API Key** policy selected, you can see its configuration (the default policy configuration is below). Note that the API Key to check is being retrieved from the API proxy's runtime context, when an API call is made, using the context variable **request.queryparam.apikey**. This is the default but the policy can be configured to retrieve the key from any parameter you prefer, for example, **request.header.client-id** , etc.  
    ![verify-api-key-xml.png](https://cdn.qwiklabs.com/EZCU6%2FWuR3CL%2BFl06ZO1M1o7CnarwO8tEfJN6xML0os%3D)
    
5.  Save the API proxy. If it prompts you to **Save as new revision?**, do so by clicking **SAVE AS NEW REVISION**.
    
6.  Click the **DEPLOY** button and then when the Deploy pop-up appears, make sure that you’ve chosen the latest revision, ensure that the **eval** is chosen in the **Environment** and then click **Deploy**. Then click **CONFIRM** on the confirmation dialog box.
    
    ![deploy new revision.png](https://cdn.qwiklabs.com/jJgJG9WDX76G1C1oZzzk2LjglP%2FvdXsjTMIv%2F3JBsWc%3D)
7.  The deployment of the API proxy to the environment typically takes about **30 seconds or so**.
    
8.  Once the new revision has been successfully deployed, click the **Debug** tab for the proxy, to test the now secured API, then click **START DEBUG SESSION**, choose the eval environment and then click **Start**.
    
9.  Make a request to the following URL with tool like POSTMAN or curl command:
    
    ```
    https://{{API hostname}}/v1/hipster-products-api/products
    ```

    Modify the {{API hostname}} to use your Apigee environment hostname (found at Management → Environments → Environment Groups).
    
10.  You should see the error response message as:
    
    {"fault":{"faultstring":"Failed to resolve API Key variable request.queryparam.apikey","detail":{"errorcode":"steps.oauth.v2.FailedToResolveAPIKey"}}}
11.  Navigating back to the **Debug** tab, you should see a 401 (unauthorized) response for your API Call as shown below. This is because the API proxy was expecting an API Key as a query parameter. ![debug verify api key failed.png](https://cdn.qwiklabs.com/%2Bh9HiqLou51eiJzXlw3rCCTwyxl7sXW67LMK7CpPZPY%3D)
    

In the next steps, you will get an API Key that will allow you to make this API call successfully.

### **Part 2B - Create API Product**

1.  In the GCP Console, click the **Distribution => API Products** from the side navigation menu, and click the "**\+ Create**" button.
    
    ![part2-create-api-products.png](https://cdn.qwiklabs.com/2XfOpopp7YrAwpIkQlGPtGQSSOzxk99TgZFviyD3uSk%3D)
2.  Populate the following fields under the **Product details**.
    
    **Note**: If you're using an Apigee org that you share with others, prefix all assets including API product names with \*your initials\* , so as to not accidentally modify others' work. Eg. API product 'Name = xx\_Hipster-Products-API-Product'.

*   Name: `Hipster-Products-API-Product-Silver`
    
*   Display name: `Hipster Products API Product Silver`
    
*   Description: `Free version of the Hipster Products API.`
    
*   Environment: `eval`
    
*   Access: `Public`
    
*   Quota: `5 requests every 1 minute`
    
    **Note:** API products have a set of fields called "Quota" that allow you to configure how many requests per number of time periods (e.g. 5 requests per 1 \[minute/hour/day/month\]) you want to allow. Just configuring this does NOT enforce quotas though! Think of these fields as metadata that the Quota Policy (enforcement point) can dynamically reference when enforcing the policy. In the next part, you'll see how it will take new effect after applying Quota policy.
    
*   Under Operation section, click the **Add an Operation**.
    
*   As the Operation pane appears on the right of the screen, choose the **Hipster-Products-API** from the API Proxy dropdown.
    
*   For Path, add the `/` wildcard.
    

**Note**: Here we are adding the entire API Proxy to the API Product. We can just as easily select specific API resource paths alone from the proxy, and bundle them together in an API Product.

*   Click **Save** on the Operation pane.
*   **Save** the API Product. ![part2-create-api-product-hipster.png](https://cdn.qwiklabs.com/cxwFimYIfAKz6nhLJWaw7uI1NZk36cT7Uu0tZHyLwRg%3D)

### **2C - Manually create a developer GCP console**

The term developer here refers to API consumers or API client.

1.  Navigate to **Distribution → Developers** in the side navigation menu, and click the "**CREATE**" button to create a developer.
    
2.  Populate the following details:
    

*   First Name: `Test`
*   Last Name: `Developer`
*   Email: `testdev@test.com`
*   Username: `testdev`

3.  Click "**ADD**".
    
    ![part2-create-developer](https://cdn.qwiklabs.com/UKaN3Uamp3fSbT5ybyFqeQMrLPA9l%2BWD9Yqz2mz2Kyw%3D)

### **2D - Manually register App in Management UI & generate API Key**

1.  Navigate to **Distribution → Apps** from the side navigation menu, and click the **CREATE** button.
    
2.  Populate the following details:
    

*   Name: `Hipster-Android-App-Basic`
*   Display Name: `Hipster Android App Basic`
*   Select the developer you previously created. If you are unable to see the developer, refresh your browser.

3.  Under Credentials section, click the **Add Credential** button.
    
4.  As the Add Credential pane appears on the right, choose the **Hipster Products API Silver** which you previously created.
    
5.  Click "**Add**".
    
6.  Click the "**Create**" button to save. ![part2-create-app-basic.png](https://cdn.qwiklabs.com/LxrvsmxDwWeXSCzUFOMYXL2eqrfr%2BhQHQ24Ium5llKY%3D)
    
7.  Once created, you will then see that Apigee has generated an API Key and Secret pair for the App. Click on the "**Show/Hide**" (eye icon) link next to the Key, and copy this API Key.
    

![part2-get-appkey.png](https://cdn.qwiklabs.com/gR6IfUB%2BFWRGdcxlFaSR4OUpkEwaLB4ltJ03aWBnMjM%3D)

You can now use this API Key to make a valid API request to your API Proxy.

### **2E - Test your API with a valid API Key**

1.  Navigate to your API proxy's "**Debug**" tab.
    
2.  Click "**START DEBUG SESSION**".
    
3.  Ensure that the deployed API revision is selected in the "**Env**" dropdown.
    
4.  Click the "**START**" button.
    
    *   Wait for the debug session to start.
5.  Make a request to the following URL with tool like POSTMAN or curl command:
    
    ```
    https://{{API hostname}}/v1/hipster-products-api/products?apikey={{API Key}}
    ```

    In the above URL:
    
    *   {{API hostname}} is the hostname for your environment group that you copied previously from **"Management" >> "Environments" >> "Environment Groups" >> your environment group**, and
    *   {{API Key}} is the API key that you just copied from the newly created App configuration
6.  You should now see that the API request returns a 200 OK response, as the Verify API Key policy has found the API key to be valid.
    
    ![verify api key debug successful.png](https://cdn.qwiklabs.com/dAJf9BJEX3j7xu%2B5e3Me48sJBw9FGN%2BFJXLkdNfR09g%3D)

### **Part 2 - Summary**

In this part of the lab, you learned how the relationship between API Proxies, API Products and Apps, helps obfuscate API production from API consumption; and how to protect your API proxy using the Verify API Key policy. You then implemented the policy and tested it using the built-in Debug Tool.

#### **References**

##### **Apigee Documentation**

*   [Verify Api Key Policy Documentation](https://cloud.google.com/apigee/docs/api-platform/reference/policies/verify-api-key-policy)
*   [Introduction to API Products](https://cloud.google.com/apigee/docs/api-platform/publish/what-api-product)
*   [Registering app developers](https://cloud.google.com/apigee/docs/api-platform/publish/adding-developers-your-api-product)
*   [Controlling access to your APIs by registering apps](https://cloud.google.com/apigee/docs/api-platform/publish/creating-apps-surface-your-api)

#### **Questions for discussions**

1.  Why are there Proxy Endpoint and Target Endpoint? Can you think of any scenario it's useful?
    
2.  Can we have more than one Proxy Endpoint and Target Endpoint?
    
3.  In the lab, you were asked to put Verify API Key policy in the Proxy Endpoint's Request Preflow. Can we place it somewhere else like Proxy Endpoint's Request GetProducts flow or Proxy Endpoints's Request Preflow or even anywhere in the Target Endpoint? What's the impact?
    
    ![part2-questions3.png](https://cdn.qwiklabs.com/O3wAFP2GsEp0LEE9dciLSs0ZYVv0VWhjS8tfUg%2FGofE%3D)

In your other window from where you started this lab click **Check my progress** to verify that you've secured your API proxy with a Verify API Key policy. API proxy has been secured with API Key.

Part 3 - Manage tiered API Product subscription through API call quotas and dynamic response
--------------------------------------------------------------------------------------------

Persona : API Product Team & API Dev Team

#### **Use Case**

Call Quota: API tiering is a new look at API as a Product. With tiering you provide the base level (e.g. silver) as a basic option. This offer is an entry point to leverage your data offering with a potential upsell to premium API Products. The goal is to upsell to additional functional levels. This term is also known as "freemium". The basic approach is as follows – offer basic functions or call quotas as an entry level and if more data access or more functionality is required, offer these options for a fee. This gives developers the chance to have a working prototype and explore your API in a real life scenario before making an informed purchase decision.

Different response result based on the subscription tier: Have you come across the use case in which the Premium subscribers would get all (complete) result, while the Basic subscribers would get only a subset of the result?

#### **How can Apigee help?**

Apigee offers the concept of API Products abstracted from the functional logic of API proxies. The proxies can access and enforce the limits defined in API Products. This way the API product team can focus on the business model (e.g. establishing usage quotas and entitled API operations on a per API Product basis) while the API development team works with these values to implement the parametrized behavior.

You could leverage extension policy such as Javascript to inject additional logic to remove a subset of the result for the Basic subscribers.

### **3A - Create and Configure Quota Policy**

As stated, quotas are only enforced by adding a Quota Policy into your API Proxy. With the configuration of the Quota fields in the API Product, when an API call is made that presents a valid API key, Apigee will automatically fetch the associated API Product's metadata (including the Quota fields), which become available to be dynamically referenced by a quota (or any other) policy.

1.  Navigate to **Proxy Development → API Proxies** from the side navigation menu and open the existing API Proxy that you previously created.
    
2.  Navigate to the **Develop** tab of the proxy, and verify that the policy for Verify API Key exists with the proper policy name. Click on the policy and look at the XML configuration below for policy name.
    
    ![verify api key before quota.png](https://cdn.qwiklabs.com/QwQZ090BofaNGZZDrHK8awZaUYGen8jyBzF%2FYFUncbE%3D)
3.  Still on the **Proxy endpoint** => **default** tree menu, Click **(+)** on the **Request** **Preflow** (in which you should see the **Verify-API-Key-1** as well there). Choose Create new policy option, and then choose **Quota** policies in the **Select policy** dropdown. Provide the name of **QU-ProductQuota** for both **Name** and **Display name**. Then click **ADD**.
    
    ![part3-add-quota-policy](https://cdn.qwiklabs.com/mRjMVJUPrjcVSRTkFyqog02mS5Azynja18tPMdEHscc%3D)
4.  Upon added, the QU-ProductQuota policy should appear right below the Verify-API-Key-1 policy. ![part3-quotapolicy-added](https://cdn.qwiklabs.com/jCuwm%2B4q4uSGD%2BzybDOpY776eoF7j1qKSdExdCuO378%3D)
    
5.  In your proxy, with the VerifyAPIKey policy that you have configured in the previous steps, the following variables should be populated in the proxy's runtime context during an API call, after verification of a valid API key
    
    *   verifyapikey.Verify-API-Key-1.apiproduct.developer.quota.limit
    *   verifyapikey.Verify-API-Key-1.apiproduct.developer.quota.interval
    *   verifyapikey.Verify-API-Key-1.apiproduct.developer.quota.timeunit
    
    You will now use these variables to parameterize the Quota policy and enforce a dynamic quota based on whichever API product the key corresponds to.
    
6.  Click the QU-ProductQuota policy.
    
    ![modify quota policy](https://cdn.qwiklabs.com/qsw2QrjrJwqxrEmf5zkznhNjHpTqSh%2FBGJ8zKzDZ4cM%3D)
    
    Replace the content of QU-ProductQuota.xml with the following:
    
    ```xml
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?> 
    <Quota async="false" continueOnError="false" enabled="true" name="QU-ProductQuota">
        <DisplayName>QU-ProductQuota</DisplayName> 
        <Allow count="3" countRef="verifyapikey.Verify-API-Key-1.apiproduct.developer.quota.limit"/> 
        <Interval ref="verifyapikey.Verify-API-Key-1.apiproduct.developer.quota.interval">1</Interval> 
        <TimeUnit ref="verifyapikey.Verify-API-Key-1.apiproduct.developer.quota.timeunit">minute</TimeUnit> 
        <Distributed>true</Distributed> 
        <Synchronous>true</Synchronous>
    </Quota>
    ```

    In the above Quota policy configuration, if the field is not set in the API product, this would allow a default of 3 calls per minute.
    
7.  Click **SAVE**, and then click **SAVE AS NEW REVISION** and click **Deploy**.
    
    **Note:** Any context variables populated as a result of a policy's execution, will have the policy's name (as defined in your proxy), as part of the variable name in ‘.' (dot) notation. For instance, in the previous step if you had named your Verify API Key policy "Check-API-Key", you would find the "limit" variable (derived from API product metadata associated to the key) in the runtime context variable named: \`verifyapikey.Check-API-Key.apiproduct.developer.quota.limit\`

### **3B - Create a Gold API Product**

You have created a Silver API Product in part 2, in this section we will create another API product (Gold tier) that bundle the same API Proxy but with different quotas attached to it. The Gold tier is targeted for Premium Subscribers (Developers).

1.  In your GCP Console, navigate to **Distribution → API Products**.
2.  Click "**Create**".
3.  Populate the following fields

*   Product details
    
*   Name: `Hipster-Products-API-Product-Gold`
    
*   Display name: `Hipster Products API Product Gold`
    
*   Description: `Premium version of the Hipster Product API`
    
*   Environment: `eval`
    
*   Access: `Public`
    
*   Quota: `50 requests every 1 minute`
    
    **Note:** API products have a set of fields called **Quota** that allow you to configure how many requests per number of time periods (e.g. 5 requests per 1 \[minute/hour/day/month\]) you want to allow. Just configuring this does NOT enforce quotas though! Think of these fields as metadata that the Quota Policy (enforcement point) can dynamically reference when enforcing the policy.
*   Under Operation section, click the **Add an Operation**.
    
*   As the Operation pane appears on the right of the screen, choose the **Hipster-Products-API** from the API Proxy dropdown.
    
*   For Path, add the `/` wildcard.
    
    **Note**: Here we are adding the entire API Proxy to the API Product. We can just as easily select specific API resource paths alone from the proxy, and bundle them together in an API Product.
*   Click **Save** on the Operation pane. (But DO NOT click SAVE on the API Product yet)
    
    ![part3-create-goldproduct.png](https://cdn.qwiklabs.com/x606iXyBiEjw3azxodnfp9r3ZKyUOlZwOACMO%2Fgza7o%3D)

4.  **Save** the API Product. (You may need to scroll up a little bit to see the SAVE button)
    
5.  Navigate to the **Distribution => API Products**, you should have two API Products now.
    
    ![part3-apiproduct-list](https://cdn.qwiklabs.com/wWpFPtZk5yGpUxMV5lTIUouOpF2v0z9P%2BUbFZIeH%2F88%3D)

### **3C - Create Premium App for the Gold Tier API product**

1.  Select **Distribution** → **Apps**, click **CREATE**
2.  Populate the following fields

*   App Details
*   Name: `Hipster-Android-App-Premium`
*   Display name: `Hipster Android App Premium`
*   Developer: Chose any existing Developer

3.  Under **Credentials** section, click the **Add Credential** button.
    
4.  As the **Add Credential** pane appears on the right, choose the **Hipster Products API Gold** which you previously created.
    
5.  Click **Add** on the **Add Credential** pane.
    
    ![part3-add-premium-app.png](https://cdn.qwiklabs.com/fie0j35XKfBsPQO80%2FQxdv04ieZfhtxRA5zy9ZD2PL0%3D)
6.  Click CREATE to create this app.
    
7.  Navigate back to the **Distribution** → **Apps**, you should see two apps (Hipster Android App Premium and Hipster Android App Basic).
    
    ![part3-applist.png](https://cdn.qwiklabs.com/fR9Dcb6mTXoQMk90TMYkwoSBZ4E77p2q5VVwFn0U9Ns%3D)

### 3D - Test Quota for different App

Now, we have two pair of "Product-App", which we have dynamically configured the quota in the API Product. Let's test them out.

1.  Still in the **Distribution** → **Apps** menu, first click **Hipster Android App Basic**, then copy they **Key** (not the **Secret**). Then paste it to your editor like VS Code or Notepad.

    ![part3-getappkey.png](https://cdn.qwiklabs.com/4PZzrDSYW%2FCh6CX8KmBshqGGE06a3b8GXLCSQtkNTBE%3D)

2.  Now, let's make at least 7 continuous requests to the following URL with tool like POSTMAN or curl command:
    
    ```
    https://{{API hostname}}/v1/hipster-products-api/products?apikey={{API Key}}
    ```
    
    In the above URL:
    
    *   {{API hostname}} is the hostname for your environment group that you copied previously from **"Admin" >> "Environments" >> "Groups" >> your environment group**, and
    *   {{API Key}} is the API key that you just copied from the newly created App for the **Silver** tier, i.e. `Hipster Android App Basic`.
3.  If it all goes well, you should be able to get the valid result for the first 5 times. However, you should get an error on the 6th time onward.
    
    ```
    {"fault":{"faultstring":"Rate limit quota violation. Quota limit exceeded. Identifier : \_default","detail":{"errorcode":"policies.ratelimit.QuotaViolation"}}}
    ```

    This is because, we defined 5 call per minute.
    
4.  If you wait for another minute or so before making another call, you should be getting a successful result since the quota has been reset for the next minute.
    
5.  Repeat the same for another App (Hipster Android App Premium). Can you call more than 5 times? You should be able to! In fact, we set it as 50 call per minute for the Gold Product.
    

### 3E - Modify Response Dynamically Based on Tier

Have you ever come across a scenario where the Silver's API consumer (Basic App) only get a subset of fields, while the Gold's API consumer (Premium App) will get a complete list of fields?

While you can achieve this typically with two different API proxies, this would require you need to deal with additional overhead.

Apigee allows you to do it with much elegant way with the combination of Apigee Proxy, Javascript Policy, and Custom Attribute in the API Product.

Let's learn how to do this!

1.  Recalling what response did you get when successfully calling the Hipster Products API. **Refer to Part 1B step 2** above. You should see a list of products with the attributes of **id, name, description, picture, priceUsd, and categories**. What we are going to do in this section is to remove two of the attributes if the subscribed-app is not Gold (ie Silver). This following diagram illustrates the idea.
    ![part3-modify-response-diagram.png](https://cdn.qwiklabs.com/Pe83NE6b09REi%2BQlHluZK9X3hpt6j55tZUTUB8itos0%3D)
    
2.  Make sure that you've clicked the **default** in the **Proxy Endpoint** so that the diagram shows correctly in the center. Click **(+)** on the **GetProducts** in the **Response** (not in the Request). 
    ![part3-add-javapolicy.png](https://cdn.qwiklabs.com/SDJu52mLoNk8RZrbp%2FZqWfQblcdO%2FuQ9WwN3tE3mxCc%3D)

3.  Once the Add policy step pane appears on the right, choose **Create new policy** option and browse for **Javascript** in the **Select policy** dropdown. Enter `JS-ModifyResponseBasedOnTier` for both **Name** and **Display name**.
    
4.  In the Javascript file field, choose **CREATE NEW RESOURCE**. As soon as the **Add resource** pane appears, ensure that **Javascript** is being selected in the **Resource type**, **Create new file** in the **Source**. You may leave the **Resource name** as it (`Resource-1.js`). Then click **ADD**. 
    ![part3-addjavascript-resources.png](https://cdn.qwiklabs.com/0oSrntNbWKhPtqeC87iJ%2BqZVgvfH3l9gLUhv7AGTxnk%3D)

5.  You should be routed back to the **Add policy** step pane. In the **Javascript** file field, ensure that the `Resource-1.js` file has been selected. Then click **ADD**.
    
6.  The editor diagram should look like this. Notice that there are two items being added: The `JS-ModifyResponseBasedOnTier` under **Policies** and `Resource-1.js` under the **Resources/jsc**. 
    ![part3-after-adding-js.png](https://cdn.qwiklabs.com/Y0poSgMYjxcgyYqavxQgAf4DfBXZFny55XDpX65UHWs%3D)
    
7.  Click **Resource-1.js** file (on the left pane tree menu) and copy the following code snippet to the Resource-1.js code editor pane.
    
    ```js
    if(response.content && response.content != "") { 
        var data = JSON.parse(response.content); 
        var tier = context.getVariable("verifyapikey.Verify-API-Key-1.apiproduct.Tier"); 
        if(!tier || tier !== "Gold") { 
            for(var i=0;i < data.products.length; i++) { 
                delete data.products\[i\].priceUsd; delete data.products\[i\].categories; 
            } 
        } 
        context.proxyResponse.headers\['tier'\] = tier; 
        context.proxyResponse.headers\['content-type'\] = 'application/json'; 
        context.proxyResponse.content = JSON.stringify(data, null, 2); 
    }
    ```
    
    We use [JavaScript object model](https://cloud.google.com/apigee/docs/api-platform/reference/javascript-object-model) in our JavaScript policy to perform some manipulation. The code above removes **priceUsd** and **categories** in each Hipster Product when the Tier is not **Gold** or custom attribute **Tier** is not set. The we output the modified response's content. Additionally, we also add a response header called **Tier**. 
    ![part3-add-js-resource-code.png](https://cdn.qwiklabs.com/DxL3qFYvNOAKdYl0WcayS9MrBrlLz445kY%2Bd5ZpWojE%3D)
    
8.  Click **SAVE**, and then click **SAVE AS NEW REVISION**.
    
9.  Click **DEPLOY**. As soon as the **Deploy Hipster-Products-API** pane appears, choose the latest revision and the appropriate environment (which should be **eval**). Click **Deploy**.
    
10.  Now we need to edit the Gold's API Product. Under the **Distribution -> API Product**. Locate the **Hipster-Products-API-Product-Gold** and click on it. Click Edit and scroll down to the end of the **API Product** page, under the **Custom Attributes** section, click **ADD CUSTOM ATTRIBUTE**. As it pops up, enter `Tier` in **Name 1** and `Gold` in **Value 1**. Click OK on the Custom attributes page. 
    ![part3-add-custom-attribute-goldproduct](https://cdn.qwiklabs.com/5GXOUJur%2FWtTVsMSfWkB44YzlzoLv43Z%2FUj5Ci2XpKk%3D)

Scroll up and click **SAVE** for the **API Products** page. 11. \[Optional\], you may do the same for the Silver Product but it won't make any difference in this case. This is because the Javascript code above will just remove the **priceUsd** and **categories** if the Tier is not Gold.

### **3E - Test the tiered product quotas and response**

Now, let's test the product quota together with the Javascript, this time with the Apigee Debug tool.

1.  Navigate to your API proxy's "**Debug**" tab.
    
2.  Click "**START DEBUG SESSION**".
    
3.  Ensure that the deployed API revision is selected in the "**Env**" dropdown.
    
4.  Click the "**START**" button. Wait for the debug session to start.
    
5.  Make a request to the following URL with tool like POSTMAN or curl command:
    
    ```
    https://{{API hostname}}/v1/hipster-products-api/products?apikey={{API Key}}
    ```

    In the above URL:
    
    *   {{API hostname}} is the hostname for your environment group that you copied previously from "Admin" >> "Environments" >> "Groups" >> your environment group, and
    *   {{API Key}} is the API key that you just copied from the newly created App for the **Silver** tier, i.e. `Hipster Android App Basic`.
6.  You should be getting the list of Hipster Products **without priceUsd and categories**.
    
7.  Repeatedly, make the same requests multiple times.
    
    After 5 calls we see that our free quota of 5 calls is exceeded and the quota policy shows a red exclamation mark sign
    
    ![quota exceeded error.png](https://cdn.qwiklabs.com/DzIC9ie3bYu79cPA5IZcneSqSEy4q7fO%2BvXiGwy8Flk%3D)
8.  To test the next API product tier i.e. **Gold**, use the API Key generated for the "Premium" app. Change your `apikey` parameter to match your **Premium App** credentials.
    
9.  You should be getting the full attributes of the Hipster Products (including priceUsd and categories). With this (**Gold**) tier, you can also call beyond up to 50 calls per minute. Check if there's a Tier header in your HTTP response.
    

### **Part 3 - Summary**

In this part of the lab, you have created another products aligned with your API product strategy to offer a tiered model and have different quotas attached to each product. We have not defined the limits in our API proxies but made the same proxy available in different API products that define the quota amount. We also used Javascript policy to modify the response based on the Tier and send response back to the client. With this technique, you just need to reusing a single API Proxy to be used by multiple API Products. The following table summarize API Products (with the respective quota / attribute configured), apps, and affected response.

![part3-summary-table.png](https://cdn.qwiklabs.com/HPyMShWIDsa2spfDkm82sPVKowp3fsmWk4eALgGJb%2Bg%3D)

#### **References**

##### **Apigee Documentation**

*   [Quota policy documentation](https://cloud.google.com/apigee/docs/api-platform/reference/policies/quota-policy)
*   [JavaScript policy documentation](https://cloud.google.com/apigee/docs/api-platform/reference/policies/javascript-policy)
*   [JavaScript object model](https://cloud.google.com/apigee/docs/api-platform/reference/javascript-object-model)

#### **Questions for discussions**

1.  How do you enforce an expiry date or duration of an API Key of your App?
2.  With the Javascript policy (and also Python or Java policy), does it mean that I can write any logic / code that I want? What are the impact?
3.  Apigee also has another policy called [Assign Message](https://cloud.google.com/apigee/docs/api-platform/reference/policies/assign-message-policy) policy. Can we achieve the similar result with it?

In your other window from where you started this lab click **Check my progress** to verify that you've created tiered API products and apply quotas. API Products and Apps have been created.

Part 4 - Build an App Developer Experience using Apigee's Integrated Developer Portals
--------------------------------------------------------------------------------------

_Persona : API Team_

#### **Use case**

You want to provide and manage an easy, self-service on-boarding experience for app developers who wish to consume your API Products via a Developer Portal. You want to enable app developers to learn about, register for, and begin using your APIs, as well as control visibility and access to different API Products.

#### **How can Apigee help?**

Apigee provides multiple options for your Developer Portal. Apigee supports several developer portal solutions, ranging from simple turn-key to fully customizable and extensible. The turn-key [Integrated Developer Portal](https://cloud.google.com/apigee/docs/api-platform/publish/portal/build-integrated-portal) option supports branding and customization of much of the site, such as theme, logos, and page content, and can be published in seconds, directly from the management UI. We also provide a [Drupal-based portal](https://cloud.google.com/apigee/docs/api-platform/publish/drupal/open-source-drupal-8) if you want full control and to leverage any of the hundreds of Drupal modules available in the Drupal Market. Alternatively, you could also build your own, [fully customized portal](https://cloud.google.com/apigee/docs/api-platform/publish/intro-portals#custom-portal) leveraging Apigee APIs. This lab focuses on the Integrated Developer Portal.

![image alt text](https://cdn.qwiklabs.com/xYRN7I3XiPB2IhBm4dm152DWeKvIHTznMpnalVA0FKE%3D)

In this lab, you will create an Integrated Developer Portal wherein you will publish API Products, and through which app developers can

*   Learn API usage through OpenAPI specification based interactive documentation
*   Register Apps that consume API Products, and thereby
*   Generate App client credentials (API Key and Secret) that can be used to consume APIs.

### **Part 4A - Update API Proxy for CORS Support**

CORS (Cross-origin resource sharing) is a standard mechanism that allows JavaScript XMLHttpRequest (XHR) calls executed in a web page to interact with resources from non-origin domains. CORS is a commonly implemented solution to the "[same-origin policy](https://en.wikipedia.org/wiki/Same-origin_policy)" that is enforced by all browsers. For example, if you make an XHR call to your API Proxy from JavaScript code executing in your browser, the call will fail. This is because the domain serving the page to yours browser is not the same as the domain serving your API, eg. "{your org name}-{environment name}.apigee.net". CORS provides a solution to this problem by allowing servers to "opt-in" if they wish to provide cross-origin resource sharing.

1.  Navigate to your **Hipster-Products-API** API Proxy, make sure you've selected the **default** from the **Proxy endpoints**. Click (+) in the **PreFlow** in the **Request**. As soon as the **Add policy step** appears on the right, find **CORS** in the **Select policy** dropdown. Enter `CORS-DevPortal` for both **Name** and **Display name**. Then click **ADD**. ![part4-add-cors](https://cdn.qwiklabs.com/UpAVjL0MuaOwdV%2Fdly%2FQj7x6%2F%2FL%2F99wXcXpZTyfaHWo%3D)
    
2.  Click the CORS-DevPortal policy. You should see that the CORS configuration (in XML) shown in the lower part of the pane. You may modify the values (such as AllowOrigins, AllowMethods, etc.) to suit your requirement. But we shall leave it as it for now. ![part4-add-cors](https://cdn.qwiklabs.com/S5FWB9Wmof3DobRwfTL8bbuhNZ2iqwcmQvKreiz6y9k%3D)
    
3.  Click **SAVE** and **Save as a new revision**. Then **DEPLOY** the latest revision to the eval environment.
    

### **Part 4B - Create a Developer Portal**

1.  For this task, we need to access to Apigee (classic) UI by opening a new browser tab and enter [https://apigee.google.com](https://apigee.google.com/). Alternatively, you can navigate to the **Distribution => Portals** and click **GO TO CLASSIC APIGEE UI**. Make sure that the Apigee Org is correctly selected.
    
2.  Navigate to **Publish → Portals** and click the "**\+ Portal**" button, or "**Get started**" (if you haven't created any portals yet within the org). ![cf227285e5388603.png](https://cdn.qwiklabs.com/aHk80JJKKk7DKTWw0pE7fVXcTUWwwyiAhoxhAV2oYGQ%3D)
    
3.  Enter details in the portal creation wizard.
    

*   Name: `Hipster API Portal`
*   Description: `Developer portal for consumption of Hipster APIs.`

3.  Click **Create**.
    
    ![bd893c7f652d8046.png](https://cdn.qwiklabs.com/S8lwPuoh0Dw7AeVcMgyIYeBnO92bkFyLq3o%2FvSYaj6Q%3D)

### **4C - Update Hostname of OPEN API Specification file**

As we're going to publish API documentation to our developer portal, we need to update the servers-url of in the Open API Specification file which you've downloaded earlier in [Part 1A](#part-1a---create-an-api-proxy).

1.  Locate and the yaml file (the file name should be **hipster-products-backend-spec-v3.yaml**).
    
2.  Open the file with your preferred text editor
    
3.  Uncomment **servers - url**. Under the **servers - url**, enter the following value:
    
    ```
    https://{{API hostname}}/v1/hipster-products-api
    ```

    **Be mindful about the yaml's indent. The "- url" should be right below the "v"** Here's an example:
    
    ![part4-update-hostname-yaml-file-with-indent.png](https://cdn.qwiklabs.com/ZSmwdvld%2FRzSoS3hoJk1HZhDkEOrhpBiCr44ZullqYI%3D)

### **4D - Publish the Gold API Product to the Portal**

1.  On the portal ovserview page, click the "API catalog" button.
    
    ![f1e8ade23c859c6e.png](https://cdn.qwiklabs.com/DPAwfYEvA6FFklfepVfVB7U65iAiQvkpk7r8J8YgOt4%3D)
2.  Click the "**+**" button and select the Gold API Product to publish to the Portal. Select "**Hipster Products API Product Gold**" to publish and click "**Next**".
    
    ![part4-devportal-apicatalog.png](https://cdn.qwiklabs.com/mKKMca8bE2GfBQcqsjt1%2FrMtDDHrEImz07p6MUCXOkg%3D)
3.  Make sure that the "**Published**" checkbox is checked, so that the Gold API Product is visible to authorized App developers through the API catalog, during App creation.
    
    ![part4-apicatalog-details.png](https://cdn.qwiklabs.com/NKd%2Bzb4l6tnpImLzmClSBXozJuOvvow7phZ8%2FDcoWec%3D)
4.  Select the "**Registered users**" option so registered developers on the Developer Portal can view this API through the portal.
    
5.  Click the "**Select image**" button to update the icon image associated with this API product.
    
    ![image alt text](https://cdn.qwiklabs.com/NzvbT4EzT8PZVBWXTP8ybD%2ByFCQHvb0gStAn0jO067I%3D)
6.  Then select "**Image URL**" and provide the following URL to import the image.
    
    ![4878e513e51ba764.png](https://cdn.qwiklabs.com/Efc9Z5r14cXCRcvRXhRvgghI9TMQdvZvV%2BHsWjmfTHg%3D)
    
    Image URL: [https://storage.googleapis.com/apigeexlabs/apijam-lab1/HipsterAPIProductImage.png](https://storage.googleapis.com/apigeexlabs/apijam-lab1/HipsterAPIProductImage.png)
    
    ![df65aa5b8e3fdbda.png](https://cdn.qwiklabs.com/xIuyXy077j88e9EIxBA%2BPVJj0IM6QSznsYBzQrcKL%2BU%3D)
7.  Navigate to the bottom of the page. Under the API Documentation, choose **OpenAPI Document**. Click **Select Document**.
    
8.  Upload your local "**hipster-products-backend-spec-v3.yaml**" file that you created and modified in the previous steps. Then click **Select**.
    
    ![b18e1baa0290ecd5.png](https://cdn.qwiklabs.com/RX12vKyYDZsiYQosA%2Fi7uvJTc9l%2Br5G83zxnDUVWjLI%3D)
9.  Then click **Save** to save and publish the API product to the API catalog.
    

The API product is now published to the developer portal.

### **4E - App Developer sign-up**

We created app developer from the GCP Console (From Administrator perspective) in step 2C. In this step, we allow the self-service registration through the developer portal. As an administrator, you can control the if the self-registration require moderation or approval step.

1.  Click the "**Live Portal**" link to launch a browser tab/window with the new Developer Portal.
    
    ![image alt text](https://cdn.qwiklabs.com/tBm%2BrfYM79n3nxyYubCF37mssqyQ6mhiqX%2F%2Bo0e0Lik%3D)
2.  On the developer portal, click the main menu option labeled **Sign In**.
    
    ![image alt text](https://cdn.qwiklabs.com/yxNZJUxbVTvqjMWkzIkVieRudWPJULssm7HV5LAjEbo%3D)
3.  This will take you to the App Developer login page. Here, click **Create an Account**.
    
    ![7365676c852a1b44.png](https://cdn.qwiklabs.com/84V%2B0q5axC7OXXCSaq5PlA%2BMjF%2BKg2No0YWZbvlzCqk%3D)
4.  Provide the following details, and then click **Create Account**. First Name: `{{your first name}}` Last Name: `{{your last name}}` Email: `{{your email address}}` Password: `{{enter a password}}` Check the "I agree to terms." box
    
    ![6c55cafdcffa0316.png](https://cdn.qwiklabs.com/XZx0Z13%2BV6kUHUDECGA6ziOU1%2FomAQoBF2PXnVt6kls%3D)
    
    You may need to verify a captcha image. You may have to go through a few captcha screens (there may be a few image screens). Finally, click **Verify**.
    
5.  On account creation, the App Developer will receive an email notification with an account verification link.
    
    ![33f46bca085aaf9a.png](https://cdn.qwiklabs.com/3m5N6mUNpMK5X0gghQ0862v%2BrMhRZ4xCJjFXTFNTRck%3D)
    
    Since you have provided your own email address as the App Developer in this lab, you should have received this notification. Click the link or copy and paste it into the browser to verify the account.
    
    ![d100443dea850dec.png](https://cdn.qwiklabs.com/igq7Z%2B2QbBxQSmburOVuJFVwh8sAHkYt6JpnwJ7S6Dg%3D)
6.  Once this account is verified, you can sign in as App Developer into the portal using your credentials.
    
    ![18db17922a1a3196.png](https://cdn.qwiklabs.com/y%2FTzC1jdwYmd%2FvJMdfferibvggtfyKD8UaoJdlys4N8%3D)

### **4F - View API Documentation, Generation of the Sample Codes, and Test API**

1.  Log in as App Developer using the account credentials created in the previous steps. Click on the **APIs** menu link on the Developer Portal. This will take you to the API catalog page. Here you will see that API product we previously published to be visible to all registered developers.
    
    ![4a550c9d30a151f4.png](https://cdn.qwiklabs.com/t7uYbN8c0IYd8wWElRRz%2BnhocdbPPunU2uiCCmrY53A%3D)
2.  Click the **API Product** icon for the Gold product in the catalog, to view its documentation. This will take you to an interactive documentation page generated from the OpenAPI spec that we associated to the API product at the time of publishing.
    
    ![image alt text](https://cdn.qwiklabs.com/Q6GinS0a7yegdYqKg%2BSkKgjL1PkYIfvnbOaV7LNbYsw%3D)
3.  Select one of the API resource paths from the left panel of the docs and click "**Execute**" to test the API. You might then see error such as **Unknown Error** or **401 Unauthorized response** in the right panel.
    
    ![image alt text](https://cdn.qwiklabs.com/j368bFxsYGXD8Mzqw%2FIOTMeFkrCeiwikr%2BebSdpsdNc%3D) This is because you haven't yet registered any Apps and therefore have no API Keys to use for an authorized API call. We will fix this error in the next part.
4.  Click on the `full screen icon` icon next to the **Try this API**. As the pane get expanded, notice that Apigee Developer Portal automatically generates sample script / codes in various platform such as cURL, HTTP, Python, Node.JS, Javascript, PHP, and Java.
    
    ![part4-sample-codes](https://cdn.qwiklabs.com/GbXSebHSR89%2Fw62TlZmNi201ehpCaGEs2VxXcl37shE%3D) This capability allows your app developers to easily copy-and-paste the sample code into their editor / IDE to test your API conveniently.

### **4G - Self-service Apps Registration**

Much like the creation process of app developer, the app developer can also register an app in the developer portal in a self-service mannner.

1.  Navigate to the developer account drop down menu on the top right corner, and select the "**Apps**" link.
    
    ![4954b76ab0dd35a5.png](https://cdn.qwiklabs.com/AXQqMFzNF4RONiyOv%2F3I6T%2BXKfeOkNgzuX7KzrhUKHk%3D)
2.  Click the **New App** button (either on the page itself or on the top panel, as shown below).
    
    ![image alt text](https://cdn.qwiklabs.com/4b31qmo5ql9zournfOt1uyXpAArIFDmTybVXJ3wc6n0%3D)
3.  Enter the following App details and click Create:
    
    *   App Name: `Hipster-Test-App`
    *   Description: `Test app to try out Hipster Products API using the Gold API Product`
    *   Select the **Gold** API Product that is available for subscription. Click **Enable** then click **Save**.
4.  You will find that an API Key/Secret pair has been generated for your newly created App. You can now use this API Key to test the API.
    
    ![8fdb68fe030aa2fb.png](https://cdn.qwiklabs.com/U%2BMxC%2FCKKrSmksWAT1ePAFxkvtLMKQhIPKc%2B8ldn%2F7A%3D)
5.  Navigate to the API Catalog, select the Gold API Product docs, and test out the GET `/products` resource again. This time though, first click on the Authorize button and select the newly created App's credentials to set authorization information (API Key) for the API call.
    
    ![4a550c9d30a151f4.png](https://cdn.qwiklabs.com/t7uYbN8c0IYd8wWElRRz%2BnhocdbPPunU2uiCCmrY53A%3D) ![5c59cc0c136b918d.png](https://cdn.qwiklabs.com/w4WB%2BBOH8CklPQOTfFBB7n%2B9Zdukx%2FEEREe6pAY97%2F0%3D) ![a717430fd7ca709a.png](https://cdn.qwiklabs.com/7166pYTa35tBOUCYajvOhjMwJN43L%2BhIt%2FFpUK1tma0%3D)
    
    Click "**OK**" after authorization information is set.
    
    ![image alt text](https://cdn.qwiklabs.com/K4t6kiLGydtARYkchDzryZe8LQZjFuidaAqVbHGa7No%3D)
6.  Now, click **Execute**. You will see that a valid 200 OK API response is received.
    
    ![image alt text](https://cdn.qwiklabs.com/Cm3jpj1%2B%2FCevttPhTzuUk2DcpUnV%2BooXIJUznOEhEFo%3D)

### **Part 4 - Summary**

You've learned how to do the following:

*   Deploy the Apigee Integrated Developer Portal
*   Publish an API Product with an OpenAPI Specification
*   Use the Developer Portal UI to browse the API catalog, documentation, and sample codes.
*   Register Apps to consume API products as a developer.

#### **References**

##### **Apigee Documentation**

*   [Building your integrated portal](https://cloud.google.com/apigee/docs/api-platform/publish/portal/build-integrated-portal)
*   [Tutorial: Building your first portal](https://cloud.google.com/apigee/docs/api-platform/publish/portal/get-started/overview)
*   [Developer portal solutions](https://cloud.google.com/apigee/docs/api-platform/publish/intro-portals)

#### **Questions for discussions**

1.  How do I add a simple page such as Contact Us form which displays email address, then add it to the navigation menu of my developer portal?
2.  How do I generate another set of credential for my app in the developer portal?
3.  I'd like to have a discussion forum feature in my developer portal. Can I have such capability in the Apigee Integrated Portal? Otherwise, what are the options?

### Bonus Challenge \[Optional\]

Redesign the developer to suit your company theme / look and feel, by adding the following:

*   Logo
*   Basic style including primary and accent color
*   Background image

In your other window from where you started this lab click **Check my progress** to verify that you've completed the steps to publish your API through a developer portal. API proxy has been published on dev portal.

Part 5 - Measure API Program Success with Apigee Analytics
----------------------------------------------------------

_Persona : API Product Team_

### **Use Case**

#### **What comes after publishing the first API Product**

After you have some API Products up and running you want to have a look at how the program performs and how successful your API program is.

*   How is traffic trending over time?
*   When is the traffic flowing fastest?
*   Where are the most users?
*   What are the most active developers?

#### **Insights help with capacity planning and API program**

With trend usage data you can see how your API calls are trending over time and predict how the traffic will be in the next year, when you need to do your capacity planning

Through visualization of the data with out of the box analytics, Apigee helps you with finding the trends and patterns of your APIs

#### **Tying traffic to API Products**

![apps with metadata](https://cdn.qwiklabs.com/yPWK9KrpAZeDbfWn4Xe6ESdrDRPWjQJXXSloz3yqlbY%3D)

When registering an application to an API product that metadata is loaded when verifying an API key or Oauth Token so you can identify the Developer, App and the Product. This data is crucial for insights into your program.

**Note:** \* If you have followed previous steps you can probably already see some traffic for the proxies you have created earlier and work with your previous API proxy for the custom analytics. \* If you haven't followed previous steps, you can follow this part of the lab but will see less data in your dashboards.

### **Part 5A - Understand API usage using the pre-built Apigee analytics dashboards**

Apigee comes with a lot of pre built reports. For the purpose of these labs and in the interest of time, we will walk through the different dashboards with screenshots from populated demo environments. Feel free to use the optional load generator to follow along in your own environment.

If you prefer you can have a look at the API Dashboards by watching this video that gives you an overview of the metrics dashboards with a lot of filled values: https://www.youtube.com/watch?v=A2KQ3GJvb98&t=88s. Take note that, they might be appeared slightly different in the new console / UI.

To get to the reports open the **API metrics** menu under the **Analytics** in the sidebar.

![part5-analytics-menu](https://cdn.qwiklabs.com/XO23MP6sJCdO5VSNnkuv9H0jx4mfjyTVb42jggmyngY%3D)

#### **1\. API Proxy Performance**

The Proxy Performance dashboard helps you see API proxy traffic patterns and processing times. You can easily visualize how much traffic your APIs generate and how long it takes for API calls to be processed, from the time they are received by Apigee until they are returned to the client app. Under this dashboard, you can furthermore discover Average transaction per seconds (tps), Traffic, Average response time, Traffic by proxy, and Average response time by proxy. Notice that, you can also filter the dashboard by choosing the specific environment, proxy, and the period of the dashboard.

![part5-api-proxy-perf](https://cdn.qwiklabs.com/PkNk8%2FyOMIXQs%2FvsFV5IXWQ44WL6jAG18DMIR7roJp0%3D)

#### **2\. Error Code Analysis**

The Error Code Analysis dashboard tells you about error rates for API proxies and targets. The Error Code Analysis dashboard uses:

*   The response code to calculate proxy errors
*   The target response code to calculate target errors

In this dashboard, you can view dashboards such as Error Composition, Proxy Errors, Target Error, Error By Proxy, Error By Target, Proxy errors by response code, and Target errors by response code.

Still within the same API metrics menu on the left, choose the **ERROR CODE ANALYSIS** tab.

![part5-error-code-analysis](https://cdn.qwiklabs.com/gd8Vm8vrYXpmtYeYgArNwkQJARZN3vScjhuiG8n%2B%2F7A%3D)

#### **3\. Latency Analysis**

The Latency Analysis dashboard can tell you to any latency issues your API proxies may be experiencing. It displays latency measurements down to the window of a minute, highlighting the median, 95th percentile, and 99th percentile values.

It consists of four dashboard namely: Reponse time, Target response time, Request processing latency, and Response processing latency.

The Latency Analysis dashboard can be find right next to the Error Code Analysis tab.

![part5-analysis-analysis](https://cdn.qwiklabs.com/TW3fxgjbRL%2BuFhZyrW1QgsyIs1tEdUDxNJ6z%2BhLPc6s%3D)

#### **4\. Cache Performance**

The Cache Performance dashboard lets you see at a glance the value of your Apigee cache. The dashboard helps you visualize the benefit of the cache in terms of lower latency and reduced load backend servers.

This dashboard measures Cache hit rate, Cache hits, Reponse time.

![part5-cache-performance](https://cdn.qwiklabs.com/kh84CCicLDLE0ff3Xr2QCw5Uohq0ueIsnFaJWa84ZIc%3D)

#### **5\. Target Performance**

The Target Performance dashboard helps you visualize traffic patterns and performance metrics for API proxy backend targets.

![part5-target-performance](https://cdn.qwiklabs.com/VAU7Dxm%2BvsUgb0Rryn6eM%2FbL1PiX5FOXWNzw%2B8NeVgE%3D)

#### **6\. Developer Engagement**

The Developer Engagement dashboard tells you which of your registered app developers are generating the most API traffic. For each of your developers, you can find out who is generating the most API traffic and the most errors. For example, if a particular developer's app is generating a lot of errors relative to other developers, you can pro-actively address the problem with that developer.

Note: The Developer Engagement dashboard is only available in the classic Apigee UI (https://apigee.google.com), Analyze > Developers > Developer Engagement.

![image alt text](https://cdn.qwiklabs.com/vXxHfIx7IAFvOF4TeOt1EqZMhxJXBvpRYue3dH7g3X4%3D)

In the main view, if it is enabled, select the Analyze button under the Actions column for the app to view details about that app and the app developer. The following chart appears:

![image alt text](https://cdn.qwiklabs.com/0BomyjINywhQP19xuctUrORpJS7AmiYj3SSgJ2Cu1Ug%3D)

#### **7\. Traffic Composition**

The Traffic Composition dashboard measures the relative contribution of your top APIs, apps, developers, and products to your overall API program.

Note: The Traffic Composition dashboard is only available in the classic Apigee UI (https://apigee.google.com), Analyze > Developers > Traffic Composition.

![image alt text](https://cdn.qwiklabs.com/pLp1e8jFx17ee38YICC2DK9yKoTX1h4sPMF5qpkeLC0%3D)

#### **8\. Devices**

The Devices dashboard tells you about the devices and servers that are being used to access your APIs. It lets you spot trends in how users are accessing your APIs. For instance, you might notice that traffic from one type of device is increasing, while another is going down, and then decide if the change requires any action or not.

Note: The Device dashboard is only available in the classic Apigee UI (https://apigee.google.com), Analyze > End Users > Devices.

![image alt text](https://cdn.qwiklabs.com/WIFn4tPXn7W4F1aWWHA2cJtAIZ%2FjSqBELd%2FBq9YoEp4%3D)

#### **9\. Geomap**

The Geo Map dashboard tracks traffic patterns, error patterns, and quality of service across geographical locations. You can view information about all your APIs, or zoom in on specific ones.

Note: The Geomap dashboard is only available in the classic Apigee UI (https://apigee.google.com), Analyze > Developers > Geomap.

![image alt text](https://cdn.qwiklabs.com/qn1XOcBPhP3rB64x3eMQF7XDC0tYtivD6gBlencmZVc%3D)

### **Part 5B - Create custom report**

In the event where, the out of the box reports do not meet your requirement, you may create a custom report.

1.  Back to the Apigee menu in GCP Console. Under **Analytics → Custom Reports**, click the "**\+ CREATE**" button and choose **Custom Report**.
    
    ![part5-custom-report-create.png](https://cdn.qwiklabs.com/qC8vgayodfCa9Tr2iUz9mGl9kmLXMkw%2B9Kw%2B8P7KBPk%3D)
2.  Enter the following Custom Report details:
    
    *   Report Name: `API Traffic by API Product and Client Location`
    *   Report Description: `API Traffic by API Product and Client Location`
    *   Select the **Column** Chart type.
    
    Under the Metrics section:
    
    *   First, choose **traffic** with **Aggregated function** of **sum**. Click **Done**.
    *   Click **ADD A METRIC**, then choose **proxy errors** with **Aggregated function** of **sum**. Click **Done**.
    
    Under the Dimensions section:
    
    *   First, choose **API Product**, then click **ADD A DIMENSION**
    *   Then, choose **Country**, then click **ADD A DIMENSION**
    *   Finally, choose **City**
    
    Your custom report should looks like this: ![part5-create-a-custom-report.png](https://cdn.qwiklabs.com/PeG77JJ9sLkdTM2NZqDfvIRzr0Y4oTSkdA9969H5VTU%3D)
    
    Click CREATE.
    
3.  Once created, click the "API Traffic by API Product and Client Location" report to view the report. The custom report should display two column chart diagrams, showing "Sum of traffic" and "Sum of proxy errors". Plus a summary section at the end.
    
    Notice that you can change the Report time by selecting 1 hour, 3 hours, etc. ![part5-view-customreport-default.png](https://cdn.qwiklabs.com/x%2FLtytGQiG2zhtDDKKTxsd%2B6SGnqH7vg7Wgne5hmGrE%3D)
    
4.  \[**Optional**\] Generate several requests to your API from various location. One simple tips is to either use CURL command in the GCP Cloud Shell or Web version of Postman. But remember don't spend too much time on it, time is ticking :)
    
5.  Remember that we have chosen 3 dimensions during the creation. Now, choose any of the **API Product** in the API Product's Dimension **value** section, you should see the **Country** dimension where the API Clients access your API shows up. Choose any one of the country and you should see the **city**.
    
    ![part5-view-customreport-dimension.png](https://cdn.qwiklabs.com/oAS6ocm%2B9Hlg0NkJbD%2FoCO%2BKPCzuNFpRvfGuPS9dX78%3D)
6.  You can also download the data as CSV by clicking the **Export** button to the top right, or change the report time range.
    

### **Part 5 - Summary**

In this lab, you have explored all Reports that show you the success and performance of your API Program and give you valuable insights into how to optimize your APIs.

#### **References**

##### **Apigee Documentation**

*   [Apigee Analytics](https://cloud.google.com/apigee/docs/api-platform/analytics/analytics-services-overview)
*   [Extract Variables Policy](https://cloud.google.com/apigee/docs/api-platform/reference/policies/extract-variables-policy)
*   [Data Capture Policy](https://cloud.google.com/apigee/docs/api-platform/reference/policies/data-capture-policy)
*   [Custom Reports documentation](https://cloud.google.com/apigee/docs/api-platform/analytics/create-custom-reports)

In your other window from where you started this lab click **Check my progress** to verify that you've successfully created a custom analytics report that measures custom metrics. A custom report created.

Congratulations!
----------------

Great! That brings us to the end of lab 1 of Apigee API Jam. Hopefully you've now got a good understanding of API management fundamentals and are ready to tackle more advance aspects of API management.
