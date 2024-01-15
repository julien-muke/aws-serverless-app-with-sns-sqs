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

👉 Deploy a simple web application that uses a HTTP API
👉 We will have some routes which will be things like `OPTION`, `GET`, `PUT` and `DELETE` for various different paths, the routes are similar to the methods that we have in the rest API
👉 We'll then have a Lambda function and a dynamodb table
👉 In front of all of these we'll have an Amazon S3 static website

👉 We're going to connect from our machines using a web browser to the static website and it's going to have some client-side code running it's going to actually have a form in which we can enter information and submit it to the API.

The website will then talk to the public endpoint of the HTTP API and via the API it will then be integrated to the Lambda function and then the dynamodb table so the request information gets passed through to Lambda processed by Lambda and dynamodb will then store the results in a table and what this application will do is very simply just add some products to our database this could be a way for a store owner to actually add new items to their database the form that's generated will also be populated with information from the table so it will always be returned back and viewable on the form as you can see at the bottom here you can then add new items and when you submit them they get added to the table and then added to the web form as well.

👉 we're going to use cloud formation for deploying the HTTP API the Lambda function and the dynamodb table so cloud formation deploys infrastructure using Code.



## ➡️ Step 1 - Create a basic user pool







## 💰 Cost

All services used are eligible for the AWS Free Tier. However, charges will incur at some point so it's recommended that you shut down resources after completing this tutorial.