# Queue-Fed Lambda Terraform Module

This module abstracts the newly-supported pattern of triggering a Lambda function directly from an SQS queue, and handles provisining the Lambda, queue, a dead-letter queue to hold failed messages, and a log group for the lambda.

## Quickstart

```terraform

module "q_lambda" {
  source "github.com/bmd/terraform-queue-fed-lambda"

  project_prefix = "qfl-test"

  filename      = "example.zip"
  function_name = "example_func_shun"
  handler       = "index.handle"

  runtime         = "go1.x"
  lambda_iam_role = aws_iam_role.test_iam.arn

  event_batch_size = 1
}

```

## Terraform Versions

Terraform 0.12 only

## Resources




## Variables

### Required Inputs

`filename` - The path to the function's deployment package within the local filesystem. For more information on creating a deployable Lambda package, look at [AWS's Documentation](https://docs.aws.amazon.com/lambda/latest/dg/lambda-python-how-to-create-deployment-package.html)

`function_name` - A unique name for your Lambda Function.

`handler` - The function entrypoint in your code.

`role` - IAM role attached to the Lambda Function. This governs both who / what can invoke your Lambda Function, as well as what resources our Lambda Function has access to. See [Lambda Permission Model](https://docs.aws.amazon.com/lambda/latest/dg/lambda-permissions.html) for more details. 

`project_prefix` - 

`runtime` - See [runtimes](https://docs.aws.amazon.com/lambda/latest/dg/API_CreateFunction.html#SSS-CreateFunction-request-Runtime) for valid values.

### Optional Variables

`description` - 

`environment` - 

`memory_size` - 

`publish` - 

`reversed_concurrent_executions` - 

`timeout` - 

`dlq_message_retention_seconds` - 

`log_retention_in_days` - (Optional) The number of days to retain the Lambda's CloudWatch execution logs for. The default value is 14. If `0` is entered, a CloudWatch log stream will not be created (not recommended).

## Outputs

`task_queue_id` - The URL of the SQS queue.

`task_queue_arn` - The ARN of the SQS queue.

`deadletter_queue_id` - The URL of the deadletter queue in SQS (if configured).

`deadletter_queue_arn` - the ARN of the deadletter queue in SQS (if configured).

`log_stream_arn` - The ARN of the CloudWatch log stream for the Lambda execution logs (if configured).