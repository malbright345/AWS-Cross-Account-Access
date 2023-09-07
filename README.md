<h1>AWS Cross-Account Access </h1>



<h2>Introduction</h2>
Cross-account access in AWS can be achieved using roles with AWS Identity and Access Management (IAM) or by assigning permissions directly via bucket policies and individual user policies. 
<br />
<br />
Using IAM roles for cross-account access is generally considered a more secure and manageable approach, especially for scenarios involving multiple AWS accounts and fine-grained control over permissions.
Assigning permissions directly via bucket policies and individual user policies can be simpler but may pose security and management challenges, particularly as complexity increases.
The choice between these approaches depends on your specific use case, security requirements, and the complexity of your AWS environment. In many cases, it's advisable to use IAM roles for cross-account access to maintain a higher level of security and control.
<br />
<br />
<h2>Description </h2>
<br />
<br />
In this demo, we will explore two ways of configuring cross-acount access.  First by assigning permissions to a user directly through IAM and bucket policy, then by using roles. 
<br />
<br />
<h2>Web Services Used </h2>

- <b>AWS IAM </b>
- <b>AWS S3</b>
- <b>AWS CLI</b>
- <b>AWS Cloud Shell</b>
<br />
<h1>Program walk-through:</h1>
<br />
<br />
<p align="left">
<h2>Part One: </h2> <br /> 

To get started we first create the bucket that will need to be accessed from a different account.  We will refer to the account that has the bucket as account-2, and the user who needs access to the bucket will be in account-1.
<br />
  <br />
To increase readability, account-2 will be marked in red, while account-1 will be marked in magenta.
 <br />
  <br />
- <b>Create bucket and user</b>
<br />
<br /> 
 Here is a demonstration of creating the bucket in account-2 <br/>
<br /> 
<img src="https://i.imgur.com/bwCkwEK.png" height="80%" width="80%"/>
<br />
<br />
<br />
Give the bucket in account-2 a name.  The name of an S3 bucket must be gloablly unique:  <br/>
<br/>
<img src="https://i.imgur.com/SEcR5wD.png" height="80%" width="80%"/>
<br />
<br />
<br />
We can keep the defaults when creating the bucket, keeping the default selections for "ACLs disbaled" and "block all public access." <br/>
<br/>
<img src="https://i.imgur.com/VYj7zAm.png" height="80%" width="80%"/>
<br />
<br />
<br />
Keep the default settings  <br/>
<br />
<img src="https://i.imgur.com/10zRjXD.png" height="80%" width="80%" />
<br />
<br />
Keep the defaults and click "Create bucket" to create the bucket.  <br/>
<br />
<img src="https://i.imgur.com/0BscBsC.png" height="80%" width="80%" />
<br />
<br />
<br />
 - <b>Copy the arn of the new bucket as we will need to use it to grant access to this bucket</b>
<br />
 <br />
<img src="https://i.imgur.com/TPZ2lH3.png" height="80%" width="80%" />
<br />
<br />
<br />
Now that the bucket is created, we need to navigate to account-1.  In account-1 we need to create a user, assign permission policies to the user, and give the user access keys to use for programmatic access.  To do this, we first create the policy that will allow the user to access the bucket we just created, and use the bucket arn we just copied in the policy.  To get started, we navigate to account-1 and begin creating the policy.  <br/>
 <br />
<img src="https://i.imgur.com/Uj0KHKc.png" height="80%" width="80%" />
<br />
<br />
<br />
 The permissions we are adding in this policy are for S3:  <br/>
 <br />
<img src="https://i.imgur.com/Yx49haG.png" height="80%" width="80%" />
<br />
<br />
<br />
 Click in the "JSON" tab to edit the policy in JSON directly, as opposed to using the visual editor, and paste the arn of the bucket from account-2 into the JSON permission policy. <br />  Keep in mind that this policy is created in account-1, while the bucket in its arn can be found in account-2:  <br/>
<br />
<br />
<br />
 <img src="https://i.imgur.com/lpV0mP1.png" height="80%" width="80%" />
 <br />
<br />
Review and create the policy :  <br/>
 <br />
<img src="https://i.imgur.com/ybM5tk7.png" height="80%" width="80%" />
<br />
<br />
<br />
 After the policy is created, we need to attach the policy to a user.  Still in account-1, we now proceed to create the user.  <br/>
  <br/>
<img src="https://i.imgur.com/VODpvSf.png" height="80%" width="80%" />
<br />
<br />
<br />
 We will be attaching the policy directly to the user  <br/>
 <br/>
<img src="https://i.imgur.com/GIHh4tJ.png" height="80%" width="80%"/>
<br />
<br />
<br />
 Attach the policy and continue creating the user <br/>
 <br/>
<img src="https://i.imgur.com/iL1hCZr.png" height="80%" width="80%"/>
<br />
<br />
<br />
 Review and create  <br/>
<img src="https://i.imgur.com/FzVTdm2.png" height="80%" width="80%" />
<br />
<br />
<br />
Click "Create User"
<img src="https://i.imgur.com/se1q5er.png" height="80%" width="80%" />
<br />
<br />
<br />
After the user is created, we need to assign access keys to give the user programmatic acccess  <br/>
 <br/>
<img src="https://i.imgur.com/PSsoksc.png" height="80%" width="80%""/>
<br />
<br />
<br />
Create the access keys  <br/>
<br />
<img src="https://i.imgur.com/FVW0vmW.png" height="80%" width="80%" />
<br />
<br />
<br />
<img src="https://i.imgur.com/CkG3HbM.png" height="80%" width="80%" />
<br />
<br />
  <br/>
<img src="https://i.imgur.com/hhDlY8x.png" height="80%" width="80%" />
<br />
<br />
<br />
Retrieve the access keys  <br/>
<br/>
<img src="https://i.imgur.com/u51I4mZ.png" height="80%" width="80%" />
<br />
<br />
<br />
 
<img src="https://i.imgur.com/vgWEwCe.png" height="80%" width="80%"/>
<br />
<br />
<br />
<br/>
The user in account-1 now has permission to access the bucket in account-2.  We now need the bucket in account-2 to also have the correct permissions to be accesssed.  For cross-account access, both accounts must have policies that permit access.  Simply giving the user permission is not sufficient.  We need to edit the bucket policy as well. Navigate back to the bucket in account-2 and edit the bucket policy.
<img src="https://i.imgur.com/vAyqQgR.png" height="80%" width="80%" />
<br />
<br />
<br />
Edit the bucket policy.  Use the JSON editor option and paste the arn of the user we just created in account-1  <br/>
<img src="https://i.imgur.com/WLJ3ok6.png" height="80%" width="80%" />
<br />
<br />
<br />
 - <b> Once the user has permission to access the bucket and the bucket policy allows the user to access it, we can list the contents of the bucket in account-2 from the CLI as the user from account-1 and cross-account access is acheived</b>
 <br />
<br />
 <br/>
<img src="https://i.imgur.com/QrIOLAl.png" height="80%" width="80%"/>
<br />
<br />
<br />
 <H2> Role-Based Cross-Account Access  </H2/>
  <br/>
  <br/>
  Navigate to IAM in account-2 and create a role
  <br/>
<img src="https://i.imgur.com/5RYtZ06.png" height="80%" width="80%"/>
<br />
<br />
<br />
 Select the trusted entity for role in account-2 as "another aws account"   <br/>
<br />
<img src="https://i.imgur.com/8vH8Waq.png" height="80%" width="80%"/>
<br />
<br />
<br />
  Look up the account id number for the account to be trusted (account-1) so we can reference it   <br/>
<br />
<img src="https://i.imgur.com/KkdVXpC.png" height="80%" width="80%"/>
 <br/>
  <br/>
   <br/>
  Paste the account id from account-1 into the role in account-2 that will trust account-1
   <br/>
<img src="https://i.imgur.com/N9xtgdz.png" height="80%" width="80%"/>
<br />
<br />
<br />
  Put in the account number to be trusted and click next   <br/>
<br />
<img src="https://i.imgur.com/xZNAKbU.png" height="80%" width="80%"/>
<br />
<br />
<br />
  Add permission to the role in account-2   <br/>
<br />
<img src="https://i.imgur.com/YIkhmfV.png" height="80%" width="80%"/>
  <br/>
  <br />
<br />
 Select JSON view when specifying permissions
<br />
<img src="https://i.imgur.com/2gCjIJO.png" height="80%" width="80%"/>
<br />
<br />
<br />
  Click on "JSON" to edit the policy <br/>
<br />
<img src="https://i.imgur.com/dOQVsM2.png" height="80%" width="80%"/>
<br />
<br />
<br />
 Use the bucket for cross account access in account-2 and give permission to a role in account-2 to access this bucket <br/>
<br />
<img src="https://i.imgur.com/MLAA0qC.png" height="80%" width="80%"/>
 <br/>
 <br />
<br />
  Follow the principle of least privilege to assign granular permissions. 
  <br />
<img src="https://i.imgur.com/wSQXpHt.png" height="80%" width="80%"/>
<br />
<br />
<br />
  Name the policy and fill in the description for the role in account-2  <br/>
<br />
<img src="https://i.imgur.com/TBAnI4Y.png" height="80%" width="80%"/>
<br />
<br />
<br />
Review permissions defined in the policy   <br/>
<br />
<img src="https://i.imgur.com/RgEsdB7.png" height="80%" width="80%"/>
<br />
<br />
  <br/>
Click create policy <br/>
<br />
<img src="https://i.imgur.com/YxDGET6.png" height="80%" width="80%"/>
<br />
<br />
<br />
 Check the JSON representation of the policy to see the details   <br/>
<br />
<img src="https://i.imgur.com/NtpJzSh.png" height="80%" width="80%"/>
<br />
<br />
<br />
  Select the policy to attach it to the role, and scroll down to click next <br/>
 <br />
 <img src="https://i.imgur.com/AUVbVrK.png" height="80%" width="80%"/>
  <br />
<br />
<br />
 Click next to continue creating the role  <br/>
<br />
<img src="https://i.imgur.com/8l6TDYb.png" height="80%" width="80%"/>
  <br/>
   <br/>
    <br/>
   Name the role and add a description
   <br/>
<img src="https://i.imgur.com/GCgP0zl.png" height="80%" width="80%"/>
<br />
<br />
<br />
 The JSON of the STS AssumeRole with the account-1 as the "principal"  <br/>
<br />
<img src="https://i.imgur.com/HW4TEaN.png" height="80%" width="80%"/>
<br />
<br />
<br />
  Click create role in account-2  <br/>
<br />
<img src="https://i.imgur.com/hJy68Lq.png" height="80%" width="80%"/>
 <br/>
 <br/>
 <br/>
 The role is now created, we need to click on it for details
 <br/>
<img src="https://i.imgur.com/b6fPWGG.png" height="80%" width="80%"/>
<br />
<br />
<br />
  Details after clicking on the role now that the role is created  <br/>
<br />
<img src="https://i.imgur.com/Ekhwbcx.png" height="80%" width="80%"/>
<br />
<br />
<br />
   Change the trust relationship for the role from "root," which in this case means anyone in account-1. Instead, use granular permissions and the principle of least privilege to limit the scope to the specific user in account-1 <br/>
<br />
<img src="https://i.imgur.com/KNdyYra.png" height="80%" width="80%"/>

  <br/>
  Edit trust policy of the role in account-2 that will be assumed by account-1  <br/>
<img src="https://i.imgur.com/MwluJRZ.png" height="80%" width="80%"/>
<br />
<br />
<br />
 Click the update policy button  <br/>
<br />
<img src="https://i.imgur.com/w6SrRDc.png" height="80%" width="80%"/>
<br />
<br />
<br />
 Trust relationship now limited in scope  <br/>
<br />
<img src="https://i.imgur.com/5RW7i2V.png" height="80%" width="80%"/>
  <br/>
  <br />
<br />
<br />
  Account-1 list of current users to add the ability to call the Secure Token Service (STS) AssumeRole <br/>
<br />
<img src="https://i.imgur.com/H1rJ7T8.png" height="80%" width="80%"/>
<br />
<br />
<br />
Create an inline policy to add permission to call Secure Token Service (STS) to allow the user to assume a role.  Whithout the ability to call the STS:AssumeRole API, a user does not have permission to assume any roles <br/>
<br />
<img src="https://i.imgur.com/JSn2htv.png" height="80%" width="80%"/>
<br />
<br />
<br />
 Create the policy   <br/>
<br />
<img src="https://i.imgur.com/MldgFH6.png" height="80%" width="80%"/>
 <br />
<br />
<br />
 To adhere to the principle of least privilege, this user should only be allowed to call a specific role  <br/>
<br />
<img src="https://i.imgur.com/ImyYFoF.png" height="80%" width="80%"/>
<br />
<br />
<br />
 Copy the ARN of the role to be assumed by this user  <br/>
<br />
<img src="https://i.imgur.com/vg6Sek3.png" height="80%" width="80%"/>
<br />
<br />
<br />
Specify the ARN   <br/>
<br />
<img src="https://i.imgur.com/xNsDO5P.png" height="80%" width="80%"/>
<br />
<br />
<br />
  Restrict the STS:AssumeRole permission to specific role <br/>
<br />
<img src="https://i.imgur.com/9OsWmsi.png" height="80%" width="80%"/>
<br />
<br />
<br />
 View the policy in JSON to make sure it's correct   <br/>
<br />
<img src="https://i.imgur.com/QM2HW90.png" height="80%" width="80%"/>
<br />
<br />
<br />
 Click create to create the policy  <br/>
<br />
<img src="https://i.imgur.com/7RW6uCX.png" height="80%" width="80%"/>
 <br />
<br />
<br />
 Scroll down to make sure the policy was attached as expected  <br/>
<br />
<img src="https://i.imgur.com/OjSMtM1.png" height="80%" width="80%"/>
<br />
<br />
<br />
 The user in account-1 now has the permission to assume the role in account-2  <br/>
<br />
<img src="https://i.imgur.com/W69TEs2.png" height="80%" width="80%"/>
<br />
<br />
<br />
 Review the JSON policy  <br/>
<br />
<img src="https://i.imgur.com/bDlEEeQ.png" height="80%" width="80%"/>
<br />
<br />
<br />
 Navigate to AWS Cloud Shell in account-1 to test "test-user-in-account-1" access to bucket in account-2 <br/>
<br />
<img src="https://i.imgur.com/Y8l7wD4.png" height="80%" width="80%"/>
<br />
<br />
<br />
 Use the command "get caller identity" to ensure we are now working as the "test-user-in-account-1" in Cloud Shell  <br/>
<br />
<img src="https://i.imgur.com/ewboM8f.png" height="80%" width="80%"/>
<br />
<br />
<br />
  Now try to assume the role. Name the session to get the token needed to assume the role  <br/>
<br />
<img src="https://i.imgur.com/FqubGpW.png" height="80%" width="80%"/>
<br />
<br />
<br />
 Try to call the aws S3 ls command for the bucket in account-2  <br/>
<br />
<img src="https://i.imgur.com/ps1kisx.png" height="80%" width="80%"/>
<br />
<br />
<br />
 Since we can list the items in the bucket in account-2 from the Cloud Shell of the user in account-1, we prove that we have successfully assumed the role in account-2 and achieved cross-account access using roles.  <br/>
<br />
<img src="https://i.imgur.com/bZHXeNS.png" height="80%" width="80%"/>
<br />
<br />
<br />
