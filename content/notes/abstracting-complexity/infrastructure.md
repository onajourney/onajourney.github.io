+++
title = 'Abstracting Complexity - Infrastructure'
date = 2024-01-20T10:03:26-05:00
tags = ["Infrastructure", "AWS"]
draft = false
weight = 2
+++

The most direct way of making infrastructure more maintainable is through automation. This removes the need to manually set up infrastructure. Great! But how do we achieve this? We could, of course, write scripts to do so, but this can quickly become cumbersome. Luckily, there are better tools out there. Tools that let us handle infrastructure in a more declarative manner. Tools that manage the state of our infrastructure. Tools that let us treat infrastructure as code, which we can check into our project's repository. This is what we are going to explore in this post using two of the most popular provisioning tools, as well as my own approach. 

We will use a pseudo-application that processes HTTP requests, fetches items from a database, and returns those items in a response.

# CloudFormation/SAM
CloudFormation is AWS's own infrastructure-as-code provisioning tool. SAM (Serverless Application Model) is a superset of CloudFormation. It allows for a more succinct declaration of resources, expanding them into CloudFormation templates under the hood.

This is what our infrastructure configuration looks like using SAM:

```yml
AWSTemplateFormatVersion: 2010-09-09
Transform: AWS::Serverless-2016-10-31
Resources:
  getAllItemsFunction:
    Type: AWS::Serverless::Function
    Properties:
      Handler: src/get-all-items.getAllItemsHandler
      Runtime: nodejs12.x
      Events:
        Api:
          Type: HttpApi
          Properties:
            Path: /
            Method: GET
    Connectors:
      MyConn:
        Properties:
          Destination:
            Id: SampleTable
          Permissions:
            - Read
  SampleTable:
    Type: AWS::Serverless::SimpleTable
```
Under the hood, this does the following: Creates a Lambda function, creates a role (AWSLambdaBasicExecutionRole) for your Lambda function (to allow it to run), creates the gateway (the API endpoint), an API stage resource (to deploy your API endpoint and make it accessible), sets the Lambda permission (to allow the API gateway to call the Lambda), creates a DynamoDB table, creates a policy to allow DynamoDB access from our Lambda function, and finally attaches that policy to our Lambda role.

Just describing it is a mouthful. Declaring it without SAM would require declaring each of these sources, policies, permissions, tables, as individual blocks (taking our file from 23 lines to 111) as can be seen here.

So yes - SAM is exponentially more maintainable than CloudFormation.

# Terraform
Terraform is another infrastructure-as-code tool, but works with any cloud provider.

This is what our infrastructure configuration looks like using Terraform (using the AWS Provider):

```hcl
provider "aws" {
  region = "us-west-2"
}

resource "aws_lambda_function" "get_all_items_function" {
  function_name = "getAllItemsFunction"
  handler       = "src/get-all-items.getAllItemsHandler"
  runtime       = "nodejs12.x"

  s3_bucket = "your_lambda_source_bucket"
  s3_key    = "your_lambda_source_key.zip"

  role = aws_iam_role.lambda_iam_role.arn
}

resource "aws_iam_role" "lambda_iam_role" {
  name = "lambda_iam_role"

  assume_role_policy = jsonencode({
    Version = "2012-10-17",
    Statement = [
      {
        Action = "sts:AssumeRole",
        Effect = "Allow",
        Principal = {
          Service = "lambda.amazonaws.com"
        },
      },
    ],
  })
}

resource "aws_iam_role_policy" "lambda_policy" {
  name = "lambda_policy"
  role = aws_iam_role.lambda_iam_role.id

  policy = jsonencode({
    Version = "2012-10-17",
    Statement = [
      {
        Action = [
          "dynamodb:GetItem",
          "dynamodb:Query",
          "dynamodb:Scan",
          "dynamodb:BatchGetItem",
          "dynamodb:ConditionCheckItem",
          "dynamodb:PartiQLSelect"
        ],
        Effect   = "Allow",
        Resource = aws_dynamodb_table.sample_table.arn
      },
    ],
  })
}

resource "aws_apigatewayv2_api" "http_api" {
  name          = "httpApi"
  protocol_type = "HTTP"
}

resource "aws_apigatewayv2_route" "route" {
  api_id    = aws_apigatewayv2_api.http_api.id
  route_key = "GET /"
  target    = "integrations/${aws_apigatewayv2_integration.integration.id}"
}

resource "aws_apigatewayv2_integration" "integration" {
  api_id             = aws_apigatewayv2_api.http_api.id
  integration_type   = "AWS_PROXY"
  integration_method = "POST"
  integration_uri    = aws_lambda_function.get_all_items_function.invoke_arn
}

resource "aws_lambda_permission" "apigw_lambda" {
  statement_id  = "AllowAPIGatewayInvoke"
  action        = "lambda:InvokeFunction"
  function_name = aws_lambda_function.get_all_items_function.function_name
  principal     = "apigateway.amazonaws.com"
  source_arn    = "${aws_apigatewayv2_api.http_api.execution_arn}/*/*"
}

resource "aws_dynamodb_table" "sample_table" {
  name           = "SampleTable"
  billing_mode   = "PAY_PER_REQUEST"
  hash_key       = "id"

  attribute {
    name = "id"
    type = "S"
  }
}

```

This does require manually creating each resource (something that AWS SAM does abstract), but it does feel significantly more approachable than using CloudFormation directly, and allows for other perks such as modularization and the use of the HashiCorp Configuration Language (HCL).

# FS Terraform
FS Terraform is my own approach to infrastructure; it takes a file-based approach to resource declaration. While it still allows for infrastructure-as-code, it provides a higher level of abstraction and modularity.

This is what our infrastructure configuration looks like using FS Terraform:
```
resources/
├─ api/
|  └─ sample-endpoint.json
├─ dynamodb/
│  └─ sample_table.json
└─ lambda/
   └─ get_all_items/
      └─ index.js
```

In which our api/sample-endpoint.json would contain:

```
{
    "route_key": "GET /",
    "runLambda": "get_all_items"
}
```

Our dynamodb/sample-table.json would contain:

```
{
    "name": "SampleTable",
    "hash_key": "id",
    "attributes": [
        {
            "name": "id",
            "type": "S"
        }
    ]
}
```

And lastly, our lambda/get_all_items/index.js file would contain our Lambda code.

All other resources and policies are declared based on intent and context, and new resources are declared by simply creating new files within their respective resource folder.

{{< haiku "In cloud's vast expanse," "Blueprints flourish, take their stance," "Softly structure spawns." >}}
