
![image](https://user-images.githubusercontent.com/59709429/125682247-e4730e3f-93f2-4349-810b-cfdd4767afd6.png)

Overview:

Simple serverless web application that enables users to request unicorn rides from the Wild Rydes fleet. The application will present users with an HTML based user interface for indicating the location where they would like to be picked up and will interface on the backend with a RESTful web service to submit the request and dispatch a nearby unicorn. The application will also provide facilities for users to register with the service and log in before requesting rides.


Application Architecture:
The application architecture uses AWS Lambda, Amazon API Gateway, Amazon DynamoDB, Amazon Cognito, and AWS Amplify Console. Amplify Console provides continuous deployment and hosting of the static web resources including HTML, CSS, JavaScript, and image files which are loaded in the user's browser. JavaScript executed in the browser sends and receives data from a public backend API built using Lambda and API Gateway. Amazon Cognito provides user management and authentication functions to secure the backend API. Finally, DynamoDB provides a persistence layer where data can be stored by the API's Lambda function.

Components:
- Cognito handles user sign-up, sign-in, & access control.

- CloudFront caches static S3 web content at the edge.

- API Gateway processes API calls. Using API Gateway’s “Lambda Authorizers”, it connects a Lambda function that processes the Authorization header and returns an IAM policy.

- API Gateway then uses that policy to determine if it is valid for the resource and either routes the request or rejects it. API Gateway caches the IAM policy for a period of time, so you could also classify this as the “Valet Key” pattern. API Gateway distributes logs to CloudWatch.

- Lambda handles everything required to run and scale the execution to meet actual demand with high availability. Lambda supports several programming languages and it can be called directly from any web or mobile applications.

- Lambda is integrated with API Gateway. Synchronous calls from API gateway to AWS Lambda enables the application to operate as serverless. AWS Lambda stores dynamic data in a NoSQL database DynamoDB and static data in S3 Bucket.

- MQ Broker — responsible for asynchronous communication between microservices.

- RDS stores user profiles & other business objects.
