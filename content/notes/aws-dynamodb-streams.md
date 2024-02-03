+++
title = 'Aws Dynamodb Streams'
date = 2024-01-27T11:09:19-05:00
draft = true
+++

The purpose of this note is to document the behaviors between terraform and dynamodb streams against an AWS live environment and LocalStack. 

At a bare minimun we need a lambda function (and its execution role), a DynamoDB table with streams enabled, and the resources necessary to allow for an interaction between the two (a lambda permission and an event source mapping). 

I have provided one below which will be used in our testing:

```js
resource "aws_dynamodb_table" "simple" {
  name           = "simple"
  billing_mode   = "PAY_PER_REQUEST"
  hash_key       = "id"

  attribute {
    name = "id"
    type = "S"
  }

  stream_enabled   = false
  stream_view_type = "NEW_AND_OLD_IMAGES"
}

resource "aws_iam_role" "lambda_exec_role" {
  name = "lambda_exec_role"

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

resource "aws_lambda_function" "simple" {
  filename         = "./.terraform/tmp/simple.zip"
  function_name    = "simple"
  role             = aws_iam_role.lambda_exec_role.arn
  handler          = "index.handler"
  source_code_hash = filebase64sha256("./.terraform/tmp/simple.zip")
  runtime          = "nodejs18.x"

  environment {
    variables = {
      TABLE_NAME = aws_dynamodb_table.simple.name
    }
  }
}

resource "aws_lambda_permission" "allow_dynamodb" {
  statement_id  = "AllowExecutionFromDynamoDB"
  action        = "lambda:InvokeFunction"
  function_name = aws_lambda_function.simple.arn
  principal     = "dynamodb.amazonaws.com"
  source_arn    = aws_dynamodb_table.simple.stream_arn
}

resource "aws_lambda_event_source_mapping" "simple" {
  event_source_arn  = aws_dynamodb_table.simple.stream_arn
  function_name     = aws_lambda_function.simple.arn
  starting_position = "LATEST"
}
```