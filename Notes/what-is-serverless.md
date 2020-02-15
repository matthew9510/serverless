# Serverless computing
Serverless computing is responsible for executing a piece of code by
dynamically allocating the resources, and only charging for the amount of 
resources used to run the code. 

Issues without serverless:
1. We are charged for keeping the server up even when we are not serving out any
requests.
2. We are responsible for up&me and maintenance of the server and all its 
resources.
3. We are also responsible for applying the appropriate security updates to the 
server.
4. As our usage scales we need to manage scaling up our server as well. And as a
result manage scaling it down when we don’t have as much usage.

Managing a server ends up distracting you from actually developing and 
maintaining an actual application. 

Serverless computing is an execution model where the cloud provider is 
responsible for executing a piece of code by dynamically allocating the
resources, and only charging for the amount of resources used to run the code. 

The code is typically run inside stateless containers that can be triggered by a
variety of events including http requests, database events, queuing services, 
monitoring alerts, file uploads, scheduled events (cron jobs), etc.
The code that is sent to the cloud provider for execution is usually in the form
of a function.

Serverless is sometimes referred to as “Functions as a Service” or “FaaS”.

While serverless abstracts the underlying infrastructure away from the
developer, servers are still involved in executing our functions.

## Microservices
The biggest change that we are faced with while transitioning to a serverless 
world is that our application needs to be architectured in the form of 
functions. You might be used to deploying your application as a single Rails or
Express monolith app. But in the serverless world you are typically required to
adopt a more microservice based architecture. You can get around this by running
your entire application inside a single function as a monolith and handling 
the routing yourself. But this isn’t recommended since it is better to reduce 
the size of your functions.

## Stateless Functions
Your functions are typically run inside secure (almost) stateless containers.
This means that you won’t be able to run code in your application server that
executes long aXer an event has completed or uses a prior execution context to
serve a request. You have to effectively assume that your function is invoked in
a new container every single time.

## Cold Starts
Since your functions are run inside a container that is brought up on demand to
respond to an event, there is some latency associated with it. This is referred
to as a **Cold Start**. 

The duration of cold starts depends on the implementation of the specific cloud
provider. On AWS Lambda it can range from anywhere between a few hundred 
milliseconds to a few seconds. It can depend on the runtime (or language) used, 
the size of the function (as a package), and of course the cloud provider in 
question. 

Aside from optimizing your functions, you can use simple tricks like a separate
scheduled function to invoke your function every few minutes to keep it warm.


