# AWS X\-Ray Sample Application<a name="xray-scorekeep"></a>

The AWS X\-Ray [eb\-java\-scorekeep](https://github.com/awslabs/eb-java-scorekeep/tree/xray) sample app, available on GitHub, shows the use of the AWS X\-Ray SDK to instrument incoming HTTP calls, DynamoDB SDK clients, and HTTP clients\. The sample app uses AWS Elastic Beanstalk features to create DynamoDB tables, compile Java code on instance, and run the X\-Ray daemon without any additional configuration\.

![\[Scorekeep uses the AWS X-Ray SDK to instrument incoming HTTP calls, DynamoDB SDK
        clients, and HTTP clients\]](http://docs.aws.amazon.com/xray/latest/devguide/images/scorekeep-flow.png)

The sample is an instrumented version of the [Scorekeep](https://github.com/awslabs/eb-java-scorekeep) project on AWSLabs\. It includes a front\-end web app, the API that it calls, and the DynamoDB tables that it uses to store data\. All the components are hosted in an Elastic Beanstalk environment for portability and ease of deployment\.

Basic instrumentation with filters, plugins, and instrumented AWS SDK clients is shown in the project's `xray-gettingstarted` branch\. This is the branch that you deploy in the getting started tutorial\. Because this branch only includes the basics, you can diff it against the `master` branch to quickly understand the basics\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/xray/latest/devguide/images/scorekeep-gettingstarted-servicemap-after-github.png)

The sample application shows basic instrumentation in these files:

+ **HTTP request filter** – [https://github.com/awslabs/eb-java-scorekeep/tree/xray-gettingstarted/src/main/java/scorekeep/WebConfig.java](https://github.com/awslabs/eb-java-scorekeep/tree/xray-gettingstarted/src/main/java/scorekeep/WebConfig.java)

+ **AWS SDK client instrumentation** – [https://github.com/awslabs/eb-java-scorekeep/tree/xray-gettingstarted/build.gradle](https://github.com/awslabs/eb-java-scorekeep/tree/xray-gettingstarted/build.gradle)

The `xray` branch of the application adds the use of HTTPClient, Annotations, SQL queries, custom subsegments, an instrumented AWS Lambda function, and instrumented initialization code and scripts\. This service map shows the `xray` branch running without a connected SQL database:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/xray/latest/devguide/images/scorekeep-servicemap.png)

To support user log\-in and AWS SDK for JavaScript use in the browser, the `xray-cognito` branch adds Amazon Cognito to support user authentication and authorization\. With credentials retrieved from Amazon Cognito, the web app also sends trace data to X\-Ray to record request information from the client's point of view\. The browser client appears as its own node on the service map, and records additional information, including the URL of the page that the user is viewing, and the user's ID\.

Finally, the `xray-worker` branch adds an instrumented Python Lambda function that runs independently, processing items from an Amazon SQS queue\. Scorekeep adds an item to the queue each time a game ends\. The Lambda worker, triggered by CloudWatch Events, pulls items from the queue every few minutes and processes them to store game records in Amazon S3 for analysis\.

With all features enabled, Scorekeep's service map looks like this:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/xray/latest/devguide/images/scorekeep-servicemap-allfeatures.png)

For instructions on using the sample application with X\-Ray, see the getting started tutorial\. In addition to the basic use of the X\-Ray SDK for Java discussed in the tutorial, the sample also shows how to use the following features\.


+ [Manually Instrumenting AWS SDK Clients](scorekeep-sdkclients.md)
+ [Creating Additional Subsegments](scorekeep-subsegments.md)
+ [Recording Annotations, Metadata, and User IDs](scorekeep-annotations.md)
+ [Instrumenting Outgoing HTTP Calls](scorekeep-httpclient.md)
+ [Instrumenting Calls to a PostgreSQL Database](scorekeep-postgresql.md)
+ [Instrumenting AWS Lambda Functions](scorekeep-lambda.md)
+ [Instrumenting Startup Code](scorekeep-startup.md)
+ [Instrumenting Scripts](scorekeep-scripts.md)
+ [Instrumenting a Web App Client](scorekeep-client.md)
+ [Using Instrumented Clients in Worker Threads](scorekeep-workerthreads.md)
+ [Deep Linking to the X\-Ray Console](scorekeep-deeplinks.md)