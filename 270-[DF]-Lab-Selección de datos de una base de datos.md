# Selecting Data from a Database

## Scenario
The database operations team has created a relational database named "world" containing three tables: city, country, and countrylanguage. Based on specific use cases defined in the lab exercise, you will write a few queries using database operators and the SELECT statement.

## Lab Overview and Objectives
This lab demonstrates how to use some common database operators and the SELECT statement.

After completing this lab, you should be able to:

- Use the SELECT statement to query a database
- Use the COUNT() function
- Use the following operators to query a database:
  - ( < )
  - ( > )
  - =
  - WHERE
  - ORDER BY
  - AND

When you start the lab, the following resources are already created for you.

[![01.jpg](https://i.postimg.cc/sxGsWPsj/01.jpg)](https://postimg.cc/2LYsRZ1M)

A Command Host instance and world database containing three tables

At the end of this lab, you will have used the SELECT statement and some common database operators:

[![02.jpg](https://i.postimg.cc/5jc1SH2w/02.jpg)](https://postimg.cc/8fbq1zW5)

A lab user is connected to a database instance. It also displays some commonly used database operations.

Sample data in this course is taken from Statistics Finland, general regional statistics, February 4, 2022.

## Duration

This lab requires approximately 45 minutes to complete.

## AWS Service Restrictions

In this lab environment, access to AWS services and service actions might be restricted to the ones that you need to complete the lab instructions. You might encounter errors if you attempt to access other services or perform actions beyond the ones that this lab describes.

### Task 1: Connect to the Command Host

In this task, you connect to an instance containing a database client, which is used to connect to a database. This instance is referred to as the Command Host.

5. In the AWS Management Console, choose the **Services** menu. Choose **Compute**, and then choose **EC2**.
6. In the left navigation menu, choose **Instances**.
7. Next to the instance labeled Command Host, select the check box and then choose **Connect**.
   Note: If you do not see the Command Host, the lab is possibly still being provisioned, or you may be using another Region.
8. For **Connect to instance**, choose the **Session Manager** tab.
9. Choose **Connect** to open a terminal window.
   Note: If the **Connect** button is not available, wait for a few minutes and try again.
10. To configure the terminal to access all required tools and resources, run the following command:

    ```bash
    sudo su
    cd /home/ec2-user/
    ```

    Tips:
    - Copy and paste the command into the Session Manager terminal window.
    - If you are using a Windows system, press Shift+Ctrl+v to paste the command.

11. To connect to the database server, run the following command in the terminal. A password was configured when the database was installed:

    ```bash
    mysql -u root --password='re:St@rt!9'
    ```

    Tip: At any stage of the lab, if the Sessions Manager window is not responsive or if you need to reconnect to the database, then follow these steps:
    - Close the Sessions Manager window, and try to reconnect using the previous steps.
    - Run the following commands in the terminal.

    Run these commands to reconnect to the database if needed:

    ```bash
    sudo su
    cd /home/ec2-user/
    mysql -u root --password='re:St@rt!9'
    ```


