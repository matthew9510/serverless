# Identity and Access Management (IAM)
Amazon IAM (Identity and Access Management) enables you to manage users and user
permissions in AWS. You can create one or more IAM users in your AWS account.
You might create an IAM user for someone who needs access to your AWS console,
or when you have a new application that needs to make API calls to AWS. This is
to add an extra layer of security to your AWS account.

We created an IAM user **so that our AWS CLI can operate on our account
without using the AWS Console**.

***AWS Identity and Access Management (IAM) is a web service that helps you
securely control access to AWS resources for your users. You use IAM to control
who can use your AWS resources (authentication) and what resources they can use
and in what ways (authorization).***

**IAM is a service just like all the other services that AWS has, but in some ways
it helps bring them all together in a secure way.**

## What is an IAM User
When you first create an AWS account, you are the root user. The email address
and password you used to create the account is called your root account
credentials. You can use them to sign in to the AWS Management Console. 
When you do, you have complete, unrestricted access to all resources in
your AWS account, including access to your billing information and the ability
to change your password.

**Though it is not a good practice to regularly access your account with this
level of access, it is not a problem when you are the only person who works in
your account.**

**When another person needs to access and manage your AWS
account, you definitely don’t want to give out your root credentials.
Instead you create an IAM user.**

By default, users can’t access anything in your account. **You grant permissions
to a user by creating a policy and attaching the policy to the user.** You can
grant one or more of these policies to restrict what the user can and cannot
access.

## What is an IAM Policy
An IAM policy is a rule or set of rules defining the operations allowed/denied
to be performed on an AWS resource.

Policies can be granted in a number of ways:
- Attaching a managed policy. AWS provides a list of pre-defined policies
such as AmazonS3ReadOnlyAccess.
- Attaching an inline policy. An inline policy is a custom policy created by
hand.
- Adding the user to a group that has appropriate permission policies attached.
- Cloning the permission of an existing IAM user.

A IAM policy has a `Resource` property (ARN). 
An **ARN** is an identifier for a resource in AWS.

We also add the corresponding service actions and condition context keys in
`Action` and `Condition` property.

You can find all the available AWS Service acHons and condition context keys for
use in IAM Policies [here](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_actionsconditons.html)

Aside from attaching a policy to a user, you can attach them to a role or a
group.

## What is an IAM Role
Sometimes your AWS resources need to access other resources in your account. 
While this could be done by creating an IAM user and putting the user’s 
credentials to the Lambda function or embed the credentials in the Lambda code,
this is just **not secure**. If somebody was to get hold of these credentials, they
could make those calls on your behalf. This is where IAM role comes in to play.

**An IAM role is very similar to a user, in that it is an identity with 
permission policies that determine what the identity can and cannot do in AWS.**

***An IAM role is very similar to a user, in that it is an identity with permission
policies that determine what the identity can and cannot do in AWS.*** However, a
role does not have any credentials (password or access keys) associated with it.
Instead of being uniquely associated with one person, a role can be taken on by
anyone who needs it. 

**For example a lambda function can be assigned with a role
to temporarily take on the permission. Roles can be applied to users as well.** 

Roles can be applied to users as well. In this case, the user is taking on the
policy set for the IAM role. This is useful for cases where a user is wearing
multiple “hats” in the organization. ***Roles make this easy since you only need
to create these roles once and they can be re-used for anybody else that wants 
to take it on.***

***You can also have a role tied to the ARN of a user from a different
organization.*** This allows the external user to assume that role as a part
of your AWS organization. This is typically used when you have a third party
service that is acting on your AWS Organization. You’ll be asked to create a
Cross-Account IAM Role and add the external user as a Trust Relationship.
The Trust Relationship is telling AWS that the specified external user can
assume this role.

## What is an IAM Group
An IAM group is simply a collection of IAM users. You can use groups to specify
permissions for a collection of users, which can make those permissions easier
to manage for those users. For example, you could have a group called Admins and
give that group the types of permissions that administrators typically need.
Any user in that group automatically has the permissions that are assigned to
the group. If a new user joins your organization and should have administrator
privileges, you can assign the appropriate permissions by adding the user to
that group. Similarly, if a person changes jobs in your organization, instead of
editing that user’s permissions, you can remove him or her from the old groups
and add him or her to the appropriate new groups.
