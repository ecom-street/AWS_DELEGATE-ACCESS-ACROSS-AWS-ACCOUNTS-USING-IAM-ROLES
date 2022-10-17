# AWS_DELEGATE-ACCESS-ACROSS-AWS-ACCOUNTS-USING-IAM-ROLES
In this blog we are going to go through the steps to delegate access to resources in two different AWS accounts called Production and Development.
Developers should be able to access an S3 bucket in the Production account, however the testers from the same account should not be able to access the S3 bucket in the Production account.
By setting up cross-account access this way:
You don't have to create individual IAM users in each account.
Users don't have to sign out of one account and sign into another in order to access resources in different AWS account.
A user in one account can switch to a role in the same or a different account. While using the role, the user can perform only the actions and access only the resources permitted by the role; their original user permissions are suspended. When the user exits the role, the original user permissions are restored.

<img width="662" alt="iam1" src="https://user-images.githubusercontent.com/115148205/196085038-8fd70e63-5614-4ed7-adff-6a12d9376919.PNG">
