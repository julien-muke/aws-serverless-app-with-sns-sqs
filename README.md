# ![aws](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/01cd6124-8014-4baa-a5fe-bd227844d263)     Build a Serverless App using SNS, SQS and Lambda on AWS


## 🤖 Introduction

In this AWS tutorial, you'll learn how to build a simple event-driven and serverless application using Amazon SNS, Amazon SQS, and AWS Lambda. 


## 📐 Architecture Design


![Blank diagram-10](https://github.com/julien-muke/aws-serverless-app-with-sns-sqs/assets/110755734/a79f6fd4-7451-4ac6-bf7c-89d9c161f8c6)



## ⚙️ AWS Services Used

* Amazon Simple Notification Service (Amazon SNS)
* Amazon Simple Queue Service (Amazon SQS)
* AWS Lambda Function
* Amazon CloudWatch


## 🔋 Features



👉 The user is going to submit a notification to an SNS topic

👉 It's going to be integrated with a queue in other words the queue is subscribed to the topic and the message that we add to the topic ends up in the queue

👉 Then SQS is going to trigger a Lambda function

👉 The Lambda function is going to run and it's going to write some information to cloudwatch and whatever we put in the topic we're going to see that in Cloud watch logs



## ➡️ Step 1 - Create a queue

We're going to use Amazon SQS by creating a queue, sending a message to the queue, and receiving and processing the message.

To create a queue:

1. Navigate to the Amazon SQS and click on "Create queue"


![Screenshot 2024-01-13 at 15 35 58](https://github.com/julien-muke/aws-serverless-app-with-sns-sqs/assets/110755734/1ac098ef-21c9-47a1-9977-0897bdda6c32)


2. Choose "Standard" as a queue type, name the queue `MyQueue`

![Create-queue-Simple-Queue-Service-us-east-1-2](https://github.com/julien-muke/aws-serverless-app-with-sns-sqs/assets/110755734/3d092a96-c702-477a-84f0-e4f4e4c8c5e6)


3. I'm not going to change any of parameters we're just going to leave the configuration as default, then scroll down and click "Create queue"


![Create-queue-Simple-Queue-Service-us-east-1-3](https://github.com/julien-muke/aws-serverless-app-with-sns-sqs/assets/110755734/fba08c7f-3ea7-4e66-bfe1-d88f27a0b3a6)




## 💰 Cost

All services used are eligible for the AWS Free Tier. However, charges will incur at some point so it's recommended that you shut down resources after completing this tutorial.