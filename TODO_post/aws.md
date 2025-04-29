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

## Virtual Private Cloud (VPC)

![virtual private cloud](/images/virtual_private_cloud.png)

![availability zone](/images/availability_zone.png)

Private cloud to isolate your resources from the world's.

## Elastic Cloud Compute (EC2)

A cloud virtual machine that you rent.

#### SSH

Go to the security group and set a rule for SSH access over TCP using port 22 with source as output of

```bash
curl -4 ifconfig.me
```

Then connect

```bash
ssh -4 -v -i namikey.pem ec2-user@XXX
```

#### Docker

Start the docker daemon

```bash
sudo systemctl start docker
```

Enable to start on boot

```bash
sudo systemctl enable docker
```

Run the container in the background and restart automatically

Port 8000 in container > 80 in EC2 instance > public IP of instance

```bash
docker run --detach --restart unless-stopped -p 80:8000 image
```

See the logs of a container

```bash
docker logs -f container
```

Connect to container shell

```bash
docker exec -it container /bin/bash
```

## S3

Cloud storage organized into buckets.

By default buckets are completely private. Individual files or folders can be made public.

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

S3 buckets will not store empty folders.
