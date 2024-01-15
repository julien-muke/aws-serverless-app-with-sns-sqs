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


6. Copy the code and we're going to put it in as an access policy on SQS, go back to SQS console, click on "Access Policy", 

![Screenshot 2024-01-13 at 16 00 48](https://github.com/julien-muke/aws-serverless-app-with-sns-sqs/assets/110755734/63ba735d-6cc6-4a3e-b976-e92aa6bdf203)




7. Then click on "Edit", you will see an example of a resource based policy, now I'm going to delete out everything that's already in there and paste in my code and then click on "Save".


![Edit-queue-Simple-Queue-Service-us-east-1-2](https://github.com/julien-muke/aws-serverless-app-with-sns-sqs/assets/110755734/d3c9a764-ae1b-4c0b-a4f9-ff0313183245)


👍 Now the SNS does have permissions to the SQS Queue.


## ➡️ Step 3 - Create a Lambda function

To create a Lambda function:

1. Navigate to the Lambda console, click on "Create fonction"


![Screenshot 2024-01-13 at 15 55 11](https://github.com/julien-muke/aws-serverless-app-with-sns-sqs/assets/110755734/25f74187-3de7-4273-a81f-cd4e2dfa4102)


2. Choose "Author from scratch", Enter a name for the function `MyTest`, choose runtime `Node.js 16.x` then click "Save"


![Create-function-Lambda (3)](https://github.com/julien-muke/aws-serverless-app-with-sns-sqs/assets/110755734/efa0378b-e104-47f8-afdb-3cdea340ba2b)


3. Next we need to update our function code, we already have a function let's come into the function and in source code let's go to `index.js` and again we've got some code in visual studio so I've opened this file in the AWS Lambda directory SQS to Lambda copy all of the code to your clipboards and let's go and paste it in we'll simply paste it into the code editor and then deploy.


```js
    exports.handler = async function(event, context) {
  event.Records.forEach(record => {
    const { body } = record;
    console.log(body);
  });
  return {};
}

```

![Screenshot 2024-01-13 at 15 57 05](https://github.com/julien-muke/aws-serverless-app-with-sns-sqs/assets/110755734/9b988d03-8484-4cd8-bee4-5af2214fbf8f)


👍 This Lambda function code is going to pass the messages from sqs and it's going to write whatever it finds in the body of the message into Cloud watch logs.


4. We do need to have some permissions to the queue for Lambda as well remember the function execution role is what determines the permissions that Lambda has when it executes so it must have permissions to read messages from the queue and then delete them from the queue.

* Back in Lambda let's go to configuration and then choose the execution role


![Screenshot 2024-01-13 at 15 59 00](https://github.com/julien-muke/aws-serverless-app-with-sns-sqs/assets/110755734/35885e98-aef3-428c-9429-11564b856a6e)


* Then I'm going to click on "ADD permissions" and attach policies let's just search for SQS the select `AWSLambdaSQSQueueExecutionRole` and what we want is the SQS execution role this does have the receive message and delete message permissions so we'll attach that policy to the role and click on "Add permissions"


![Screenshot 2024-01-13 at 16 00 03](https://github.com/julien-muke/aws-serverless-app-with-sns-sqs/assets/110755734/52665d59-fdba-4d55-a478-db88229972e7)


5. Lastly we're going to create the trigger in SQS:

* Go to Lambda triggers, click on "Configure Lambda function trigger" 

![Screenshot 2024-01-13 at 16 00 48](https://github.com/julien-muke/aws-serverless-app-with-sns-sqs/assets/110755734/ab461e7d-2d87-4e08-9516-0eff1ec14c97)



* Choose the Lambda function and then click on Save


![Screenshot 2024-01-13 at 16 01 08](https://github.com/julien-muke/aws-serverless-app-with-sns-sqs/assets/110755734/2d2f1a0e-2281-41e3-a7a6-ed74ea597854)



👍  So what have we done, we've created an SNS topic and we've subscribed an SQS Queue to the topic and we made sure that the queue has permissions to allow the topic to add messages to the queue then we updated the function code and we added permissions to receive messages and delete them from the queue and of course it already has permissions to cloudwatch so when we add a message to the topic and we'll do that manually it will then push that message to the queue Lambda will then process it and write the event to cloudwatch.


## ➡️ Step 4 - Let's Test our Topic


1. Back on our topic click on "Publish message" 


![Screenshot 2024-01-13 at 16 02 48](https://github.com/julien-muke/aws-serverless-app-with-sns-sqs/assets/110755734/4c24a921-8518-4f8f-8d5b-48f379765089)




2. We're going to enter a subject I just wrote `serverless app test` and then I'm going to write `VOILA, IT WORKED!!` and click on "Publish message"


![Publish-message-demo-sns-Topics-Amazon-SNS-Simple-Notification-Service-us-east-1](https://github.com/julien-muke/aws-serverless-app-with-sns-sqs/assets/110755734/d1065f61-aefc-4c75-a001-e38ad933818d)


3. What we're going to do is go to Lambda and then go back to monitor click on view logs in cloudwatch logs 

![Screenshot 2024-01-13 at 16 06 48](https://github.com/julien-muke/aws-serverless-app-with-sns-sqs/assets/110755734/7250ee39-45a6-440e-a79f-98ebbabd3a1c)


* We can see a very recent execution happened I know that this is the most recent one 


![Screenshot 2024-01-13 at 16 07 44](https://github.com/julien-muke/aws-serverless-app-with-sns-sqs/assets/110755734/ce874eea-cf56-4066-9adc-9c6012fc2e13)


* If i expand each logs and we can see the information, we can see the subject and we can see the message which I added manually.


![Screenshot 2024-01-13 at 16 09 09](https://github.com/julien-muke/aws-serverless-app-with-sns-sqs/assets/110755734/f7f1711f-197a-4175-8bde-57c05aae5978)

👍 That's it, a really simple serverless application that is event driven.

## 💰 Cost

All services used are eligible for the AWS Free Tier. However, charges will incur at some point so it's recommended that you shut down resources after completing this tutorial.