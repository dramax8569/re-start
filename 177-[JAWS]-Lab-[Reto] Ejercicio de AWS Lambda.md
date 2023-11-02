# AWS Lambda Exercise (Challenge)

## Lab overview
In this challenge lab, you create an AWS Lambda function to count the number of words in a text file.

## Objectives
After completing this lab, you will be able to do the following:

- Create a Lambda function to count the number of words in a text file.
- Configure an Amazon Simple Storage Service (Amazon S3) bucket to invoke a Lambda function when a text file is uploaded to the S3 bucket.
- Create an Amazon Simple Notification Service (Amazon SNS) topic to report the word count in an email.

## Duration
This lab requires approximately 90 minutes to complete.

# Count Words in Text File using AWS Lambda

## Your Challenge

Create a Lambda function to count the number of words in a text file. The general steps are as follows:

1. Use the AWS Management Console to develop a Lambda function in Python and create the function's required resources.

2. Report the word count in an email by using an SNS topic. Optionally, also send the result in an SMS (text) message.

3. Format the response message as follows:

   "The word count in the <textFileName> file is nnn."

   Replace <textFileName> with the name of the file.

4. Enter the following text as the email subject: Word Count Result

5. Automatically invoke the function when the text file is uploaded to an S3 bucket.

6. Test the function by uploading a few sample text files with different word counts to the S3 bucket.

7. Forward the email that one of your tests produces and a screenshot of your Lambda function to your instructor.

## Hints

- Create all of your resources in the same AWS Region.

- You need an AWS Identity and Access Management (IAM) role for the Lambda function to access other AWS services. Because the lab policy does not permit the creation of an IAM role, use the LambdaAccessRole role. The LambdaAccessRole role provides the following permissions:

  - AWSLambdaBasicExecutionRole: This is an AWS managed policy that provides write permissions to Amazon CloudWatch Logs.

  - AmazonSNSFullAccess: This is an AWS managed policy that provides full access to Amazon SNS via the AWS Management Console.

  - AmazonS3FullAccess: This is an AWS managed policy that provides full access to all buckets via the AWS Management Console.

  - CloudWatchFullAccess: This is an AWS managed policy that provides full access to Amazon CloudWatch.

- Refer to the following lab for additional guidance:
  
  - Working with AWS Lambda

# Conclusion

Congratulations! You have successfully accomplished the following tasks:

1. Created a Lambda function to count the number of words in a text file.

2. Configured an S3 bucket to trigger the Lambda function when a text file is uploaded to the S3 bucket.

3. Created an Amazon SNS topic to deliver the word count in an email.

