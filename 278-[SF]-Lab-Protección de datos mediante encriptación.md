# Data Protection Using Encryption

## Lab Overview

Cryptography is the conversion of communicated information into a secret code that keeps the information confidential and private. Its functions include authentication, data integrity, and non-repudiation. The central function of cryptography is encryption, which transforms data into an unreadable form.

Encryption ensures privacy by keeping the information hidden from people who the information is not intended for. Decryption, the opposite of encryption, transforms encrypted data back into data; it won't make any sense until it has been properly decrypted.

In this lab, you will connect to a file server that is hosted on an Amazon Elastic Compute Cloud (Amazon EC2) instance. You will configure the AWS Encryption command-line interface (CLI) on the instance. You will create an encryption key using the AWS Key Management Service (AWS KMS). The key will be used to encrypt and decrypt data. Next, you will create multiple text files that are unencrypted by default. You will then use the AWS KMS key to encrypt the files and view them while they are encrypted. You will finish the lab by decrypting the same files and viewing the contents.

## Objectives

After completing this lab, you should be able to:

- Create an AWS KMS encryption key
- Install the AWS Encryption CLI
- Encrypt plaintext
- Decrypt ciphertext

## Duration

This lab requires approximately 45 minutes to complete.

## Lab Environment

The lab environment has one preconfigured EC2 instance named File Server. An AWS Identity and Access Management (IAM) role is attached, which allows you to connect to the instance using the AWS Systems Manager Session Manager.

All backend components, such as EC2 instances, IAM roles, and some AWS services, have been built into the lab already.


## Task 1: Create an AWS KMS Key

In this task, you will create an AWS KMS key that you will later use to encrypt and decrypt data.

**AWS Key Management Service (KMS)** allows you to create and manage cryptographic keys and control their use across a wide range of AWS services and in your applications. AWS KMS is a secure and resilient service that uses hardware security modules (HSMs) that have been validated under the Federal Information Processing Standard (FIPS) Publication 140-2, or are in the process of being validated, to protect your keys.

6. In the console, enter "KMS" in the search bar, and then choose "Key Management Service."

7. Choose "Create a key."

[![1.png](https://i.postimg.cc/Hxj3SdJ7/1.png)](https://postimg.cc/Fd5jztV9)

8. For Key type, choose "Symmetric," and then choose "Next." Symmetric encryption uses the same key to encrypt and decrypt data, which makes it fast and efficient to use. Asymmetric encryption uses a public key to encrypt data and a private key to decrypt information.


[![2.png](https://i.postimg.cc/g0gNqn0W/2.png)](https://postimg.cc/XZyfnNFH)



9. On the "Add labels" page, configure the following:
   - Alias: MyKMSKey
   - Description: Key used to encrypt and decrypt data files.
   Then, choose "Next."

[![3.png](https://i.postimg.cc/JhrpkbMx/3.png)](https://postimg.cc/sMLPFGBG)

10. On the "Define key administrative permissions" page, in the "Key administrators" section, search for and select the check box for "voclabs," and then choose "Next."

[![4.png](https://i.postimg.cc/rwChshXz/4.png)](https://postimg.cc/RWh1bL0z)

11. On the "Define key usage permissions" page, in the "This account" section, search for and select the check box for "voclabs," and then choose "Next."

[![5.png](https://i.postimg.cc/13ZCykdL/5.png)](https://postimg.cc/xctyR4nR)

12. Review the settings, and then choose "Finish."

[![6.png](https://i.postimg.cc/wTN48qwX/6.png)](https://postimg.cc/gnYH3WFj)

[![7.png](https://i.postimg.cc/G3Vg58x3/7.png)](https://postimg.cc/kDNyV5HL)

13. Choose the link for "MyKMSKey," which you just created, and copy the ARN (Amazon Resource Name) value to a text editor.

[![8.png](https://i.postimg.cc/Y2hyRfgy/8.png)](https://postimg.cc/NLv4fXd1)
    
    ```
    arn:aws:kms:us-west-2:726556837515:key/1cdb5367-f3d0-4e94-b3b0-5f5b94285242
    ```
    
   You will use this copied ARN later in the lab.

**Summary of Task 1:**
In this task, you created a symmetric AWS KMS key and gave ownership of that key to the "voclabs" IAM role that was pre-created for this lab.


# AWS KMS Key Configuration and File Server Setup

## Task 2: Configure the File Server Instance

Before you can encrypt and decrypt data, you need to set up a few things. To use your AWS KMS key, you will configure AWS credentials on the File Server EC2 instance. After that, you will install the AWS Encryption CLI (`aws-encryption-cli`), which you can use to run encrypt and decrypt commands.

15. In the AWS Management Console, enter "EC2" in the search bar, and then choose "EC2."

16. In the Instances list, select the checkbox next to the File Server instance, and then choose "Connect."

[![9.png](https://i.postimg.cc/BtfXrnt7/9.png)](https://postimg.cc/bSLyb8x1)
    
18. Choose the Session Manager tab, and then choose "Connect."

[![10.png](https://i.postimg.cc/7h25t2PT/10.png)](https://postimg.cc/N2QG5LHg)
    
19. To change to the home directory and create the AWS credentials file, run the following commands:
    
      ```
      cd ~
      aws configure
      ```

20. When prompted, configure the following:

      - AWS Access Key ID: Enter your Access Key ID and press Enter.
      - AWS Secret Access Key: Enter your Secret Access Key and press Enter.
      - Default region name: Copy and paste the Region provided from the Vocareum AWS Details page.
      - Default output format: Press Enter.

      The AWS configuration file is created, and you will update it in a later step. The previous entries are temporary placeholders.

[![11.png](https://i.postimg.cc/9fLwD2Nz/11.png)](https://postimg.cc/v16ZKJyd)

21. Navigate to the Vocareum console page, and choose the "AWS Details" button.

22. Next to "AWS CLI," choose "Show."

23. Copy and paste the code block, which starts with `[default]`, into a text editor.

24. Return to the browser tab where you are logged in to the File Server.

25. To open the AWS credentials file, run the following command:
    
      ```
      vi ~/.aws/credentials
      ```
[![12.png](https://i.postimg.cc/xC7kPRQ2/12.png)](https://postimg.cc/N96fsRLJ)

26. In the `~/.aws/credentials` file, type `dd` multiple times to delete the contents of the file.

27. Paste in the code block that you copied from Vocareum.

      The AWS credentials file should now look similar to the following:

      ```
      Example of AWS credentials file contents.
      ```

28. To save and close the file, press Escape, type `:wq`, and then press Enter.

29. To view the updated contents of the file, run the following command:

    ```
    cat ~/.aws/credentials
    ```

    Now you will install the AWS Encryption CLI and export your path. By doing this, you will be able to run the commands to encrypt and decrypt data.

[![13.png](https://i.postimg.cc/hjhzxYHs/13.png)](https://postimg.cc/qhVvH1B6)

30. To install the AWS Encryption CLI and set your path, run the following commands:

    ```
    pip3 install aws-encryption-sdk-cli
    export PATH=$PATH:/home/ssm-user/.local/bin
    ```
[![14.png](https://i.postimg.cc/FKrJFzp2/14.png)](https://postimg.cc/pmSdq2gZ)

[![15.png](https://i.postimg.cc/pLYFtzff/15.png)](https://postimg.cc/VJv6qSHv)
**Summary:**
In this task, you configured the File Server EC2 instance to use AWS credentials and installed the AWS Encryption CLI to enable encryption and decryption of data.

# Task 3: Encrypt and Decrypt Data

In this task, you will create a text file with mock sensitive data in it. You will then use encryption to secure the file contents. Afterward, you will decrypt the data and view the file contents.

30. To create the text file, run the following commands:

    ```
    touch secret1.txt secret2.txt secret3.txt
    echo 'TOP SECRET 1!!!' > secret1.txt
    ```

31. To view the contents of the secret1.txt file, run the following command:

    ```
    cat secret1.txt
    ```
[![16.png](https://i.postimg.cc/x8wz9PpR/16.png)](https://postimg.cc/64h30CX2)

32. To create a directory to output the encrypted file, run the following command:

    ```
    mkdir output
    ```

33. Copy and paste the following command to a text editor:

    ```
    keyArn=(KMS ARN)
    ```

34. In the text editor, replace `(KMS ARN)` with the AWS KMS ARN that you copied in Task 1.

35. Run the updated command in the File Server terminal. This command saves the ARN of an AWS KMS key in the `$keyArn` variable. When you encrypt using an AWS KMS key, you can identify it by using a key ID, key ARN, alias name, or alias ARN.

[![17.png](https://i.postimg.cc/WpCrm2pk/17.png)](https://postimg.cc/Rq1N4xHS)

36. To encrypt the `secret1.txt` file, run the following command:

    ```
    aws-encryption-cli --encrypt \
                      --input secret1.txt \
                      --wrapping-keys key=$keyArn \
                      --metadata-output ~/metadata \
                      --encryption-context purpose=test \
                      --commitment-policy require-encrypt-require-decrypt \
                      --output ~/output/.
    ```

    The following information describes what this command does:
    - The first line encrypts the file contents. The command uses the `--encrypt` parameter to specify the operation and the `--input` parameter to indicate the file to encrypt.
    - The `--wrapping-keys` parameter, and its required key attribute, tell the command to use the AWS KMS key that is represented by the key ARN.
    - The `--metadata-output` parameter is used to specify a text file for the metadata about the encryption operation.
    - As a best practice, the command uses the `--encryption-context` parameter to specify an encryption context.
    - The `--commitment-policy` parameter is used to specify that the key commitment security feature should be used to encrypt and decrypt.
    - The value of the `--output` parameter, `~/output/.`, tells the command to write the output file to the output directory.

  When the encrypt command succeeds, it does not return any output.

[![18.png](https://i.postimg.cc/FsSSD21n/18.png)](https://postimg.cc/svV12HYp)

37. To determine whether the command succeeded, run the following command:

    ```
    echo $?
    ```

    If the command succeeded, the value of `$?` is 0. If the command failed, the value is nonzero.

38. To view the newly encrypted file location, run the following command:

    ```
    ls output
    ```

    The output should look like the following:

    ```
    secret1.txt.encrypted
    ```

39. To view the contents of the newly encrypted file, run the following command:

    ```
    cd output
    cat secret1.txt.encrypted
    ```
[![19.png](https://i.postimg.cc/6pNvSYv6/19.png)](https://postimg.cc/rKQsRGj7)

The encryption and decryption process takes data in plaintext, which is readable and understandable, and manipulates its form to create ciphertext, which is what you are now seeing. When data has been transformed into ciphertext, the plaintext becomes inaccessible until it's decrypted.

[![20.png](https://i.postimg.cc/mDrMjkFQ/20.png)](https://postimg.cc/gwfnkYsJ)

This diagram shows how encryption works with symmetric keys and algorithms. A symmetric key and algorithm are used to convert a plaintext message into ciphertext.

40. Press Enter.

    Next, you will decrypt the `secret1.txt.encrypted` file.

41. To decrypt the file, run the following commands:

    ```
    aws-encryption-cli --decrypt \
                      --input secret1.txt.encrypted \
                      --wrapping-keys key=$keyArn \
                      --commitment-policy require-encrypt-require-decrypt \
                      --encryption-context purpose=test \
                      --metadata-output ~/metadata \
                      --max-encrypted-data-keys 1 \
                      --buffer \
                      --output .
    ```

42. To view the new file location, run the following command:

    ```
    ls
    ```

    The `secret1.txt.encrypted.decrypted` file contains the decrypted contents from the `secret1.txt.encrypted` file.

43. To view the contents of the decrypted file, run the following command:

    ```
    cat secret1.txt.encrypted.decrypted
    ```
    
[![21.png](https://i.postimg.cc/d1SCxF8g/21.png)](https://postimg.cc/2bh31P9x)

[![22.png](https://i.postimg.cc/8cJv8Y4c/22.png)](https://postimg.cc/XBWqdQT6)

    After successful decryption, you can now see the original plaintext contents of the `secret1.txt`.


[![23.png](https://i.postimg.cc/W48g0gtN/23.png)](https://postimg.cc/VSSJ8Sjp)

This diagram shows how the same secret key and symmetric algorithm from the encryption process are used to decrypt the ciphertext back into plaintext.

## Summary of Task 3

In this task, you learned how to encrypt plaintext data into ciphertext by running the `--encrypt` command. You then successfully decrypted the ciphertext back into the original, readable plaintext data.

## Conclusion

Congratulations! You have successfully completed the following tasks:

- Created an AWS KMS encryption key.
- Installed the AWS Encryption CLI.
- Encrypted plaintext data.
- Decrypted ciphertext data.

With these skills, you are now equipped to use AWS Key Management Service (AWS KMS) for encryption and decryption of sensitive data, helping to keep your information secure.

If you have any further questions or need additional assistance, feel free to explore AWS documentation or seek support from the AWS community.

Happy encrypting and decrypting!


