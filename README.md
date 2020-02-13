# KendraWorkshop
This is a step by step guide to set up Amazon Kendra 

## Prerequisite
1) Create an AWS account and an AWS Identity and Access Management user, as specified in Sign Up for AWS. If you already dont have it created yet. Else you can just login into the account. 

2) Create an S3 bucket in the same region that you are using Amazon Kendra. For instructions, see Creating and Configuring an S3 Bucket in the Amazon Simple Storage Service Console User Guide.

3) Upload your documents to your S3 bucket.

For instructions, see Uploading, Downloading, and Managing Objects in the Amazon Simple Storage Service Console User Guide.

If you are using the console to get started, do Getting Started with the Console. If you are using the AWS CLI or the SDK, you need to create IAM roles and polices for Kendra to use to access resources.

4) To create IAM roles and policies for Kendra

Create an IAM policy and role that enables Kendra to access your Amazon CloudWatch Logs.

Sign in to the AWS Management Console and open the IAM console at https://console.aws.amazon.com/iam/.

From the left menu, choose Policies and then choose Create policy.

Choose JSON and then replace the default policy with the following:
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:PutMetricData"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "cloudwatch:namespace": "AWS/Kendra"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:DescribeLogGroups"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup"
            ],
            "Resource": [
                "arn:aws:logs:region:account ID:log-group:/aws/kendra/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:DescribeLogStreams",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": [
                "arn:aws:logs:region:account ID:log-group:/aws/kendra/*:log-stream:*"
            ]
        }
    ]
}
```
Choose Review policy.

Name the policy "KendraPolicyForGettingStartedIndex" and then choose Create policy.

From the left menu, choose Roles and then choose Create role.

Choose Another AWS account and then type your account ID in Account ID. Choose Next: Permissions.

Choose the policy that you created above and then choose Next: Tags

Don't add any tags. Choose Next: Review.

Name the role "KendraRoleForGettingStartedIndex" and then choose Create role.

Find the role that you just created. Choose the role name to open the summary. Choose Trust relationships and then choose Edit trust relationship.

Replace the existing trust relationship with the following:
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "kendra.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```
Choose Update trust policy.

Create an IAM policy and role that enables Kendra to access and index your Amazon S3 bucket.

Sign in to the AWS Management Console and open the IAM console at https://console.aws.amazon.com/iam/.

From the left menu, choose Policies and then choose Create policy.

Choose JSON and then replace the default policy with the following:
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::bucket name/*"
            ],
            "Effect": "Allow"
        },
        {
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::bucket name"
            ],
            "Effect": "Allow"
        },
        {
            "Effect": "Allow",
            "Action": [
                "kendra:BatchPutDocument",
                "kendra:BatchDeleteDocument"
            ],
            "Resource": "arn:aws:kendra:region:account ID:index/*"
        }
    ]
}
```
Choose Review policy.

Name the policy "KendraPolicyForGettingStartedDataSource" and then choose Create policy.

From the left menu, choose Roles and then choose Create role.

Choose Another AWS account and then type your account ID in Account ID. Choose Next: Permissions.

Choose the policy that you created above and then choose Next: Tags

Don't add any tags. Choose Next: Review.

Name the role "KendraRoleForGettingStartedDataSource" and then choose Create role.

Find the role that you just created. Choose the role name to open the summary. Choose Trust relationships and then choose Edit trust relationship.

Replace the existing trust relationship with the following:
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "kendra.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```
Choose Update trust policy.
