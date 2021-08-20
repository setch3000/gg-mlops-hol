+++
title = "Set up AWS IoT fleet provisioning"
weight = 11
+++

These resources enable devices to register themselves with AWS IoT and operate as Greengrass core devices.
+ A token exchange IAM role, which core devices use to authorize calls to AWS services.
+ An AWS IoT role alias that points to the token exchange role.
+ (Optional) An AWS IoT policy, which core devices use to authorize calls to the AWS IoT and AWS IoT Greengrass services.
+ An AWS IoT fleet provisioning template.
+ An AWS IoT provisioning claim certificate and private key for the fleet provisioning template

## To create a token exchange IAM role

Please click [CloudFormation Link](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=GGv2Workshop&templateURL=https://sehyul.s3.ap-northeast-2.amazonaws.com/gg-workshop/cfn-gg-mlops.json) to create a IAM Role with policy document that the token exchange role requires.

## Create an AWS IoT role alias that points to the token exchange role

In a terminal in Cloud9, please use below command to make a empty file for creating a policy document.



aws cloudformation describe-stack-resource \
    --stack-name test5 \
    --logical-resource-id MyFunction

``` shell
aws iot create-role-alias --role-alias GreengrassCoreTokenExchangeRoleAlias --role-arn arn:aws:iam::576184218696:role/GGv2ML-IoTFeetProvGGRole-ZP6AWDDNTRVI
```

The response looks similar to the following example, if the request succeeds.

```json
{
    "roleAlias": "GreengrassCoreTokenExchangeRoleAlias",
    "roleAliasArn": "arn:aws:iot:us-east-1:576184218696:rolealias/GreengrassCoreTokenExchangeRoleAlias"
}
```


After copy and paste above JSON into the file, please don't forget to save the file.


S3_BUCKET

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
      "Resource": "[ARN OF Role Alias]]"
    }
  ]
}

```
