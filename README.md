<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Build a Three-Tier Web App

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-compute-threetier)

**Author:** Muhammad Dawood Jan  
**Email:** muhammaddawoodjan989@gmail.com

---

## Build a Three-Tier Web App

![Image](http://learn.nextwork.org/blissful_turquoise_brave_goblin/uploads/aws-compute-threetier_2b3c4d5e)

---

## Introducing Today's Project!

In this project, I will demonstrate the implementation of a three-tier architecture by integrating AWS services for a scalable and efficient web application. I'm doing this project to learn how to:
Host and deliver a static website using Amazon S3 and distribute it globally with CloudFront (Presentation Tier). Build application logic with AWS Lambda and expose it via API Gateway (Logic Tier).
Store and manage data in DynamoDB, accessed securely through Lambda functions (Data Tier).
Through this project, I aim to gain hands-on experience in connecting these services to work together seamlessly, creating a robust, serverless, and production-ready architecture.

### Tools and concepts

Services I used were Amazon S3, CloudFront, API Gateway, AWS Lambda, and DynamoDB. Key concepts I learnt include Lambda functions for serverless logic execution, API Gateway for creating and exposing APIs, CORS configuration for enabling secure cross-origin requests, CloudFront for fast global content delivery, S3 for static website hosting, and DynamoDB for storing and retrieving application data in a fully managed NoSQL database.

### Project reflection

This project took me approximately a couple of hours. The most challenging part was troubleshooting CORS errors and ensuring the frontend could successfully communicate with the backend through API Gateway, as it required configuring headers both in API Gateway and within the Lambda function. It was most rewarding to see the full three-tier architecture working together, with data being retrieved from DynamoDB and displayed on the CloudFront-hosted website exactly as intended.

I did this project today to gain hands-on experience with building a complete three-tier architecture using AWS services and to better understand how the presentation, logic, and data tiers integrate in a serverless environment. This project met my goals because I successfully implemented and connected all tiers, resolved real-world issues like CORS errors, and learned how to securely and efficiently deliver a fully functional web application.

---

## Presentation tier

For the presentation tier, I will set up a static website hosted on Amazon S3 and delivered globally through Amazon CloudFront because this combination ensures fast, secure, and highly available content delivery to users worldwide while reducing latency and improving the overall user experience.

I accessed my delivered website by copying the distribution's domain name from the CloudFront distribution page. Prior to this, I configured the distribution’s default root object to `index.html` to ensure the correct file was served when accessing the site.

![Image](http://learn.nextwork.org/blissful_turquoise_brave_goblin/uploads/aws-compute-threetier_3a4b5c6d)

---

## Logic tier

For the logic tier, I'll set up an AWS Lambda function integrated with API Gateway because this enables serverless execution of application logic, allowing me to securely fetch data from DynamoDB, handle incoming requests, and respond to users without the need to manage or provision servers.

The Lambda function retrieves data by querying the DynamoDB table using the AWS SDK, while processing the request parameters to identify the required items, and then returning the fetched data as a response to the API Gateway request.

![Image](http://learn.nextwork.org/blissful_turquoise_brave_goblin/uploads/aws-compute-threetier_6a7b8c9d)

---

## Data tier

For the data tier, I will set up a DynamoDB table because it provides a fully managed, highly available, and scalable NoSQL database solution to store and retrieve user data efficiently without the need for managing underlying infrastructure.

The partition key for my DynamoDB table is UserID which means each item in the table is uniquely identified by this key, allowing efficient retrieval of user-specific data and ensuring that queries are optimized for performance.

![Image](http://learn.nextwork.org/blissful_turquoise_brave_goblin/uploads/aws-compute-threetier_u1v2w3x4)

---

## Logic and Data tier

Once all three layers of my three-tier architecture are set up, the next step is to integrate them by updating my website’s JavaScript code to call the API Gateway endpoint because this allows the presentation tier to dynamically fetch and display data from the data tier through the logic tier, creating a fully functional and connected application.

To test my API, I copied the Invoke URL of the `prod` stage from API Gateway and appended `/users?userId=1` to it. The response `{"email":"test@example.com","name":"Test User","userId":"1"}` confirmed that the data tier integration was successful and the application can correctly retrieve user data through the logic tier.

![Image](http://learn.nextwork.org/blissful_turquoise_brave_goblin/uploads/aws-compute-threetier_a112c3d5)

---

## Console Errors

The error in my distributed site was because the script.js file still contained the placeholder `[YOUR-PROD-API-URL]` instead of my actual API Gateway prod stage Invoke URL, so the frontend was making requests to an invalid endpoint and failing to retrieve any data.

To resolve the error, I updated script.js by replacing the placeholder `[YOUR-PROD-API-URL]` with my actual API Gateway prod stage Invoke URL. I then reuploaded it into S3 because the CloudFront distribution serves files directly from the S3 bucket, so updating the file there ensures the frontend uses the correct API endpoint when making requests.

I ran into a second error after updating script.js. This was an error with CORS (Cross-Origin Resource Sharing) because my API Gateway was not configured to accept requests from my CloudFront domain, and it was only allowing requests coming directly from its own Invoke URL.

![Image](http://learn.nextwork.org/blissful_turquoise_brave_goblin/uploads/aws-compute-threetier_a1b2c3d5)

---

## Resolving CORS Errors

To resolve the CORS error, I first enabled CORS in my API Gateway by allowing both GET and OPTIONS methods under Access-Control-Allow-Methods, and then set the Access-Control-Allow-Origin value to my CloudFront distribution domain name so that requests from my hosted frontend could be accepted by the API.

I also updated my Lambda function because the CORS headers needed to be included directly in the function’s HTTP response for the browser to accept requests from my CloudFront domain. The changes I made were adding the `Access-Control-Allow-Origin` header (initially set to `*` for testing, but to be replaced with my CloudFront domain for security) along with the `Content-Type` header in every response path, ensuring that both successful and error responses support cross-origin requests.

![Image](http://learn.nextwork.org/blissful_turquoise_brave_goblin/uploads/aws-compute-threetier_1qthryj2)

---

## Fixed Solution

I verified the fixed connection between API Gateway and CloudFront by reloading my CloudFront-hosted website, performing a search request through the frontend, and confirming that the API response data from DynamoDB was successfully displayed without triggering any CORS errors in the browser console.

![Image](http://learn.nextwork.org/blissful_turquoise_brave_goblin/uploads/aws-compute-threetier_2b3c4d5e)

---

---
