# Terraform_DynamoDB_S3_Lambda
## Problem Statement:

Write Terraform scripts to:
● Setup a lambda that gets triggered on file upload to the the s3 bucket
● The lambda should read the uploaded CSV file(column names/data can be of your choice)
and store it’s content in to DynamoDB table
● Create an Authenticated(token based) Rest API endpoints that do CRUD on the records
stored in DynamoDB after CSV file upload.
● Use all the Terraform best practices.
● Create modules wherever needed.
● Use a remote backend for terraform.
● All the resources should be created in Terraform(No manual).

### Step 1: 
First of all, we’ll create a project directory named mycode in our local system. Inside this directory, we’ll create a file named index.js, which will contain the following code.

``` var AWS = require('aws-sdk');
var dynamo = new AWS.DynamoDB.DocumentClient();

exports.handler = function(event, context, callback) {

    var operation = event.operation;

    if (event.tableName) {
        event.payload.TableName = event.tableName;
    }

    switch (operation) {
        case 'create':
            dynamo.put(event.payload, callback);
            break;
        case 'read':
            dynamo.get(event.payload, callback);
            break;
        default:
            callback('Unknown operation: ${operation}');
    }
};  


```


### Step 2: 
Now, we will zip index.js into index.zip using any zipping software or using CLI and will keep this zip file into the same directory only and upload on S3 bucket using aws CLI.

`$ aws s3 cp index.zip s3://mybucket98161615/index.zip`

### Step 3:
Now we’ll create an IAM access policy with JSON to grant the required permissions to the DynamoDB table. We will attach this policy to a role, and this role will then be attached to a Lambda function, which will assume the required access to DynamoDB.
~~~
{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Action": "sts:AssumeRole",
        "Principal": {
          "Service": "lambda.amazonaws.com"
        },
        "Effect": "Allow",
        "Sid": ""
      }
    ]
}
~~~~

And the policy.json contains 

```
{  
  "Version": "2012-10-17",
  "Statement":[{
    "Effect": "Allow",
    "Action": [
     "dynamodb:BatchGetItem",
     "dynamodb:GetItem",
     "dynamodb:Query",
     "dynamodb:Scan",
     "dynamodb:BatchWriteItem",
     "dynamodb:PutItem",
     "dynamodb:UpdateItem"
    ],
    "Resource": "arn:aws:dynamodb:us-east-1:689228683508:table/mydb"
   }
  ]
}
```

Configuration Details:

![Lambda Configure](https://github.com/cybshark/Terraform_DynamoDB_S3_Lambda/blob/main/Terracode/lambda.JPG)

![S3 Buckeket](https://github.com/cybshark/Terraform_DynamoDB_S3_Lambda/blob/main/Terracode/s33.JPG)

![DyananoDB](https://github.com/cybshark/Terraform_DynamoDB_S3_Lambda/blob/main/Terracode/dyDb.JPG)

#conclusion
Im Facing have lot of error to how to put data in tabel. And IAM Roles.
I try to do best but lack of experinces so I fail error.
IF Anyone know about CRUD and so please Add here.


