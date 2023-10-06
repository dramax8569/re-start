# Introduction to AWS Identity and Access Management (IAM)

In many business environments, access involves a single login to a computer or a network of computer systems that provides the user access to all resources on the network. This access includes rights to personal and shared folders on a network server, company intranets, printers, and other network resources and devices. Unauthorized users can quickly exploit these same resources if the access control and associated authentication procedures are not set up properly.

In this lab, you will explore users, user groups, and policies in the AWS Identity and Access Management (IAM) service.

## Objectives

After completing this lab, you should be able to:

- Create and apply an IAM password policy
- Explore pre-created IAM users and user groups
- Inspect IAM policies as applied to the pre-created user groups
- Add users to user groups with specific capabilities active
- Locate and use the IAM sign-in URL
- Experiment with the effects of policies on service access

Here is diagram of the current environment with the listed IAM users and IAM groups.

## Other AWS services

During this lab, you might receive error messages when performing actions beyond the steps in this lab. These messages will not impact your ability to complete the lab.

## IAM

IAM can be used for the following:

- Manage IAM users and their access: You can create users and assign them individual security credentials (access keys, passwords, and multi-factor authentication devices). You can manage permissions to control which operations a user can perform.
- Manage IAM roles and their permissions: An IAM role is similar to a user in that a role is an AWS identity with permission policies that determine what the identity can and cannot do in Amazon Web Services (AWS). However, instead of being uniquely associated with one person, a role is intended to be assumable by anyone who needs it.
- Manage federated users and their permissions: You can activate identity federation to allow existing users in your enterprise to access the AWS Management Console, to call AWS application programming interfaces (APIs), and to access resources without the need to create an IAM user for each identity.

## Duration

This lab requires approximately 60 minutes to complete.


## Task 1: Create an Account Password Policy

In this task, you will create a custom password policy for your AWS account, which will affect all users associated with the account.

1. Start by noting the Region you are currently in (e.g., Oregon). You can find the Region displayed in the upper-right corner of the console page.
----------------------2
2. Go to the AWS Management Console and enter "IAM" in the search box, then select "IAM" from the results.

3. In the left navigation pane, choose "Account settings."

4. Here, you can view the default password policy currently in effect. However, your company has stricter requirements, so you need to update this policy.

5. Choose "Change password policy."
----------------------3
6. Under "Select your account password policy requirements," configure the following options:
   - For "Enforce minimum password length," change it from 8 to 10 characters.
   - Select every checkbox except for "Password expiration requires administrator reset."
   - For "Enable password expiration," keep the default option of 90 days.
   - For "Prevent password reuse," keep the default option of 5 passwords.
----------------------4
7. Choose "Save changes."

These changes will affect the AWS account level and apply to every user associated with the account.
----------------------5
**Summary of Task 1:**
In this task, you enhanced the password requirements by creating a custom password policy. The selected password options will make it more challenging for users to create easily crackable passwords.


## Task 2: Explore Users and User Groups

In this task, you will explore the pre-created users and user groups in IAM.

12. In the left navigation pane, select "Users." You will find the following IAM users already created for you:
   - user-1
   - user-2
   - user-3

13. Choose "user-1." This takes you to the Summary page for user-1, and the Permissions tab is displayed. Note that user-1 currently has no permissions.

14. Go to the "Groups" tab. You'll notice that user-1 is not a member of any user groups. User groups consist of multiple users who need access to the same resources. By assigning privileges to the group, you can efficiently manage permissions for multiple users.

15. Switch to the "Security credentials" tab. Here, you can see that user-1 has been assigned a Console password.
----------------------6
16. In the left navigation pane, select "User groups." You will find the following user groups already created for you:
    - EC2-Admin
    - EC2-Support
    - S3-Support

17. Choose the "EC2-Support" group. This will take you to the Summary page for the EC2-Support group.

18. Navigate to the "Permissions" tab. The EC2-Support group has a managed policy called "AmazonEC2ReadOnlyAccess" associated with it. Managed policies are pre-defined policies that can be attached to IAM users and user groups. When a policy is updated, the changes apply immediately to all users and groups with that policy attached.

19. Click on the plus sign next to the "AmazonEC2ReadOnlyAccess" policy to view its details. IAM policies consist of statements that specify allowed or denied actions for AWS resources. This particular policy grants permissions to list and describe information related to Amazon Elastic Compute Cloud (EC2), Elastic Load Balancing (ELB), Amazon CloudWatch, and Amazon EC2 Auto Scaling. These permissions allow users to view resources without making modifications.
----------------------7
20. Go back to the left navigation pane and select "User groups" again.

21. Now, choose the "S3-Support" group.

22. Access the "Permissions" tab for this group. The "S3-Support" group has the "AmazonS3ReadOnlyAccess" policy attached.

23. Click on the plus sign next to the "AmazonS3ReadOnlyAccess" policy to reveal its details. This policy grants permissions to get and list resources within Amazon S3.
----------------------8
24. Return to the left navigation pane and select "User groups" once more.

25. Choose the "EC2-Admin" group.

26. Go to the "Permissions" tab. Unlike the other two groups, the "EC2-Admin" group has a Customer inline policy, which is a policy assigned specifically to this group or user. Inline policies are typically used for unique or one-off situations.

27. Click on the plus sign next to the "EC2-Admin-Policy" policy to view its details. This policy grants permissions to view (Describe) information about Amazon EC2 and also provides the ability to start and stop EC2 instances.

----------------------9

## Summary of Task 2

In this task, you explored pre-created users and user groups within IAM. You also examined the policies attached to these user groups and gained insights into the differences in permissions among the user groups.

28. Under Actions, select the "Show Policy" link. IAM policies define which actions are permitted or denied for specific AWS resources. The policy you viewed granted permissions for listing and describing information about Amazon Elastic Compute Cloud (EC2), Elastic Load Balancing (ELB), Amazon CloudWatch, and Amazon EC2 Auto Scaling. This read-only access is suitable for support roles. The basic structure of IAM policy statements includes:
   - Effect: Specifies Allow or Deny permissions.
   - Action: Defines the allowed API calls against AWS services (e.g., cloudwatch:ListMetrics).
   - Resource: Determines the scope of entities covered by the policy rule (e.g., a specific Amazon S3 bucket, EC2 instance, or * for any resource).

29. To close the "Show Policy" window, click the close button (X).

30. In the left navigation pane, select "User groups."

31. Choose the "S3-Support" group, which has the "AmazonS3ReadOnlyAccess" policy attached.

32. From the Actions menu, select the "Show Policy" link. This policy grants permissions to retrieve and list resources in Amazon S3.

33. To close the "Show Policy" window, click the close button (X).

34. In the left navigation pane, select "User groups."

35. Choose the "EC2-Admin" group, which differs from the other two groups. Instead of a managed policy, it has a custom inline policy assigned specifically to this group. Inline policies are typically used for applying permissions in unique or one-off situations.

36. Under Actions, choose "Show Policy" to view the policy. This policy grants permission to view (Describe) information about Amazon EC2 and provides the ability to start and stop EC2 instances.

37. At the bottom of the screen, select "Cancel" to close the policy.

## Summary of Task 2

In this task, you had the opportunity to examine pre-created users and user groups, explore their associated policies, and understand the distinctions in permissions between these user groups.

## Business Scenario

For the remainder of this lab, you will be working with a set of users and user groups to enable permissions that support the following business scenario:

Your company is experiencing substantial growth in its use of AWS, utilizing numerous EC2 instances and a significant amount of Amazon S3 storage. To efficiently manage access for new staff members based on their job functions, you have the following requirements:

1. **User:** user-1
   - **In Group:** S3-Support
   - **Permissions:** Read-only access to Amazon S3

2. **User:** user-2
   - **In Group:** EC2-Support
   - **Permissions:** Read-only access to Amazon EC2

3. **User:** user-3
   - **In Group:** EC2-Admin
   - **Permissions:** View, start, and stop EC2 instances

This setup will allow you to tailor permissions to each user's specific responsibilities within your AWS environment.
