<h1>AWS Cross Account Access -- Draft In Progress</h1>

### Description of role based and programmatic cross-account access

<h2>Introduction</h2>
Description of CLI access using key assigned to user identity 
<br />
Description using role-based access and advantge of temporry credentials
<h2>Description </h2>

In this demo, we will explore two ways of configuring cross-acount access.  First by assigning permissions to a user directly through IAM and bucket policy, then by using roles. 
<br />
<br />
<h2>Web Services Used </h2>

- <b>Amazon Identity Access Management </b>
- <b>Amazon S3</b>
- <b>Amazon CLI</b>
- <b>Amazon CLoudshell</b>

We will use Amazon Web Services (AWS) to illustrate cross-account access using API keys assigned to a user directly, as well as through assuming role in the same account as the resource.
<h1>Program walk-through:</h1>
<br />
<p align="left">
<h2>Part One: </h2> <br /> 
First we create a resource in one account and a user in another account.  We then configure the resource (bucket) policy in such a way as to allow users from a different account to access it. 
<br />
- <b>Create bucket and user</b>
<br />
<br /> 
To get started, sign in to your AWS account and navigate to the AWS console to select the icon for API Gateway: <br/>
<br /> 
<img src="https://i.imgur.com/bwCkwEK.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Once in API Gateway, select HTTP API and click "Build":  <br/>
<br/>
<img src="https://i.imgur.com/SEcR5wD.png" height="80%" width="80%"/>
<br />
<br />
<br />
Name the API Gateway and click to "add integration": <br/>
<br/>
<img src="https://i.imgur.com/8FAVCiX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Select Lambda integration for the API. This is where we add the backend service that the API will communicate with.  <br/>
<br />
<img src="https://i.imgur.com/S1U2ewf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
For a Lambda integration, API Gateway invokes the Lambda function and responds with the response from the Lambda function. The Lambda function needs to be created before we can integrate it with the API Gateway :  <br/>
<br />
<img src="https://i.imgur.com/crWckjz.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 - <b>Create Lambda Function to Integrate with API Gateway</b>
<br />
<br />
<br />
 Navigate back to AWS Console and chose Lambda to create the Lambda function that will be integrated with API Gateway:  <br/>
 <br />
<img src="https://i.imgur.com/nNXjfEL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Click "Create Function":  <br/>
 <br />
<img src="https://i.imgur.com/0Ev5MFu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 Enter a name for the function and keep the defaults for "author from scratch":  <br/>
 <br />
<img src="https://i.imgur.com/PnraodU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 By default, Lambda will create an execution role with permissions to upload logs. Click "create function":  <br/>
  <br/>
<img src="https://i.imgur.com/eB0bGgh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
The function is now created. We need to scroll down to see the code section and edit the code :  <br/>
 <br />
<img src="https://i.imgur.com/JERldJk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 Edit the response message in the code of the Lambda function to say "Hello from Lambda: Secure me!" and click "deploy":  <br/>
  <br/>
<img src="https://i.imgur.com/KlTFfmI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
 - <b>Integrate Lambda Function with API Gateway</b>
 <br />
<br />
 Navigate back to API Gateway tab and select the newly created Lambda from the drop down menu to integrate it:  <br/>
 <br/>
<img src="https://i.imgur.com/WnhCqwI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 Click "next" to continue creating the API Gateway. In this example, the Lambda function we just created "my-api-lambda" , will now be integrated with the API Gateway we just created named "my-api":  <br/>
 <br/>
<img src="https://i.imgur.com/nfZv5qV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 The next step is to configure routes, the method is set to ANY by default, but we will change it to GET:  <br/>
<img src="https://i.imgur.com/t7ulGfi.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />

<img src="https://i.imgur.com/HW8QPWS.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Keep the rest of the defualts, click "next":  <br/>
 <br/>
<img src="https://i.imgur.com/KyhvfQ6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Keep default stage and click "next":  <br/>
<br />
<img src="https://i.imgur.com/YlZK7G5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Review and click "create" to create API Gateway with Lambda integration:  <br/>
 <br/>
<img src="https://i.imgur.com/MR4yTI1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 - <b>Demonstrate That The API Gateway Is Currently Open to the Internet</b>
<br />
<br />
API Gateway is now inegrated with the Lambda function.  Knowing the "Invoke URL" of the API Gateway and the name of the Lambda function is sufficient to access the resource  :  <br/>
<img src="https://i.imgur.com/YTE32Nm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Under the "Lambda" tab, we can copy the name of the function and add it to the end of the API "invoke url" path :  <br/>
<br/>
<img src="https://i.imgur.com/3Qb6UA1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
In the URL bar, type the API "invoke URL" /  name of the lambda function to specify the path. Accessing the "Hello from Lambda: Secure me!" message proves that this path is openly accessible:  <br/>
<br/>
<img src="https://i.imgur.com/dh9vJsX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
We can also demonstrate open access to the resource by using Postman.com, which is an API platform for building, testing, and using APIs.  We can use Postman.com to add authorization headers to our API calls once we secure our API Gateway.  For now, since we havent't configured a Cognito user pool to serve as our identity provider, and haven't attached an authorizer to the API Gateway, we don't have any tokens to append to our message, nor are they required for access.  Therefore, the same URL that we typed in the browser URL bar to get access to our "Hello from Lambda: Secure me!" message in the screen shot above, can also be used in Postman.com to prove that no credentials are necessary to access our Lambda function. Type in the same URL for our resource into Postman.com (or copy-paste it from your browser's URL) and click send:  <br/>
<br/>
<img src="https://i.imgur.com/hFRIupW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
The response from Postman is "200 OK" and the "Hello from Lambda: Secure me!" message is returned, demonstrating that my-api-lambda resource is currently freely accessible from the public internet:  <br/>
<img src="https://i.imgur.com/H9w3Gbp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 - <b>Begin Configuring Authorizer to Secure API Gateway</b>
 <br />
<br />
 
To begin configuring an authorizer for the API Gateway, go to API Gateway tab and click on routes:  <br/>
<img src="https://i.imgur.com/uk1P2ZM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 Select "GET" under the my-api-lambda path to see that there are currently no authorizers attached to this path:  <br/>
  <br/>
<img src="https://i.imgur.com/Clrpm39.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
There are currently no authorizers to select from.  <br/>
<br />
<img src="https://i.imgur.com/9rNZE3D.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
- <b>Configure Cognito User Pool and App Client</b> <br/>
<br />
In order to configure a JWT authorizer to protect this resource, we will first create an identity provider in the form of a Cognito User Pool to issue tokens, and an app client that will request these tokens and return them in a callback URL. Then we will create a test user with which to sign in to the app client to request the tokens that we will then use to access the API Gateway Lambda function:
<br />
<br />
<br />
Navigate back to console and open Cognito in new tab:  <br/>
<br />
<img src="https://i.imgur.com/fSZK1xF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Start with Cognito by creating the user pool.  The user pool serves as the authorization server and issuer of JWTs:  <br/>
<br />
<img src="https://i.imgur.com/LITx6py.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Configure sign in experience through Cognito.  Check user name and email for sign in options :  <br/>
<br />
<img src="https://i.imgur.com/mtijisZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Click "next" :  <br/>
<br />
<img src="https://i.imgur.com/yvnf9Q8.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Keep Cognito defaults for password requirements:  <br/>
<br />
<img src="https://i.imgur.com/f6qT417.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Select "No MFA", enable "self-service account recovery email" only for messages, and click next:  <br/>
<br />
<img src="https://i.imgur.com/ONiGy4p.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Keep defaults for self-service sign-up:  <br/>
<br />
<img src="https://i.imgur.com/bVXtk2B.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Keep defaults, click "next" at the bottom to continue configuring user pool:  <br/>
<br />
<img src="https://i.imgur.com/ouc9so6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Select "send email with Cognito," keep the rest of the default selections and click "next":  <br/>
<br />
<img src="https://i.imgur.com/2DBUElo.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Name the user pool and select "Use Cognito Hosted UI".  The Cognito Hosted UI will provide the interface to log in with our test user.  Once authenticated, the test user will be able to request JSON Web Tokens   <br/>
<br />
<img src="https://i.imgur.com/OXYOYkB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Use Cognito domain name:  <br/>
<br />
<img src="https://i.imgur.com/CSY1mUy.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Select "Public Client" name the app client.  Since we are using OAuth 2.0 implicit grant, select "don't generate client secret":  <br/>
<img src="https://i.imgur.com/yaNqcWH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Add localhost:3000 as the callback URL.  This URL is where the test user will be redirected after authenticating.  The JSON Web Tokens will also be sent to the URL bar after the test user successfully authenticaes to the Cognito User Pool through the Cognito Hosted UI. Optionally, you can also add the callback URL for postman.com to use the platform to test access to the API:  <br/>
  <br/>
<img src="https://i.imgur.com/VV9EiKl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Keep Cognito user pool as the identity provider and select OAuth 2.0 grant type to be "implicit grant" for this demo so we can collect the JWT that will be sent to the callback URL:  <br/>
<img src="https://i.imgur.com/H4b4ygI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Select OIDC scopes and click next.  Selecting OIDC scopes delegates permission to the user of this token to access the Open Id Connect profile of the user.  We will be able to see these scopes once we decode a valid JWT that will be issued by this Cognito user pool:  <br/>
  <br/>
<img src="https://i.imgur.com/KEoQph5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Review settings on next page, scroll to the bottom, and click "create user pool":  <br/>
<img src="https://i.imgur.com/yZF4q8P.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 The Cognito user pool named "Cognito-user-pool" in this example is now created, and we can click on it to create a test user we will use to log in  <br/>
<img src="https://i.imgur.com/wwZ9i7X.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
- <b>Create Test User in Cognito User Pool</b> <br/>
<br />
<br />
<br />
  After clicking on "cognito-user-pool", under the "users tab", click  "create user":  <br/>
<img src="https://i.imgur.com/FfsnPD6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Create user and configure sign-in details:  <br/>
<img src="https://i.imgur.com/sqq8Biu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
  Set the temporary password and click "create user":  <br/>
<img src="https://i.imgur.com/VAhtKGf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 Our test user "testuser" is now created, and we can navigate to "app integration" :  <br/>
  <br/>
<img src="https://i.imgur.com/GVDj2pw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
  From "users" tab navigate to "app integration" tab to find the app client. Scroll down to find app client name and click on it:  <br/>
   <br/>
<img src="https://i.imgur.com/bKalepO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<img src="https://i.imgur.com/YNNalmJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 Scroll down to find "View Hosted UI":  <br/>
<img src="https://i.imgur.com/44ynmlY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Click View Hosted UI" to login as "testuser":  <br/>
 <br/>
<img src="https://i.imgur.com/mKdyfW2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 The hosted UI uses the domain we specified earlier as the app client domain, and prompts us to log in :  <br/>
  <br/>
<img src="https://i.imgur.com/se8jBVD.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
  The first time we log in, we are required to change the password :  <br/>
   <br/>
<img src="https://i.imgur.com/BLsrko1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
- <b>Retrieving JWT from URL and Decoding Using jwt.io</b> <br/>
<br />
<br />
<br />
 After successfully logging in we will be redirected to localhost:3000 and sent JWT credentails in the URL bar, which we will copy and separate into relevant tokens:  <br/>
  <br/>
<img src="https://i.imgur.com/ZrWaXtE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
  These are the JWT Credentials returned in the URL and copy-pasted into pages.  The two JWT tokens are the access token and the Id token. These tokens are base64 encoded but they are not encrypted.  They can be decoded into plain-text JSON that is easily readable:  <br/>
   <br/>
<img src="https://i.imgur.com/f0CgKNf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
   Base64 encoded JSON token before pasting into jwt decoder at jwt.io:  <br/>
<img src="https://i.imgur.com/Wr7dU7Z.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
  Paste token into jwt.io decoder to view the header, body, and signature:  <br/>
<img src="https://i.imgur.com/F0dDBJA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />

 <br/>
<img src="https://i.imgur.com/IyCWh3W.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />

<img src="https://i.imgur.com/QbyURoX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
  <br/>
<img src="https://i.imgur.com/trhDE4R.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 - <b>JWT and Implicit Grant Analysis <b/>
 <br />
<br />
<br />
