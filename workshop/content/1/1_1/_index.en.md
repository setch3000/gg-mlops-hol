+++
title = "Set up AWS IoT fleet provisioning"
weight = 21
+++

These resources enable devices to register themselves with AWS IoT and operate as Greengrass core devices.
+ A token exchange IAM role, which core devices use to authorize calls to AWS services.
+ An AWS IoT role alias that points to the token exchange role.
+ (Optional) An AWS IoT policy, which core devices use to authorize calls to the AWS IoT and AWS IoT Greengrass services.
+ An AWS IoT fleet provisioning template.
+ An AWS IoT provisioning claim certificate and private key for the fleet provisioning template

## To create a token exchange IAM role

Please click [CloudFormation Link](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=GGv2Workshop&templateURL=https://sehyul.s3.ap-northeast-2.amazonaws.com/gg-workshop/cfn-gg-mlops.json) to create a IAM Role with policy document that the token exchange role requires.

![1.png](/images/1/1.png)
![2.png](/images/1/2.png)
![3.png](/images/1/3.png)
![4.png](/images/1/4.png)


## Create an AWS IoT role alias that points to the token exchange role

In 1~2 minutes, you can find 'Arn of token exchange IAM role', 'create-role-alias command', 'Name of the IAM Role for AWS IoT fleet provisioning'	and 'Name of the IoT policy for AWS IoT fleet provisioning' in ***Outputs*** tab in the CloudFormation stack you've made.

To make an AWS IoT role alias that points to the token exchange role, please copy the command from ***CreateRoleRliasCommand*** in ***Outputs*** tab.

![5.jpg](/images/1/5.png)

In a terminal in Cloud9, please paste command to make an AWS IoT role alias that points to the token exchange role.
The command looks simliar to the below example.

``` shell
aws iot create-role-alias --role-alias GGV2WSTokenExchangeRoleAlias --role-arnarn:aws:iam::123456789012:role/GGv2Workshop-GGV2WSTokenExchangeRole-1UGDXWM2VHZZE
```

The response looks similar to the following example, if the request succeeds.

```json
{
    "roleAlias": "GGV2WSTokenExchangeRoleAlias",
    "roleAliasArn": "arn:aws:iot:us-east-1:123456789012:rolealias/GGV2WSTokenExchangeRoleAlias"
}
```

## Create an AWS IoT policy for your Greengrass devices

Please make an empty file with below command.

``` shell
touch greengrass-v2-iot-policy.json
```

Please copy and paste the below JSON into the file, and replace [ARN OF IoT Role Alias] with the IoT role alias you've made in the previous section.
Also, please don't forget to save the file.


``` json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:Publish",
        "iot:Subscribe",
        "iot:Receive",
        "iot:Connect",
        "greengrass:*"
      ],
      "Resource": [
        "*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": "iot:AssumeRoleWithCertificate",
      "Resource": "[ARN OF IoT Role Alias]"
    }
  ]
}

```

Below is an example JSON with an ARN OF IoT Role Alias.

``` json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:Publish",
        "iot:Subscribe",
        "iot:Receive",
        "iot:Connect",
        "greengrass:*"
      ],
      "Resource": [
        "*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": "iot:AssumeRoleWithCertificate",
      "Resource": "arn:aws:iot:us-east-1:123456789012:rolealias/GGV2WSTokenExchangeRoleAlias"
    }
  ]
}

```
