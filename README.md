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
<img width="686" alt="a26" src="https://user-images.githubusercontent.com/115148205/196096137-b490f60e-4c2d-44a0-8dbd-79e58d800d13.PNG">

# Step 2: Grant access to the role
# To modify the Developers user group to allow them to switch to the S3ProductionAccessRole role.

1. Create two users, Navneet and Abhishek, and two user groups, Developers and Testers.
2. Add Navneet to Developers and Abhishek to Testers user groups.
3. Sign in as an administrator in the Development account, and open the IAM console.
4. Choose User groups, and then choose Developers.
5. Choose the Permissions tab, choose Add permissions, and then choose Create inline policy.
6. Choose the JSON tab.
7. Add the following policy statement to allow the AssumeRole action on the S3ProductionAccessRole in the Production account. Be sure that you change [ProdAccountID] in the Resource element to the actual AWS account ID of the Production account.

# Policy from the documentation
<img width="531" alt="a27" src="https://user-images.githubusercontent.com/115148205/196096851-94c8240f-1af5-45ef-8928-175208c52329.PNG">

The Allow effect explicitly allows the Developers group access to the S3ProductionAccessRole role in the Production account. Any developer who tries to access the role succeeds.

8. Choose Review policy.
9. Type a Policy name such as allow-assume-S3-role-in-production.
10. Choose Create policy.
![image](https://user-images.githubusercontent.com/115148205/196098918-e18f6d27-e69d-4855-a96c-c5c89d96cfab.png)

# To modify the testers user group to deny permission to assume the S3ProductionAccessRole role
1. Choose User groups, and then choose Testers.
2. Choose the Permissions tab, choose Add permissions, and then choose Create inline policy.
3. Choose the JSON tab.
4. Add the following policy statement to allow the AssumeRole action on the S3ProductionAccessRole in the Production account. Be sure that you change [ProdAccountID] in the Resource element to the actual AWS account ID of the Production account.

# Policy from the documentation
<img width="536" alt="a29" src="https://user-images.githubusercontent.com/115148205/196099331-e1aa358d-58d7-427b-aed6-070e7cc552d6.PNG">

The Deny effect explicitly denies the Testers group access to the S3ProductionAccessRole role in the Production account. Any tester who tries to access the role receives an access denied message.

5. Choose Review policy.
6. Type a Policy name such as deny-assume-S3-role-in-production.
7. Choose Create policy.

<img width="441" alt="a30" src="https://user-images.githubusercontent.com/115148205/196099664-e91c0a8c-9de4-4ca4-a2f4-4a81a8eceb68.PNG">

# Step 3: Test access by switching roles (Console)
We now have a role that grants access to a resource in the Production account. We also have one user group in the Development account with users allowed to use that role. This step discusses how to test switching to that role from the AWS Management Console.

Switching roles using the AWS Management Console only works with accounts that do not require an ExternalId. For example, assume that you grant access to your account to a third party and require an ExternalId in a Condition element in your permissions policy. In that case, the third party can access your account only by using the AWS API or a command line tool. The third party cannot use the console because it cannot supply a value for ExternalId. More informaton at How to use an external ID when granting access to your AWS resources to a third party,

# To assume a role
1. Logon to AWS console as Navneet (Developer) in the Development user group.
2. Select Navneet on top right corner (the Identity menu) on the navigation bar, and then choose Switch Role.
3. Next, in order to access the role, Navneet must type the ProdAccountID (999999999999), the role name (S3ProductionAccessRole), Display name as S3-Production and then chooses Switch Role.
4. Navneet can now use the Amazon S3 console to work with the Amazon S3 bucket, or any other resource to which the S3ProductionAccessRole has permissions.
5. When done, Navneet can return to his original permissions. To do that, he chooses S3-Production role display name on the navigation bar and then chooses Switch Back.
6. The next time that Navneet wants to switch roles and chooses the Identity menu in the navigation bar, he sees the S3-Production entry still there from last time. He can simply choose that entry to switch roles immediately without reentering the account ID and role name.
7. Repeat the above steps with Abhishek (Tester) and he will not be able to assume the role S3ProductionAccessRole,

<img width="330" alt="a32" src="https://user-images.githubusercontent.com/115148205/196108733-8980c4a0-7f18-48e1-9cf3-712b233d220c.PNG">
<img width="661" alt="a33" src="https://user-images.githubusercontent.com/115148205/196108882-21dcbc2d-80aa-4bca-9fa3-d00545eb8ee0.PNG">

<img width="663" alt="a34" src="https://user-images.githubusercontent.com/115148205/196109059-b6582e6b-7e70-4f7e-a356-db1b4889f953.PNG">
<img width="268" alt="a35" src="https://user-images.githubusercontent.com/115148205/196109611-d088f535-4ca0-417f-a681-d7cc50afb923.PNG">

# Revoke an assumed role
1. Navigate to S3ProductionAccessRole under IAM > Roles in the Production Account.
2. Click on Revoke Sessions > Click on Revoke active sessions > Select I acknowledge that I am revoking all active session for this role > Click Revoke active sessions.
3. This will add an inline policy with a policy name as AWSRevokeOlderSessions with a condition of DateLessThan the aws:TokenIssueTime. All existing sessions will be revoked.

<img width="644" alt="a36" src="https://user-images.githubusercontent.com/115148205/196110028-3d418785-84d3-4659-bcc3-4223970bbdb7.PNG">
<img width="566" alt="a37" src="https://user-images.githubusercontent.com/115148205/196110223-bed4c4cc-cf69-4816-8c6a-385649b3e863.PNG">
<img width="661" alt="a38" src="https://user-images.githubusercontent.com/115148205/196110410-3d0aff59-e35c-47f5-9b84-7fd8bd6b5e3e.PNG">
<img width="660" alt="a39" src="https://user-images.githubusercontent.com/115148205/196110545-945702c9-1172-45b8-b60e-7bd5bfed3f90.PNG">

# Clean Up
1. Empty the bucket.
2. Delete the bucket.
3. Delete S3ProductionAccessPolicy and in the Production Account.
4. Delete Navneet and Abhishek users.
5. Delete Developers and Testers User groups.

# Summary

1. In this demo we have learned how to delegate access across AWS accounts using IAM roles.
2. You don't have to create individual IAM users in each account.
3. Users don't have to sign out of one account and sign into another in order to access resources in different AWS accounts.
4. A user in one account can switch to a role in the same or a different account. While using the role, the user can perform only the actions and access only the resources permitted by the role; their original user permissions are suspended. When the user exits the role, the original user permissions are restored.












    
