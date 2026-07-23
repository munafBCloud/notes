# IAM policies in Terraform

## best Practices

### 1. Use `aws_iam_policy_document` Whenever Possible

Generate IAM policies using Terraform instead of hardcoding JSON. This provides validation, improves readability, and makes policies easier to maintain.

### 2. Follow the Principle of Least Privilege

Grant only the permissions required for the resource or service to perform its task. Avoid wildcard permissions such as:

```json
"Action": "*"
"Resource": "*"
```

unless they are absolutely necessary.

### 3. Separate IAM Roles and Policies

Create IAM roles, IAM policies, and policy attachments as separate Terraform resources. This improves reusability, simplifies management, and keeps your infrastructure modular.

### 4. Scope Resources Explicitly

Reference specific AWS resource ARNs whenever possible instead of using `"*"`. Examples include:

- DynamoDB Tables
- S3 Buckets
- Lambda Functions
- SNS Topics
- SQS Queues

Restricting resources reduces unnecessary access and improves security.

### 5. Use Variables and Terraform References

Build policies dynamically using Terraform resource references and variables instead of hardcoded values.

Example:

```hcl
aws_dynamodb_table.inventory.arn
```

---

# Example – DynamoDB Read/Write Policy

```hcl
data "aws_iam_policy_document" "dynamodb_policy" {
  statement {
    effect = "Allow"

    actions = [
      "dynamodb:GetItem",
      "dynamodb:PutItem",
      "dynamodb:UpdateItem",
      "dynamodb:DeleteItem",
      "dynamodb:Scan",
      "dynamodb:Query"
    ]

    resources = [
      aws_dynamodb_table.inventory.arn
    ]
  }
}
```

**Purpose**

Allows a Lambda function to read, create, update, delete, scan, and query items in a DynamoDB table.

---

# Example – S3 Bucket Access

```hcl
data "aws_iam_policy_document" "s3_policy" {
  statement {
    effect = "Allow"

    actions = [
      "s3:GetObject",
      "s3:PutObject"
    ]

    resources = [
      "${aws_s3_bucket.website.arn}/*"
    ]
  }
}
```

**Purpose**

Allows an application or Lambda function to upload and download objects within an S3 bucket.

---

# Example – CloudWatch Logs

```hcl
data "aws_iam_policy_document" "cloudwatch_policy" {
  statement {
    effect = "Allow"

    actions = [
      "logs:CreateLogGroup",
      "logs:CreateLogStream",
      "logs:PutLogEvents"
    ]

    resources = [
      "arn:aws:logs:*:*:*"
    ]
  }
}
```

**Purpose**

Allows Lambda to create CloudWatch Log Groups, Log Streams, and write log events during execution.

---

# Example - Amazon SES Email Sending

```hcl
data "aws_iam_policy_document" "ses_policy" {
  statement {
    effect = "Allow"

    actions = [
      "ses:SendEmail",
      "ses:SendRawEmail"
    ]

    resources = [
      "*"
    ]
  }
}
```

**Purpose**

Allows an application or Lambda function to send emails through Amazon Simple Email Service (SES).

> **Note:** SES policies commonly use `"Resource": "*"` because email sending isn't always tied to a specific resource ARN. if possible restrict more access using IAM conditions.
