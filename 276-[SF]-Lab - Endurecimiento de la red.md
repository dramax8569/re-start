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

## Accessing the AWS Management Console

- At the upper-right corner of these instructions, choose **Start Lab**.

(Troubleshooting tip: If you get an Access Denied error, close the error box, and choose **Start Lab** again.)

The following information indicates the lab status:

- A red circle next to AWS at the upper-left corner of this page indicates that the lab has not been started.
- A yellow circle next to AWS at the upper-left corner of this page indicates that the lab is starting.
- A green circle next to AWS at the upper-left corner of this page indicates that the lab is ready.

Wait for the lab to be ready before proceeding.

- At the top of these instructions, choose the green circle next to AWS.

This option opens the AWS Management Console in a new browser tab. The system automatically signs you in.

(Tip: If a new browser tab does not open, a banner or icon at the top of your browser might indicate that your browser is preventing the site from opening pop-up windows. Choose the banner or icon, and choose **Allow pop-ups**.)

If you see a dialog prompting you to switch to the new Console Home, click **Switch to the new Console Home**.

Arrange the AWS Management Console tab so that it displays alongside these instructions. Ideally, you should be able to see both browser tabs at the same time so that you can follow the lab steps.

**Do not change the lab Region unless specifically instructed to do so.**

## Accessing the AWS Management Console

1. At the upper-right corner of these instructions, choose **Start Lab**.

   Troubleshooting tip: If you get an Access Denied error, close the error box, and choose **Start Lab** again.

   The following information indicates the lab status:

   - A red circle next to AWS at the upper-left corner of this page indicates that the lab has not been started.
   - A yellow circle next to AWS at the upper-left corner of this page indicates that the lab is starting.
   - A green circle next to AWS at the upper-left corner of this page indicates that the lab is ready.
   
   Wait for the lab to be ready before proceeding.

2. At the top of these instructions, choose the green circle next to AWS.

   This option opens the AWS Management Console in a new browser tab. The system automatically signs you in.

   Tip: If a new browser tab does not open, a banner or icon at the top of your browser might indicate that your browser is preventing the site from opening pop-up windows. Choose the banner or icon, and choose **Allow pop-ups**.

   If you see a dialog prompting you to switch to the new Console Home, click **Switch to the new Console Home**.

3. Arrange the AWS Management Console tab so that it displays alongside these instructions. Ideally, you should be able to see both browser tabs at the same time so that you can follow the lab steps.

   **Do not change the lab Region unless specifically instructed to do so.**

## Task 1: View EC2 instances and add tags

To create an assessment target for Amazon Inspector Classic to assess, you start by tagging the EC2 instances that you want to include in your target. In this task, you tag the BastionServer instance.

Every AWS tag consists of a key and value pair of your choice. For example, you might choose to name your key Name and your value MyFirstInstance.

1. In the AWS Management Console, choose **Services** and select **EC2**.

2. If you see **New EC2 Experience** at the upper-left of your screen, confirm that **New EC2 Experience** is selected. This lab is designed to use the new Amazon EC2 console.

3. In the left navigation pane, choose **Instances**.

   The running BastionServer and AppServer EC2 instances are listed.

4. Choose the BastionServer instance.

5. Choose the **Tags** tab.

6. Choose **Manage tags**.

7. Choose **Add tag**, and then enter the following information:

   - Key: SecurityScan
   - Value: true

8. Choose **Save**.

