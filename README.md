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
