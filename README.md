# AWS_DELEGATE-ACCESS-ACROSS-AWS-ACCOUNTS-USING-IAM-ROLES
In this blog we are going to go through the steps to delegate access to resources in two different AWS accounts called Production and Development.
Developers should be able to access an S3 bucket in the Production account, however the testers from the same account should not be able to access the S3 bucket in the Production account.
By setting up cross-account access this way:
You don't have to create individual IAM users in each account.
Users don't have to sign out of one account and sign into another in order to access resources in different AWS account.
A user in one account can switch to a role in the same or a different account. While using the role, the user can perform only the actions and access only the resources permitted by the role; their original user permissions are suspended. When the user exits the role, the original user permissions are restored.

<img width="662" alt="iam1" src="https://user-images.githubusercontent.com/115148205/196085038-8fd70e63-5614-4ed7-adff-6a12d9376919.PNG">

# Prerequisites
Two different AWS accounts, one for Production and the other for Development.
Users and users groups in the Development account.

<img width="149" alt="iam2" src="https://user-images.githubusercontent.com/115148205/196086686-3778baee-65ac-44d5-8c8b-094fd1ee3943.PNG">
We will be using the following substitutions in the following bucket policies:

<img width="250" alt="a21" src="https://user-images.githubusercontent.com/115148205/196088302-0f753683-d176-400f-99b6-2933f58f5198.PNG">

# Step 1: Create a role in the Production Account
Before we can create a role, we need the account ID of the Development AWS account. Each AWS account has a unique account ID identifier assigned to it.
1. Obtain the Development AWS account ID by signing in to the console then click on the account name and copy the 12-digit account number (ID).
2. Sign in to the AWS Management Console as an administrator of the Production account, and open the IAM console.
3. We are going to create a custom policy by navigating to Policies and then choose Create policy.
4. Choose the JSON tab and copy the text from the following JSON policy document. Paste this text into the JSON text box, replacing the resource ARN (arn:aws:s3:::[BucketName]) with the real one for your Amazon S3 bucket.
# Policy from the documentation

 <img width="583" alt="s3" src="https://user-images.githubusercontent.com/115148205/196091219-1210fc4b-0339-42aa-9255-3c57668569e1.PNG">
  
The ListBucket permission allows users to view objects in the productionapp bucket. The GetObject, PutObject, DeleteObject permissions allows users to view, update, and delete contents in the productionapp bucket.

5. On the Review page, enter S3ProductionAccessPolicy for the policy name. Review the policy Summary to see the permissions granted by your policy, and then choose Create policy.
6. In the navigation pane, choose Roles and then choose Create role.
7. Choose the An AWS account role type.
8. For Account ID, type the [DevAccountID], which we copied earlier at the start of this section.
9. Choose Next: Permissions to set the permissions associated with the role.
10. Select the check box next to the policy named S3ProductionAccessPolicy and then, click Next.
11. Click on Create role and name it as S3ProductionAccessRole.
12. Copy the ARN of S3ProductionAccessRole role by navigating to IAM > Roles > Search for S3ProductionAccessRole > Click on it > Copy the ARN.

At this point, we have established trust between the Production and Development accounts. We did this by creating a role in the Production account that identifies the Development account as a trusted principal. We also defined what the users who switch to the S3ProductionAccessRole can do.

<img width="371" alt="a22" src="https://user-images.githubusercontent.com/115148205/196093171-a284c2a8-f514-4599-83dd-2eccbcc59fc9.PNG">

# Step 1.3
<img width="662" alt="s23" src="https://user-images.githubusercontent.com/115148205/196093351-7f3ddad0-4fd9-43c0-b699-a5d87bcdc099.PNG">

<img width="662" alt="a23" src="https://user-images.githubusercontent.com/115148205/196093482-69f92b4a-3b8b-4b63-9aaa-41a801b9701b.PNG">

# Step 1.6
<img width="663" alt="a24" src="https://user-images.githubusercontent.com/115148205/196093597-bc485504-8852-427c-9494-a27ea69e984f.PNG">

# Step 1.10
<img width="662" alt="a25" src="https://user-images.githubusercontent.com/115148205/196093760-020f3c8b-308f-4d7d-9fda-8c623496c236.PNG">

# Step 1.11
<img width="686" alt="a26" src="https://user-images.githubusercontent.com/115148205/196094174-2590e66f-c0fc-41ac-853e-670e2a644474.PNG">





    
