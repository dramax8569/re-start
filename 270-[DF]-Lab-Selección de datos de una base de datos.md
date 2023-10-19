# Selecting Data from a Database

## Scenario
The database operations team has created a relational database named "world" containing three tables: city, country, and countrylanguage. Based on specific use cases defined in the lab exercise, you will write a few queries using database operators and the SELECT statement.

## Lab Overview and Objectives
This lab demonstrates how to use some common database operators and the SELECT statement.

After completing this lab, you should be able to:

- Use the SELECT statement to query a database
- Use the COUNT() function
- Use the following operators to query a database:
  - `<`
  - `>`
  - `=`
  - `WHERE`
  - `ORDER BY`
  - `AND`

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

[![1.png](https://i.postimg.cc/W1wFtV59/1.png)](https://postimg.cc/cv645pyY)

8. For **Connect to instance**, choose the **Session Manager** tab.
9. Choose **Connect** to open a terminal window.
   Note: If the **Connect** button is not available, wait for a few minutes and try again.

[![2.png](https://i.postimg.cc/dQhy6z01/2.png)](https://postimg.cc/MX8HK3V2)

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

### Task 2: Query the world database

In this task, you query the world database using various SELECT statements and database operators.

12. To show the existing databases, enter the following command in the terminal:

    ```sql
    SHOW DATABASES;
    ```

    Verify that a database named world is available. If the world database is not available, then contact your instructor.

13. To list all rows and columns in the country table, run the following query:

    ```sql
    SELECT * FROM world.country;
    ```

[![3.png](https://i.postimg.cc/jqBJFZWh/3.png)](https://postimg.cc/xqyCkG3k)

14. To query the number of rows in a table, you can use the COUNT() function in a SELECT statement. To list the number of rows in the country table, run the following query:

    ```sql
    SELECT COUNT(*) FROM world.country;
    ```

[![4.png](https://i.postimg.cc/YCHLp0zG/4.png)](https://postimg.cc/tYB4kXfp)

15. To list all columns in the country table, run the following query:

    ```sql
    SHOW COLUMNS FROM world.country;
    ```
[![5.png](https://i.postimg.cc/c4Y8ywwG/5.png)](https://postimg.cc/yk1N0JLL)

16. To query specific columns in the world table, run the following query to return a result set that includes the Name, Capital, Region, SurfaceArea, and Population columns:

    ```sql
    SELECT Name, Capital, Region, SurfaceArea, Population FROM world.country;
    ```
[![6.png](https://i.postimg.cc/8CkF6Y3T/6.png)](https://postimg.cc/bZMNXmp5)

17. Database column names are sometimes not user-friendly. To add a more descriptive column name to the query output, you can use the AS option. Run the following query that includes this option:

    ```sql
    SELECT Name, Capital, Region, SurfaceArea AS "Surface Area", Population FROM world.country;
    ```

    If required, scroll to the top of the query results, and observe that the SurfaceArea column is displayed as Surface Area.

[![7.png](https://i.postimg.cc/xdsXyRDM/7.png)](https://postimg.cc/DWXf71zw)

18. Ordered result sets are easier to view and work with. If you would like to order the output based on a column, you can use the ORDER BY option. In this example, you order the output based on the population:

    ```sql
    SELECT Name, Capital, Region, SurfaceArea AS "Surface Area", Population FROM world.country ORDER BY Population;
    ```

    The ORDER BY option orders data in ascending order.

[![8.png](https://i.postimg.cc/zB634Gff/8.png)](https://postimg.cc/Kk7ZMFBX)

19. To order data in descending order, use the DESC option with ORDER BY. Run the following command with this option:

    ```sql
    SELECT Name, Capital, Region, SurfaceArea AS "Surface Area", Population FROM world.country ORDER BY Population DESC;
    ```

[![9.png](https://i.postimg.cc/brndPfG9/9.png)](https://postimg.cc/ctsd7jBv)

20. You can add conditions to SELECT statements by using the WHERE clause. For example, to list all rows with a population greater than 50,000,000, run the following query:

    ```sql
    SELECT Name, Capital, Region, SurfaceArea AS "Surface Area", Population FROM world.country WHERE Population > 50000000 ORDER BY Population DESC;
    ```

    You have used the > comparison operator. Similarly, you can use other comparison operators to compare values.

[![10.png](https://i.postimg.cc/WbRhnzKW/10.png)](https://postimg.cc/NL4sGsQm)

21. You can construct a WHERE clause by using a number of conditions and operators. The following query uses two conditions: all rows with a population greater than 50,000,000 and all rows with a population less than 100,000,000. The query includes the AND operator to indicate that both the conditions must be true:

    ```sql
    SELECT Name, Capital, Region, SurfaceArea AS "Surface Area", Population FROM world.country WHERE Population > 50000000 AND Population < 100000000 ORDER BY Population DESC;
    ```

    For more information about comparison operators, see the Additional resources section at the end of the lab.

[![11.png](https://i.postimg.cc/BZPjHHhP/11.png)](https://postimg.cc/HjHWCc1T)

### Challenge

Query the country table to return a set of records based on the following question:

**Which country in Southern Europe has a population greater than 50,000,000?**

    ```
    SELECT Name, Capital, Region, SurfaceArea AS "Surface Area", Population from world.country WHERE Population > 50000000 AND Region = "Southern Europe";
    ```

[![12.png](https://i.postimg.cc/qM8gZ0Hk/12.png)](https://postimg.cc/ppL2pwd7)

### Conclusion

Congratulations! You have now successfully:

- Used the SELECT statement to query a database
- Used the COUNT() function
- Used the following operators to query a database:
  - `<`
  - `>`
  - `=`
- Utilized the WHERE clause
- Employed the ORDER BY clause
- Combined conditions with the AND operator

