
![image](https://user-images.githubusercontent.com/59709429/125680940-f0bb98e2-04ec-4582-ad3a-b5204e41c52a.png)


![image](https://user-images.githubusercontent.com/59709429/125681498-ba784d8b-08b2-4df2-817f-970970a41f48.png)



Components:
- Cognito handles user sign-up, sign-in, & access control.

- CloudFront caches static S3 web content at the edge.

- API Gateway processes API calls. Using API Gateway’s “Lambda Authorizers”, it connects a Lambda function that processes the Authorization header and returns an IAM policy.

- API Gateway then uses that policy to determine if it is valid for the resource and either routes the request or rejects it. API Gateway caches the IAM policy for a period of time, so you could also classify this as the “Valet Key” pattern. API Gateway distributes logs to CloudWatch.

- Lambda handles everything required to run and scale the execution to meet actual demand with high availability. Lambda supports several programming languages and it can be called directly from any web or mobile applications.

- Lambda is integrated with API Gateway. Synchronous calls from API gateway to AWS Lambda enables the application to operate as serverless. AWS Lambda stores dynamic data in a NoSQL database DynamoDB and static data in S3 Bucket.

- MQ Broker — responsible for asynchronous communication between microservices.

- RDS stores user profiles & other business objects.
