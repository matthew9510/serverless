# Amazon Resource Names (ARNs)
**Amazon Resource Names (ARNs) uniquely identify AWS resources. We require an 
ARN when you need to specify a resource unambiguously across all of AWS, such as
in IAM policies, Amazon Relational Database Service (Amazon RDS) tags, and API
calls.**

ARN is really just a globally unique identifier for an individual AWS resource.
It takes one of the following formats.

    arn:partition:service:region:account-id:resource 
    arn:partition:service:region:accountid:resourcetype/resource
    arn:partition:service:region:account-id:resourcetype:resource

Finally, let’s look at the common use cases for ARN. 
1. Communication
ARN is used to reference a specific resource when you orchestrate a system
involving multiple AWS resources. For example, you have an API Gateway listening
for RESTful APIs and invoking the corresponding Lambda function based on the API
path and request method. The routing looks like the following:
    
    GET /hello_world => arn:aws:lambda:us-east1:123456789012:function:lambda-hello-world

2. IAM Policy
Example of a policy definition:

        {
            "Version": "2012-10-17", 
            "Statement": { "Effect": "Allow",
            "Action": ["s3:GetObject"],
            "Resource": "arn:aws:s3:::Hello-bucket/*"
        }

ARN is used to define which resource (S3 bucket in this case) the access is
granted for. The wildcard all resources inside the Hello-bucket.

