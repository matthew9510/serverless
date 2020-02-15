# AWS Lambda
AWS Lambda is a serverless computing service provided by AWS.
Each function runs inside a container with a 64-it Amazon Linux AMI. 
Execution environment has: 
- Memory: 128MB - 3008MB in 64 MB increments 
- Ephemeral disk space: 512MB 
- Max execution duration: 900 seconds
- Package size limit: 50MB (compressed), or 250mb (uncompressed)

You might notice that CPU is not mentioned as a part of the container 
specification. This is because you cannot control the CPU directly. As you
increase the memory, the CPU is increased as well.

The ephemeral disk space is available in the form of the `/tmp` directory.
You can only use this space for temporary storage since subsequent invocations
will not have access to this. 

**Lambdas are not intended for long running processes.**

The package size refers to all your code necessary to run your function. This
includes any dependencies (`node_modules/` directory in case of Node.js) that
your function might import.

## Lambda Function

    exports.myHandler = function(event, context, callback) {
        // Do stuff

        callback(Error error, Object result);
    };


`myHandler` is the name of our Lambda function. 

`event` object contains all the information about the event that triggered this
Lambda. In the case of an HTTP request it’ll be information about the specific
HTTP request.

`context` object contains info about the runtime our Lambda function is 
executing in. 

After we do all the work inside of our lambda function, we simply call the
`callback` function with the result (or the error) and AWS will respond
to the HTTP request with it.

## Packaging Functions
Lambda functions need to be packaged and sent to AWS. This is usually a
process of compressing the function and all its dependencies and uploading it to
a S3 bucket. And letting AWS know that you want to use this package when a
specific event takes place. To help us with this process we use the [Serverless
Framework](https://serverless.com). 

## Execution Model
The container (and the resources used by it) that runs our function is managed
completely by AWS. 

It is brought up when an event takes place and is turned off if it is not being
used. If additional requests are made while the original event is being served,
a new container is brought up to serve a request. This means that if we are
undergoing a usage spike, the cloud provider simply creates multiple instances
of the container with our function to serve those requests.

**This has some interesting implications.**

Firstly, our functions are effectively stateless.

Secondly, each request (or event) is served by a single instance of a Lambda
function. This means that you are not going to be handling concurrent requests
in your code. AWS brings up a container whenever there is a new request. It does
make some optimizations here. It will hang on to the container for a few minutes
(5 - 15mins depending on the load) so it can respond to subsequent requests
without a cold start.

## Stateless Functions
The above execution model makes Lambda functions effectively stateless. 

This means that every time your Lambda function is triggered by an event it is
invoked in a completely new environment. You don’t have access to the execution
context of the previous event.

However, due to the optimization noted above, the actual Lambda function is
invoked only once per container instantiation. Recall that our functions are
run inside containers. So when a function is first invoked, all the code in our
handler function gets executed and the handler function gets invoked. **If the** 
**container is still available for subsequent requests, your function will get** 
**invoked and not the code around it.**

For example, the `createNewDbConnection` function below is called once per
container instantiation and not every time the Lambda function is invoked.
The `myHandler` function on the other hand is called on every invocation.

    var dbConnection = createNewDbConnection();

    exports.myHandler = function(event, context, callback) {
        var result = dbConnection.makeQuery(); 
        callback(null, result);
    }; 

This caching effect of containers also applies to the `/tmp` directory that
we talked about above. It is available as long as the container is being cached.
Now you can guess that this isn’t a very reliable way to make our Lambda
functions stateful. This is because we just don’t control the underlying
process by which Lambda is invoked or its containers are cached.


## Pricing 
Finally, Lambda functions are billed only for the time it takes to execute your 
function. And it is calculated from the time it begins executing till when it
returns or terminates. It is rounded up to the nearest 100ms.
**Note** that while AWS might keep the container with your Lambda function
around after it has completed; you are not going to be charged for this.

The Lambda free tier includes 1M free requests per month and 400,000 GB-seconds
of compute time per month. Past this, it costs $0.20 per 1 million requests and
$0.00001667 for every GB-seconds. The GB-seconds is based on the memory 
consumption of the Lambda function.








