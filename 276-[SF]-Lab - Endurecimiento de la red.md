# Network Hardening Using Amazon Inspector and AWS Systems Manager

## Lab Overview

Securing an infrastructure can be a challenge for any company. Companies use many tools to audit networks and find vulnerabilities in systems and applications. This process takes significant time and effort.

In this lab, you are a new security engineer for AnyCompany. You need to identify weak areas in the company's network security and update AnyCompany's environment for better efficiency and optimization. You will use Amazon Inspector to do this.

Amazon Inspector runs scans that analyze all your network configurations—such as security groups, network access control lists (network ACLs), route tables, and internet gateways—together to infer reachability. You don't need to send packets across the virtual private cloud (VPC) network or connect to Amazon Elastic Compute Cloud (Amazon EC2) instance network ports. It’s like packetless network mapping and reconnaissance.

From Amazon Inspector, you will use the network reachability package to analyze your network configurations to find security vulnerabilities in your EC2 instances. The findings that Amazon Inspector generates also provide guidance about restricting access that is not secure.

## Objectives

After completing this lab, you should be able to:

- Configure Amazon Inspector
- Run an agentless network audit
- Investigate the scan results
- Update security groups
- Log in to an application server instance using AWS Systems Manager Session Manager

## Duration

This lab requires approximately 45 minutes to complete.

## Lab Environment

The current environment has two EC2 instances. One instance is a bastion server named BastionServer in a public subnet. The other instance is an application server named AppServer in a private subnet.

Bastion servers are servers used to manage access to an internal or private network from an external network. They are sometimes called jump boxes or jump servers. Because bastion servers are often accessible from the internet, they typically run a minimum number of services to reduce their attack surface. They are also commonly used to proxy and log communications, such as Secure Shell (SSH) sessions.

All backend components, such as Amazon EC2, AWS Identity and Access Management (IAM) roles, and some Amazon Web Services (AWS) services, have been built into your lab already.

## Task 1: View EC2 instances and add tags

To create an assessment target for Amazon Inspector Classic to assess, you start by tagging the EC2 instances that you want to include in your target. In this task, you tag the BastionServer instance.

Every AWS tag consists of a key and value pair of your choice. For example, you might choose to name your key Name and your value MyFirstInstance.

6. In the AWS Management Console, choose **Services** and select **EC2**.

7. If you see **New EC2 Experience** at the upper-left of your screen, confirm that **New EC2 Experience** is selected. This lab is designed to use the new Amazon EC2 console.

8. In the left navigation pane, choose **Instances**.

   The running BastionServer and AppServer EC2 instances are listed.

[![1.png](https://i.postimg.cc/T1HBf3gx/1.png)](https://postimg.cc/2Lh0xm09)

9. Choose the BastionServer instance.

10. Choose the **Tags** tab.

11. Choose **Manage tags**.

[![2.png](https://i.postimg.cc/Kc96nJVW/2.png)](https://postimg.cc/xqzpQy9G)

12. Choose **Add tag**, and then enter the following information:

   - Key: SecurityScan
   - Value: true

13. Choose **Save**.

[![3.png](https://i.postimg.cc/1z9B7ZVr/3.png)](https://postimg.cc/NKzRLS4K)

## Task 2: Configure and run Amazon Inspector

In this task, you will learn how to run an agentless network audit on your EC2 instances using Amazon Inspector. For this lab, you will use the network reachability rules package.

**Use case:** It might not be possible to install agents on all hosts in your deployment. Not all types of operating systems support Amazon Inspector agents. Using this method, you will be able to run a network audit on all hosts.

14. In the AWS Management Console, choose the **Services** menu. Then choose **Security, Identity, & Compliance** and select **Inspector**.

15. To open the navigation pane, click on the left.

16. Choose **Switch to Inspector Classic**.

17. Choose **Get started**.

[![4.png](https://i.postimg.cc/brbFc9yx/4.png)](https://postimg.cc/YLrxQgm0)

18. Choose **Advanced setup**.

19. In the **Define an assessment target** section, configure the following options:

   - For Name, enter **Network-Audit**.
   - Clear the checkbox for **All Instances**.
   - For Tags: Key, choose **SecurityScan**.
   - For Tags: Value, choose **true**.
   - Clear the checkbox for **Install Agents**.
20. Choose **Next**.

[![5.png](https://i.postimg.cc/VsbhCmqJ/5.png)](https://postimg.cc/8sNw88nG)

21. In the **Define an assessment template** section, configure the following options:

   - For Name, enter **Assessment-Template-Network**.
   - For Rules packages, leave **Network Reachability-1.1** selected, but choose the **x** next to each of the other packages to remove them.
   - For Duration, choose **15 Minutes**.
   - Clear the checkbox for **Assessment Schedule**.
22. Choose **Next**.

[![6.png](https://i.postimg.cc/CxJrg1gQ/6.png)](https://postimg.cc/QF9JgjVQ)

23. Choose **Create**.

   You should see a SUCCESS notification, which confirms that the assessment run was initiated. It takes about 3-5 minutes to complete.

[![7.png](https://i.postimg.cc/C5jJ6M7J/7.png)](https://postimg.cc/tn4hsjLV)

   While you wait, you can learn more about Amazon Inspector.

24. Check the status of the scan:

    - In the left navigation pane, choose **Assessment runs**.
    - In the **Amazon Inspector - Assessment Runs** section, choose the **arrow** in the row for the run that you initiated to expand it and access more options for your run.
    - To see the status of the run, choose **Show status**. If you do not see **Show status**, choose **arrow** at the top.
    - To close and return to the previous screen, choose **Close**.

<br>

[![8.png](https://i.postimg.cc/kXsvVppV/8.png)](https://postimg.cc/KRKMwJFF)

25. Once the status changes to **Analysis complete**, choose **Findings** in the left navigation pane.

<br>

[![9.png](https://i.postimg.cc/KztnmqT8/9.png)](https://postimg.cc/xJTkRPgw)

### Summary of Task 2

In this task, you created an assessment target (a collection of the AWS resources that you want Amazon Inspector Classic to analyze). Then you created an assessment template (a blueprint that you use to configure your assessment). You used the template to start an assessment run, which is the monitoring and analysis process that results in a set of findings.

## Task 3: Analyze Amazon Inspector findings

The findings that these rules generate show whether your ports are reachable from the internet through an internet gateway (including instances behind Application Load Balancers or Classic Load Balancers), a VPC peering connection, or a virtual private network (VPN) through a virtual gateway. These findings also highlight network configurations that allow for potentially malicious access, such as mismanaged security groups, ACLs, and internet gateways.

26. Choose the **arrow** to expand the high-severity finding. You should see the following key details:

   - AWS agent ID shows you the affected EC2 instance.
   - Description shows the reason for the finding. In this case, TCP port 23, which is associated with Telnet, is reachable from the internet.
   - Recommendation provides remediation suggestions.

   Telnet is a text-based terminal emulation utility that is part of the TCP/IP suite of protocols. It allows a system to connect to a remote host to perform commands as if you were on the console of the remote machine.

<br>

[![10.png](https://i.postimg.cc/cCmwnvXq/10.png)](https://postimg.cc/T5KK8YzQ)

27. Choose the **arrow** to expand the medium-severity findings and analyze the details.

   For the medium-severity finding, TCP port 22, which is associated with SSH, is reachable from the internet.

   SSH, like the Telnet utility, gives a user the ability to log in to a remote machine and perform commands as if they were on the console of that system. Telnet, however, is insecure because its data isn't encrypted when communicated. SSH provides a secure, encrypted tunnel to access another system remotely.

<br>

[![11.png](https://i.postimg.cc/yxB0xmhX/11.png)](https://postimg.cc/Kkq1sgHR)

## Task 4: Update security groups

In this task, you will see a few remediation options for the security findings that Amazon Inspector discovered. The first option shows how to lock down port 22 to specific IP addresses.

28. Choose the **arrow** to expand the details of the high-severity finding.

29. In the Recommendation section, choose the link to the security group. The link should look similar to the following example: sg-0b2dc685cd6e6e706.

When the link opens, you can see the BastionServerSG security group that is attached to the BastionServer that has produced findings within Amazon Inspector.

[![12.png](https://i.postimg.cc/4NfzT1xX/12.png)](https://postimg.cc/kDHVQKSH)

30. Choose the **Inbound rules**.
These are the current inbound rules for this security group. They are also the high and medium findings that Amazon Inspector caught.

31. Choose **Edit inbound rules**.
   
32. For the inbound rule associated with port range 23, choose **Delete**.
Port 23 Telnet is vulnerable to security attacks, and the SSH protocol helps you to overcome many security issues of Telnet. SSH is now the only major protocol to access the network devices and servers over the internet.
   
33. For the SSH rule, remove the current inbound IP address of 0.0.0.0/0 by choosing the **X** next to it to update the resource.
   The 0.0.0.0/0 IP address for inbound rules means that port 22 is accessible from anyone on the internet.

You can adjust the inbound rules so that only your IP address is able to access port 22. Although this option is much more secure, it still has vulnerabilities. For example, someone could      access the computer that is associated with that IP address and gain access.

35. For Source, choose the **Custom** dropdown list, and then select **My IP**.

36. Choose **Save rules**.

[![13.png](https://i.postimg.cc/HkP0z7Hd/13.png)](https://postimg.cc/v4f6HTcN)

### Re-scan the environment

36. Navigate to the browser tab that has Amazon Inspector open. In the left navigation pane, choose **Assessment templates**.

[![14.png](https://i.postimg.cc/0NTYRRb4/14.png)](https://postimg.cc/HJtcXN00)

37. Select the check box next to **Assessment-Template-Network**, and choose **Run**.

    This step runs the same scan from earlier in the lab and produces findings from the security group updates.
    Note: The scan takes approximately 30-60 seconds to complete.

[![15.png](https://i.postimg.cc/GBRTBRDh/15.png)](https://postimg.cc/GBRTBRDh)

38. In the left navigation pane, choose **Assessment runs**, and refresh every 10-15 seconds until the Status changes to **Analysis complete**.

[![16.png](https://i.postimg.cc/0rjMKczm/16.png)](https://postimg.cc/0rjMKczm)

39. In the left navigation pane, choose **Findings**, and then choose **Date** to sort by most-recent findings.

   The high-severity finding is now gone, but the medium-severity finding remains. Although port 22 was scoped down to allow access to only your IP address, port 22 is still technically open to the internet outside the VPC.

[![17.png](https://i.postimg.cc/KRg1Jjr8/17.png)](https://postimg.cc/KRg1Jjr8)

### Summary of Task 4

In this task, you updated the security group attached to the BastionServer so that it allows traffic from only your IP address instead of the open internet and removed the wide-open and no-longer-needed Telnet port.

## Task 5: Replace BastionServer with Systems Manager

In this task, you replace the BastionServer instance, which has primarily used SSH to connect to the AppServer within the private subnet. Instead, you use Session Manager via Systems Manager.

Systems Manager is a secure end-to-end management solution for hybrid cloud environments. Systems Manager is the operations hub for your AWS applications and resources and consists of four core feature groups.

40. In the AWS Management Console, choose **Services** and select **EC2**.

41. In the left navigation pane, choose **Security Groups**.

42. Choose the Security group ID for BastionServerSG.

43. Choose **Edit inbound rules**.

[![18.png](https://i.postimg.cc/tsQYfb2t/18.png)](https://postimg.cc/tsQYfb2t)

44. Choose **Delete**, and then choose **Save rules** to remove the SSH inbound rule.

[![19.png](https://i.postimg.cc/fVcJQPP4/19.png)](https://postimg.cc/fVcJQPP4)

45. In the left navigation pane, choose **Instances**.

46. Select the check box for BastionServer. Then choose the **Instance state** dropdown list, and choose **Stop instance**.

[![20.png](https://i.postimg.cc/WDstNnRd/20.png)](https://postimg.cc/WDstNnRd)

47. In the confirmation dialog, choose **Stop**.

[![21.png](https://i.postimg.cc/ph6dBMmj/21.png)](https://postimg.cc/ph6dBMmj)

   Next, connect to the AppServer directly using Session Manager.

   With Session Manager, you can quickly and securely access your EC2 instances through an interactive one-click browser-based shell or through the AWS Command Line Interface (AWS CLI) without the need to open inbound ports, maintain bastion hosts, or manage SSH keys.

48. Select the check box next to AppServer, and then choose **Connect**.

    You are now connected directly to the AppServer.

[![22.png](https://i.postimg.cc/zHHGt2hc/22.png)](https://postimg.cc/zHHGt2hc)

[![23.png](https://i.postimg.cc/gXJJR22d/23.png)](https://postimg.cc/gXJJR22d)

50. Enter the following Linux commands to change the directory and to view the current working directory of the AppServer.

    ```
    cd ~
    pwd
    ```

    The output should look like the following: `/home/ssm-user`

[![24.png](https://i.postimg.cc/TyQ1pDGC/24.png)](https://postimg.cc/TyQ1pDGC)

### Final scan of the environment

50. Go to your browser tab that has Amazon Inspector open.

51. In the left navigation pane, choose **Assessment runs**.

52. Select the check box for the previously run assessment, and then choose **Run**.

[![25.png](https://i.postimg.cc/nMXrhrWG/25.png)](https://postimg.cc/nMXrhrWG)

53. Wait for the Status to show **Analysis complete**, and choose the **arrow** to expand the details of the most recent assessment run.

54. Verify that there are zero Findings.

[![26.png](https://i.postimg.cc/hhNjzgWW/26.png)](https://postimg.cc/hhNjzgWW)


## Summary of Task 5

You have successfully improved the network security by adding an IAM role to the AppServer and removing the SSH inbound rule within the Bastion security group while making it even easier to connect using Session Manager provided by Systems Manager.

## Conclusion

Congratulations! You now have successfully:

- Configured Amazon Inspector
- Run an agentless network audit
- Investigated the scan results
- Updated security groups
- Logged in to an application server using Session Manager

