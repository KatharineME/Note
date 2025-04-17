+++ 
date = "2023-05-26"
title = "AWS CLI and S3"
slug = "aws_cli_and_s3"
tags = []
categories = []
+++

## Accounts and Users

An AWS Organization is a unit that can hold multiple AWS accounts.

AWS users are conceptually separate from AWS accounts. There are two types of users: root and IAM.

A root user's login information was used to create an account. The root user has supreme control over that account. But a root user cannot interface with other accounts.

An IAM user can interface with multiple accounts and have varying degrees of power in each (Administrative access for example). But an IAM user will never have the power level of a root user.

## S3

Cloud storage organized into buckets.

By default buckets are completely private. Individual files or folders can be made public however.

Bucket names must be globally unique, lowercase, and can only include hypens or periods.

https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucketnamingrules.html

## CLI

#### Setup

##### Install.

https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

##### Option 1: Configure with root user credentials (Less secure, more simple).

Login to AWS Console.

Go to username drop down > Security Credentials > Scroll down to "Access keys" section > Create a access key > Check the box > Create access key.

Take a screenshot of the keys. You won't be able to see the keys again, but you can always generate new ones.

```zsh
aws configure
```

Enter prompted information.

```zsh
AWS Access Key ID [None]: XXX
AWS Secret Access Key [None]: XXX
Default region name [None]: us-west-2
Default output format [None]: json
```

AWS CLI uses the default region when you run a command where the region to use is not self evident.

##### Option 2: Configure with IAM user credentials (More secure, less simple).

You need to login every aws cli session. If your IAM user has access to multiple accounts, this is useful because you only need to login once to access all accounts.

https://docs.aws.amazon.com/cli/latest/userguide/sso-configure-profile-token.html

#### Use with S3

List buckets.

```zsh
aws s3 ls
```

Make bucket.

```zsh
aws s3 mb s3://mybucket
```

##### Sync

Sync local folder to bucket.

```zsh
aws s3 sync ~/Downloads/thing s3://mybucket
```

Sync bucket to local folder.

```zsh
aws s3 sync s3://mybucket ~/Downloads/thing
```

If you delete a file in ~/Downloads/thing and sync to mybucket, the file will not be deleted in mybucket unless you pass the `--delete` flag.

```zsh
aws s3 sync ~/Downloads/thing s3://mybucket --delete
```

Sync file. This command will override the target file if it already exists.

```zsh
aws s3 sync s3://mybucket/folder/file.txt ~/Downloads/file.txt
```

Like Github repositories, S3 buckets will only store non-empty folders.

##### More

https://docs.aws.amazon.com/cli/latest/reference/s3/
