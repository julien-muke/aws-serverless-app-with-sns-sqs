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


![Screenshot 2024-01-13 at 15 35 58](https://github.com/julien-muke/aws-serverless-app-with-sns-sqs/assets/110755734/39fb87ba-eaaa-42ab-8ad1-730d788b7d2d)



2. Choose "Standard" as a queue type, name the queue `MyQueue`

![Create-queue-Simple-Queue-Service-us-east-1-2](https://github.com/julien-muke/aws-serverless-app-with-sns-sqs/assets/110755734/3d092a96-c702-477a-84f0-e4f4e4c8c5e6)


3. I'm not going to change any of parameters we're just going to leave the configuration as default, then scroll down and click "Create queue"


![Create-queue-Simple-Queue-Service-us-east-1-3](https://github.com/julien-muke/aws-serverless-app-with-sns-sqs/assets/110755734/fba08c7f-3ea7-4e66-bfe1-d88f27a0b3a6)


## ➡️ Step 2 - Create a topic


A topic is a message channel. When you publish a message to a topic, it fans out the message to all subscribed endpoints.

To create a topic:

1. Navigate to the Amazon SNS console, you can go over to topics on the left hand side and click "Create topic"


![Screenshot 2024-01-15 at 15 48 48](https://github.com/julien-muke/aws-serverless-app-with-sns-sqs/assets/110755734/5bd22ed2-5f16-4eed-a336-d9f86c10e8a8)


2. Choose "Standard" and I'll simply name `demo-sns` then sroll down, don't need to change anything else and click "Create topic"

![Create-topic-Topics-Amazon-SNS-Simple-Notification-Service-us-east-1](https://github.com/julien-muke/aws-serverless-app-with-sns-sqs/assets/110755734/b88d8b99-01b6-4511-adb5-d4f59aeaa5a4)


3. There's now an option to create a subscription so let's click on "Subscription"


![Screenshot 2024-01-13 at 15 44 29](https://github.com/julien-muke/aws-serverless-app-with-sns-sqs/assets/110755734/baaad3fb-89f6-4595-b979-2228e8351dbf)


4. The topic ARN is selected for us then I'm going to choose "Amazon SQS" for the protocol and select my queue as the endpoint and then click on "Create subscription"


![Create-subscription-Subscriptions-Amazon-SNS-Simple-Notification-Service-us-east-1](https://github.com/julien-muke/aws-serverless-app-with-sns-sqs/assets/110755734/b7f13dcd-5d6a-4aca-8caa-b71a4caa69d1)


5. The SNS does need permissions to the queue so what we can do is copy the ARN of our topic:


![Screenshot 2024-01-13 at 16 02 48](https://github.com/julien-muke/aws-serverless-app-with-sns-sqs/assets/110755734/21a224f9-9477-45e7-91a6-b1a714c74a1c)


* let's go to visual studio code and open the access policy, what I need to do is
paste it into my source ARN for my topic
      
* I need to go back and get the Queue ARN, let's simply copy the Queue ARN (not the URL
make sure it's the ARN) and back in the visual studio and paste it next to resource.



```json
    {
      "Statement": [
      {
        "Effect": "Allow",
        "Principal": {
            "Service": "sns.amazonaws.com"
        },
        "Action": "sqs:SendMessage",
        "Resource": "YOUR-QUEUE-ARN",
        "Condition": {
            "ArnEquals": {
            "aws:SourceArn": "YOUR-ARN-TOPIC"
        }
      }
    }
  ]
}

```


![Screenshot 2024-01-13 at 15 48 48](https://github.com/julien-muke/aws-serverless-app-with-sns-sqs/assets/110755734/6d63ef38-e2d4-41ab-be5a-92718773866d)









## 💰 Cost

All services used are eligible for the AWS Free Tier. However, charges will incur at some point so it's recommended that you shut down resources after completing this tutorial.