# Introduction to AWS Identity and Access Management (IAM) !!

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

[![1.jpg](https://i.postimg.cc/xTLfMStB/1.jpg)](https://postimg.cc/56NWWDcm)

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

[![2.png](https://i.postimg.cc/523fKSc3/2.png)](https://postimg.cc/WddRhr1F)

2. Go to the AWS Management Console and enter "IAM" in the search box, then select "IAM" from the results.

3. In the left navigation pane, choose "Account settings."

4. Here, you can view the default password policy currently in effect. However, your company has stricter requirements, so you need to update this policy.

5. Choose "Change password policy."

[![3.png](https://i.postimg.cc/Y0YH61fQ/3.png)](https://postimg.cc/vxYkMxKm)

6. Under "Select your account password policy requirements," configure the following options:
   - For "Enforce minimum password length," change it from 8 to 10 characters.
   - Select every checkbox except for "Password expiration requires administrator reset."
   - For "Enable password expiration," keep the default option of 90 days.
   - For "Prevent password reuse," keep the default option of 5 passwords.

[![4.png](https://i.postimg.cc/3x7KZbqN/4.png)](https://postimg.cc/jLg0xvtY)

7. Choose "Save changes."

These changes will affect the AWS account level and apply to every user associated with the account.

[![5.png](https://i.postimg.cc/BZK4f09t/5.png)](https://postimg.cc/PNTB10qk)

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

[![6.png](https://i.postimg.cc/85qGfyW1/6.png)](https://postimg.cc/34CVPFn6)

16. In the left navigation pane, select "User groups." You will find the following user groups already created for you:
    - EC2-Admin
    - EC2-Support
    - S3-Support

17. Choose the "EC2-Support" group. This will take you to the Summary page for the EC2-Support group.

18. Navigate to the "Permissions" tab. The EC2-Support group has a managed policy called "AmazonEC2ReadOnlyAccess" associated with it. Managed policies are pre-defined policies that can be attached to IAM users and user groups. When a policy is updated, the changes apply immediately to all users and groups with that policy attached.

19. Click on the plus sign next to the "AmazonEC2ReadOnlyAccess" policy to view its details. IAM policies consist of statements that specify allowed or denied actions for AWS resources. This particular policy grants permissions to list and describe information related to Amazon Elastic Compute Cloud (EC2), Elastic Load Balancing (ELB), Amazon CloudWatch, and Amazon EC2 Auto Scaling. These permissions allow users to view resources without making modifications.

[![7.png](https://i.postimg.cc/wvtHvK1f/7.png)](https://postimg.cc/r0L7nPCx)

20. Go back to the left navigation pane and select "User groups" again.

21. Now, choose the "S3-Support" group.

22. Access the "Permissions" tab for this group. The "S3-Support" group has the "AmazonS3ReadOnlyAccess" policy attached.

23. Click on the plus sign next to the "AmazonS3ReadOnlyAccess" policy to reveal its details. This policy grants permissions to get and list resources within Amazon S3.

[![8.png](https://i.postimg.cc/PqVTTf5v/8.png)](https://postimg.cc/nsm61J7n)

24. Return to the left navigation pane and select "User groups" once more.

25. Choose the "EC2-Admin" group.

26. Go to the "Permissions" tab. Unlike the other two groups, the "EC2-Admin" group has a Customer inline policy, which is a policy assigned specifically to this group or user. Inline policies are typically used for unique or one-off situations.

27. Click on the plus sign next to the "EC2-Admin-Policy" policy to view its details. This policy grants permissions to view (Describe) information about Amazon EC2 and also provides the ability to start and stop EC2 instances.

[![9.png](https://i.postimg.cc/25SrsLrf/9.png)](https://postimg.cc/zy6sWByd)

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

| User    | In Group      | Permissions                               |
| ------- | ------------- | ----------------------------------------- |
| user-1  | S3-Support    | Read-only access to Amazon S3             |
| user-2  | EC2-Support   | Read-only access to Amazon EC2            |
| user-3  | EC2-Admin     | View, start, and stop EC2 instances       |

This setup will allow you to tailor permissions to each user's specific responsibilities within your AWS environment.

## Task 3: Add users to user groups

You have recently hired user-1 into a role where they will provide support for Amazon S3. You add them to the S3-Support group so that they inherit the necessary permissions via the attached AmazonS3ReadOnlyAccess policy.

You can ignore any not authorized errors that appear during this task. They are caused by your lab account having limited permissions and should not impact your ability to complete the lab.

### Add user-1 to the S3-Support group

38. In the left navigation pane, choose User groups.
39. Choose the S3-Support group.
40. Choose the Users tab.
41. In the Users tab, choose Add users.
 
[![10.png](https://i.postimg.cc/FsXm9BNp/10.png)](https://postimg.cc/BtpRpN8P)
 
42. In the Add users to S3-Support window, configure the following options:
   - Select the check box for user-1.
   - Choose Add Users.

[![11.png](https://i.postimg.cc/65VKTfQX/11.png)](https://postimg.cc/rKp3PWWn)

43. In the Users tab, you see that user-1 has been added to the group.

[![12.png](https://i.postimg.cc/qvkTK6tC/12.png)](https://postimg.cc/JGdvVn51)

### Add user-2 to the EC2-Support group

You have hired user-2 into a role where they provide support for Amazon EC2.

43. Using the previous steps in this task, add user-2 to the EC2-Support group.
   - user-2 should now be part of the EC2-Support group.

[![13.png](https://i.postimg.cc/kgfdvM8Y/13.png)](https://postimg.cc/bZDMzhm1)

### Add user-3 to the EC2-Admin group

You have hired user-3 as your Amazon EC2 administrator to manage your EC2 instances.

44. Using the previous steps in this task, add user-3 to the EC2-Admin group.
   - user-3 should now be part of the EC2-Admin group.

[![14.png](https://i.postimg.cc/26BRY4tg/14.png)](https://postimg.cc/KkFsrgx5)

45. In the left navigation pane, choose User groups.
   - Each group should have a 1 in the Users column for the number of users in each group.
   - If there is not a 1 beside each group, revisit the previous instructions in this task to confirm that each user is assigned to a group as shown in the table at the beginning of the Business scenario section.

[![15.png](https://i.postimg.cc/FK65wPTj/15.png)](https://postimg.cc/d7Gxy9P1)

### Summary of task 3

In this task, you added all the associated users to the user groups.

## Task 4: Sign in and test user permissions

In this task, you test the permissions of each IAM user.

46. In the left navigation pane, choose Dashboard.

   The AWS Account section includes a Sign-in URL for IAM users in this account. This link should look similar to the following: [Sign-in URL](https://123456789012.signin.aws.amazon.com/console). You can use this link to sign in to the AWS account that you are currently using.

47. Copy the Sign-in URL for IAM users in this account to a text editor.

48. Open a private window using the following instructions for your web browser:

   - **Mozilla Firefox**
     - Choose the menu bars at the upper-right of the screen.
     - Choose New Private Window.
   
   -  **Google Chrome**
       - Choose the ellipsis at the upper-right of the screen.
       - Choose New Incognito window.
   
   - **Microsoft Edge**
       - Choose the ellipsis at the upper-right of the screen.
       - Choose New InPrivate window.
   
   - **Microsoft Internet Explorer**
       - Choose the Tools menu option.
       - Choose InPrivate Browsing.

49. Paste the Sign-in URL for IAM users in this account into your private window, and press Enter.

   You now sign in as user-1, who has been hired as your Amazon S3 storage support staff.

50. Sign in using the following credentials:

   - IAM user name: Enter user-1
   - Password: Enter Lab-Password1

51. Choose Sign in.

   If you see a dialog prompting you to switch to the new console home, choose Switch to the new Console Home.

[![16.png](https://i.postimg.cc/hvWFtXWv/16.png)](https://postimg.cc/5Y3KPNtW)

52. From the Services menu, choose S3.

53. Choose the name of one of your buckets, and browse the contents.

   Because your user is part of the S3-Support group in IAM, they have permission to view a list of S3 buckets and their contents.

[![17.png](https://i.postimg.cc/gc8FBH8Z/17.png)](https://postimg.cc/Jtr2sJTr)

   Now, test whether they have access to Amazon EC2.

54. From the Services menu, choose EC2.

55. In the left navigation pane, choose Instances.

   You cannot see any instances. Instead, you see a message that says, You are not authorized to perform this operation. This message appears because your user has not been assigned any permissions to use Amazon EC2.

[![18.png](https://i.postimg.cc/D0q9J7ng/18.png)](https://postimg.cc/dZVHgP7k)

   You now sign in as user-2, who has been hired as your Amazon EC2 support person.

56. Sign user-1 out of the AWS Management Console by following these steps:

   - At the top of the screen, choose user-1.
   - Choose Sign out.

[![19.png](https://i.postimg.cc/SKRbT0fS/19.png)](https://postimg.cc/75FRhWRd)

57. Paste the Sign-in URL for IAM users in this account into your private window, and press Enter. This link should be in your text editor.

58. Sign in using the following credentials:
   - IAM user name: Enter user-2
   - Password: Enter Lab-Password2

59. Choose Sign in.

[![20.png](https://i.postimg.cc/fTv4MWHW/20.png)](https://postimg.cc/dLDNBFPg)

   If you see a dialog prompting you to switch to the new console home, choose Switch to the new Console Home.

60. From the Services menu, choose EC2.

61. In the left navigation pane, choose Instances.

   You are now able to see an EC2 instance because you have read-only permissions. However, you are not able to make any changes to Amazon EC2 resources.

[![21.png](https://i.postimg.cc/4x4DyCM6/21.png)](https://postimg.cc/VdTGZhzv)

   If you cannot see an EC2 instance, then your Region may be incorrect. In the upper-right of the screen, choose the Region menu, and select the Region that you noted at the start of the lab (for example, Oregon).

[![22.png](https://i.postimg.cc/9F253wYN/22.png)](https://postimg.cc/z3xPKBMn)

Your EC2 instance should be selected. If it is not, choose it.

62. From the Instance state dropdown list, choose Stop instance.

63. In the Stop instance? window, choose Stop.


Your EC2 instance should be selected. If it is not, choose it.

62. From the Instance state dropdown list, choose Stop instance.

63. In the Stop instance? window, choose Stop.

[![23.png](https://i.postimg.cc/jjLB6HRC/23.png)](https://postimg.cc/v1wPb6gd)

You receive an error that says, Failed to stop the instance. You are not authorized to perform this operation. This message demonstrates that the policy gives you permission to only view information and does not give you permission to make changes.

64. At the Stop Instances window, choose Cancel.

Next, check if user-2 can access Amazon S3.

65. From the Services menu, choose S3.

You receive an You don't have permissions to list buckets message because user-2 does not have permission to use Amazon S3.

[![24.png](https://i.postimg.cc/KYww01Yf/24.png)](https://postimg.cc/v170BHfx)

You now sign in as user-3, who has been hired as your Amazon EC2 administrator.

66. Sign user-2 out of the AWS Management Console by following these steps:
    * At the top of the screen, choose user-2.
    * Choose Sign out.

[![25.png](https://i.postimg.cc/13WLNChL/25.png)](https://postimg.cc/PCw6gMq4)

67. Paste the Sign-in URL for IAM users in this account into your private window, and press Enter. If this link is not in your clipboard, retrieve it from the text editor where you pasted it earlier.

68. Sign in using the following credentials:
    * IAM user name: Enter user-3
    * Password: Enter Lab-Password3

69. Choose Sign in. If you see a dialog prompting you to switch to the new console home, choose Switch to the new Console Home.

[![26.png](https://i.postimg.cc/DZGDH7JT/26.png)](https://postimg.cc/ftwB0nf2)

70. From the Services menu, choose EC2.

71. In the left navigation pane, choose Instances. As an EC2 administrator, you should now have permissions to stop the EC2 instance. Your EC2 instance should be selected. If it is not, choose it. If you cannot see an EC2 instance, then your Region may be incorrect. In the upper-right of the screen, choose the Region menu, and select the Region that you noted at the start of the lab (for example, Oregon).

72. From the Instance state dropdown list, choose Stop instance.

73. In the Stop instance? window, choose Stop. The instance should enter the Stopping state and will shut down.

74. Close your private window.

[![27.png](https://i.postimg.cc/Z5tkS7zm/27.png)](https://postimg.cc/svcHPJ2H)

Summary of task 4
In this task, you were able to sign in as all three users. You verified that user-1 was able to view S3 buckets but unable to view EC2 instances. You then signed in as user-2 and verified that they were able to view EC2 instances but unable to perform the stop instance action. user-2 was also unable to view S3 buckets. After signing in as user-3, you were able to view EC2 instances and perform the stop instance action.

## Conclusion

Congratulations! You have successfully completed this lab and achieved the following objectives:

- Created and applied an IAM password policy
- Explored pre-created IAM users and user groups
- Inspected IAM policies as applied to the pre-created user groups
- Added users to user groups with specific capabilities active
- Located and used the IAM sign-in URL
- Experimented with the effects of policies on service access

Well done on gaining practical experience with AWS Identity and Access Management (IAM). This knowledge will be valuable as you continue to work with AWS services and manage user access and permissions.
