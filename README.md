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
To get started we first create the bucket that will need to be accessed from a different account.  We will refer to the account that has the bucket as account-2, and the user who needs access to the bucket will be in account-1.  Here is a demonstration of creating the bucket in account-2 <br/>
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
<img src="https://i.imgur.com/10zRjXD.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Keep the defaults and click "Create bucket" to create the bucket.  <br/>
<br />
<img src="https://i.imgur.com/0BscBsC.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 - <b>Copy the arn of the new bucket as we will need to use it to grant access to this bucket</b>
<br />
 <br />
<img src="https://i.imgur.com/TPZ2lH3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Now that the bucket is created, we need to navigate to account-1.  In account-1 we need to create a user, assign permission policies to the user, and give the user access keys to use for programmatic access.  To do this, we first create the policy that will allow the user to access the bucket we just created, and use the bucket arn we just copied in the policy.  To get started, we navigate to account-1 and begin creating the policy.  <br/>
 <br />
<img src="https://i.imgur.com/Uj0KHKc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 The permissions we are adding in this policy are for S3:  <br/>
 <br />
<img src="https://i.imgur.com/Yx49haG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 Click in the "JSON" tab to edit the policy in JSON directly, as opposed to using the visual editor, and paste the arn of the bucket from account-2 into the JSON permission policy. <br />  Keep in mind that this policy is created in account-1, while the bucket in its arn can be found in account-2:  <br/>
<br />
<br />
<br />
 <img src="https://i.imgur.com/lpV0mP1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 <br />
<br />
Review and create the policy :  <br/>
 <br />
<img src="https://i.imgur.com/ybM5tk7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 After the policy is created, we need to attach the policy to a user.  Still in account-1, we now proceed to create the user.  <br/>
  <br/>
<img src="https://i.imgur.com/VODpvSf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
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
<img src="https://i.imgur.com/FzVTdm2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Click "Create User"
<img src="https://i.imgur.com/se1q5er.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
After the user is created, we need to assign access keys to give the user programmatic acccess  <br/>
 <br/>
<img src="https://i.imgur.com/PSsoksc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Create the access keys  <br/>
<br />
<img src="https://i.imgur.com/FVW0vmW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<img src="https://i.imgur.com/CkG3HbM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
  <br/>
<img src="https://i.imgur.com/hhDlY8x.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Retrieve the access keys  <br/>
<br/>
<img src="https://i.imgur.com/u51I4mZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
 
<img src="https://i.imgur.com/vgWEwCe.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br/>
The user in account-1 now has permission to access the bucket in account-2.  We now need the bucket in account-2 to also have the correct permissions to be accesssed.  For cross-account access, both accounts must have policies that permit access.  Simply giving the user permission is not sufficient.  We need to edit the bucket policy as well. Navigate back to the bucket in account-2 and edit the bucket policy.
<img src="https://i.imgur.com/vAyqQgR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
Edit the bucket policy.  Use the JSON editor option and paste the arn of the user we just created in account-1  <br/>
<img src="https://i.imgur.com/WLJ3ok6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
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
- <b> Role based cross-account access </b> <br/>
  <br/>
  1
<img src="https://i.imgur.com/5RYtZ06.png" height="80%" width="80%"/>
<br />
<br />
<br />
 2 Description  <br/>
<br />
<img src="https://i.imgur.com/8vH8Waq.png" height="80%" width="80%"/>
<br />
<br />
<br />
  3 Description  <br/>
<br />
<img src="https://i.imgur.com/KkdVXpC.png" height="80%" width="80%"/>

  <br/>
  4
<img src="https://i.imgur.com/N9xtgdz.png" height="80%" width="80%"/>
<br />
<br />
<br />
  5 Description  <br/>
<br />
<img src="https://i.imgur.com/xZNAKbU.png" height="80%" width="80%"/>
<br />
<br />
<br />
  6 Description  <br/>
<br />
<img src="https://i.imgur.com/YIkhmfV.png" height="80%" width="80%"/>
  <br/>
  <br />
<br />
7
<br />
<img src="https://i.imgur.com/2gCjIJO.png" height="80%" width="80%"/>
<br />
<br />
<br />
  8 Description  <br/>
<br />
<img src="https://i.imgur.com/dOQVsM2.png" height="80%" width="80%"/>
<br />
<br />
<br />
 9 Description  <br/>
<br />
<img src="https://i.imgur.com/MLAA0qC.png" height="80%" width="80%"/>
 <br/>
 <br />
<br />
  10 Description
<img src="https://i.imgur.com/wSQXpHt.png" height="80%" width="80%"/>
<br />
<br />
<br />
  11 Description  <br/>
<br />
<img src="https://i.imgur.com/TBAnI4Y.png" height="80%" width="80%"/>
<br />
<br />
<br />
 12 Description  <br/>
<br />
<img src="https://i.imgur.com/RgEsdB7.png" height="80%" width="80%"/>
<br />
<br />
  <br/>
 13 Description  <br/>
<br />
<img src="https://i.imgur.com/YxDGET6.png" height="80%" width="80%"/>
<br />
<br />
<br />
  14 Description  <br/>
<br />
<img src="https://i.imgur.com/NtpJzSh.png" height="80%" width="80%"/>
<br />
<br />
<br />
 15 Description  <br/>
 <br />
 <img src="https://i.imgur.com/AUVbVrK.png" height="80%" width="80%"/>
  <br />
<br />
<br />
 16 Description  <br/>
<br />
<img src="https://i.imgur.com/8l6TDYb.png" height="80%" width="80%"/>
  <br/>
  17
<img src="https://i.imgur.com/GCgP0zl.png" height="80%" width="80%"/>
<br />
<br />
<br />
 18 Description  <br/>
<br />
<img src="https://i.imgur.com/HW4TEaN.png" height="80%" width="80%"/>
<br />
<br />
<br />
  19 skipped goes to 20 Description  <br/>
<br />
<img src="https://i.imgur.com/hJy68Lq.png" height="80%" width="80%"/>
 <br/>
 21
<img src="https://i.imgur.com/b6fPWGG.png" height="80%" width="80%"/>
<br />
<br />
<br />
  22 Description  <br/>
<br />
<img src="https://i.imgur.com/Ekhwbcx.png" height="80%" width="80%"/>
<br />
<br />
<br />
  23 skipped goes to 26 Description  <br/>
<br />
<img src="https://i.imgur.com/KNdyYra.png" height="80%" width="80%"/>

  <br/>
  27 skipped goes to 29 
<img src="https://i.imgur.com/MwluJRZ.png" height="80%" width="80%"/>
<br />
<br />
<br />
 30 skipped goes to 31 Description  <br/>
<br />
<img src="https://i.imgur.com/w6SrRDc.png" height="80%" width="80%"/>
<br />
<br />
<br />
 32 skipped goes to 33 Description  <br/>
<br />
<img src="https://i.imgur.com/5RW7i2V.png" height="80%" width="80%"/>
  <br/>
  <br />
<br />
<br />
  goes to 39 from 33 Description  <br/>
<br />
<img src="https://i.imgur.com/H1rJ7T8.png" height="80%" width="80%"/>
<br />
<br />
<br />
 40 Description  <br/>
<br />
<img src="https://i.imgur.com/JSn2htv.png" height="80%" width="80%"/>
<br />
<br />
<br />
 41 Description  <br/>
<br />
<img src="https://i.imgur.com/MldgFH6.png" height="80%" width="80%"/>
 <br />
<br />
<br />
 42 Description  <br/>
<br />
<img src="https://i.imgur.com/ImyYFoF.png" height="80%" width="80%"/>
<br />
<br />
<br />
 43 Description  <br/>
<br />
<img src="https://i.imgur.com/vg6Sek3.png" height="80%" width="80%"/>
<br />
<br />
<br />
 45 since 44 is skipped Description  <br/>
<br />
<img src="https://i.imgur.com/xNsDO5P.png" height="80%" width="80%"/>
<br />
<br />
<br />
  46 Description  <br/>
<br />
<img src="https://i.imgur.com/9OsWmsi.png" height="80%" width="80%"/>
<br />
<br />
<br />
 48 since 47 skipped Description  <br/>
<br />
<img src="https://i.imgur.com/QM2HW90.png" height="80%" width="80%"/>
<br />
<br />
<br />
 49 Description  <br/>
<br />
<img src="https://i.imgur.com/7RW6uCX.png" height="80%" width="80%"/>
 <br />
<br />
<br />
 50 Description  <br/>
<br />
<img src="https://i.imgur.com/OjSMtM1.png" height="80%" width="80%"/>
<br />
<br />
<br />
 51 Description  <br/>
<br />
<img src="https://i.imgur.com/W69TEs2.png" height="80%" width="80%"/>
<br />
<br />
<br />
 52 Description  <br/>
<br />
<img src="https://i.imgur.com/bDlEEeQ.png" height="80%" width="80%"/>
<br />
<br />
<br />
 53 Description  <br/>
<br />
<img src="https://i.imgur.com/Y8l7wD4.png" height="80%" width="80%"/>
<br />
<br />
<br />
 54 Description  <br/>
<br />
<img src="https://i.imgur.com/ewboM8f.png" height="80%" width="80%"/>
<br />
<br />
<br />
  55 Description  <br/>
<br />
<img src="https://i.imgur.com/FqubGpW.png" height="80%" width="80%"/>
<br />
<br />
<br />
  56 Description  <br/>
<br />
<img src="https://i.imgur.com/ps1kisx.png" height="80%" width="80%"/>
<br />
<br />
<br />
 57  Description  <br/>
<br />
<img src="https://i.imgur.com/bZHXeNS.png" height="80%" width="80%"/>
<br />
<br />
<br />
