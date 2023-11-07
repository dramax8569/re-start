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

[![bucket-1.png](https://i.postimg.cc/DZpW528G/bucket-1.png)](https://postimg.cc/62R5pxV6)

[![desencadenador.png](https://i.postimg.cc/VvSJV3CV/desencadenador.png)](https://postimg.cc/vxs812Xr)

[![desencadenador1.png](https://i.postimg.cc/HnfrDB0s/desencadenador1.png)](https://postimg.cc/FkVr0bn2)

2. Report the word count in an email by using an SNS topic. Optionally, also send the result in an SMS (text) message.

[![1.png](https://i.postimg.cc/JzZNpB3W/1.png)](https://postimg.cc/LJ4gX50y)

[![2.png](https://i.postimg.cc/XvgwbSDH/2.png)](https://postimg.cc/3ydyX6hm)

[![3.png](https://i.postimg.cc/VNwjpL40/3.png)](https://postimg.cc/w1WyszC9)

[![4.png](https://i.postimg.cc/76s0514S/4.png)](https://postimg.cc/BLFXyKXv)

[![5.png](https://i.postimg.cc/RFVwCskM/5.png)](https://postimg.cc/QBwF0bNP)

3. Format the response message as follows:

   "The word count in the <textFileName> file is nnn."

   Replace <textFileName> with the name of the file.

4. Enter the following text as the email subject: Word Count Result

   ```
      import json
      import boto3
      
      def lambda_handler(event, context):
          # Obtiene el nombre del archivo de texto desde el evento de S3
          text_file_name = event['Records'][0]['s3']['object']['key']
         
          # Inicializa el cliente de S3 y SNS
          s3_client = boto3.client('s3')
          sns_client = boto3.client('sns')
      
          # Descarga el archivo de texto desde S3
          response = s3_client.get_object(Bucket='bucket-1-christian', Key=text_file_name)
          text_content = response['Body'].read().decode('utf-8')
      
          # Cuenta las palabras en el archivo de texto
          word_count = len(text_content.split())
      
          # Formatea el mensaje de respuesta
          response_message = f"El recuento de palabras en el archivo {text_file_name} es {word_count}."
          print(f"El recuento de palabras en el archivo {text_file_name} es {word_count}.")
      
          # Publica el mensaje en un tema de SNS
          sns_client.publish(
              TopicArn='arn:aws:sns:us-west-2:095562198387:tema-1',
              Subject='Resultado del recuento de palabras',
              Message=response_message
          )
      
          return {
              'statusCode': 200,
              'body': json.dumps('Conteo de palabras completado.')
          }
   ```

6. Automatically invoke the function when the text file is uploaded to an S3 bucket.

7. Test the function by uploading a few sample text files with different word counts to the S3 bucket.

[![1.png](https://i.postimg.cc/rsJqgjhP/1.png)](https://postimg.cc/H8rGkw1w)

[![2.png](https://i.postimg.cc/28kk3pyR/2.png)](https://postimg.cc/Z0QtDM6V)



-----------

[![4.png](https://i.postimg.cc/GmD5CtRc/4.png)](https://postimg.cc/zyJjwJ3M)

[![3.png](https://i.postimg.cc/4NWJ98HK/3.png)](https://postimg.cc/svZdF4cy)



8. Forward the email that one of your tests produces and a screenshot of your Lambda function to your instructor.

[![5.png](https://i.postimg.cc/52GbP63b/5.png)](https://postimg.cc/FYjMzF8B)

[![4.png](https://i.postimg.cc/kgD7FxWT/4.png)](https://postimg.cc/qNVfp6bK)

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

