+++
title = "IAM role and IoT policy for Greengrass Core device"
weight = 31
+++

Through this lab, you will make 
+ ***GGv2WSTokenExchangeRole*** : A token exchange IAM role, which core devices use to authorize calls to AWS services.
+ ***GGv2WSTokenExchangeRoleAlias*** : An AWS IoT role alias that points to the token exchange role.
+ ***GGv2WSIoTThingPolicy***: An AWS IoT policy, which core devices use to authorize calls to the AWS IoT and AWS IoT Greengrass services.

Device certificates and Thing registry for Greengrass core devices will be automatically through AWS IoT Fleet Provisioning.

![0.png](/images/2/1/0.png)


## To create a token exchange IAM role

Please click [CloudFormation Link](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=GGv2Workshop&templateURL=https://sehyul.s3.ap-northeast-2.amazonaws.com/gg-workshop/cfn-gg-mlops.json) to create a IAM Role with policy document that the token exchange role requires.

In Step 1, please leave all settings as default and click ***Next***.

![1.png](/images/2/1/1.png)

In Step 2, please leave Stack name as default and click ***Next***.

![2.png](/images/2/1/2.png)

In Step 3, please leave all settings as default and click ***Next***.

![3.png](/images/2/1/3.png)

In Step 4, please check ***I acknowledge that AWS CloudFormation might create IAM resources.*** and click ***Create stack***.

![4.png](/images/2/1/4.png)


## Create an AWS IoT role alias that points to the token exchange role

In 1~2 minutes, you can find 'Arn of token exchange IAM role', 'create-role-alias command', 'Name of the IAM Role for AWS IoT fleet provisioning'	and 'Name of the IoT policy for AWS IoT fleet provisioning' in ***Outputs*** tab in the CloudFormation stack you've made.

To make an AWS IoT role alias that points to the token exchange role, please copy the command from ***CreateRoleRliasCommand*** in ***Outputs*** tab.

![5.jpg](/images/2/1/5.png)

In a terminal in Cloud9, please paste command to make an AWS IoT role alias that points to the token exchange role.
The command looks simliar to the below example.

``` shell
 aws iot create-role-alias --role-alias GGv2WSTokenExchangeRoleAlias --role-arn arn:aws:iam::644973932540:role/GGv2Workshop-GGv2WSTokenExchangeRole-X65E90JE1G7O
```

The response looks similar to the following example, if the request succeeds.
Please copy "roleAliasArn" value from the response, and paste it notepad or somewhere so that you can use it for the next section.

![6.jpg](/images/2/1/6.png)


## Create an AWS IoT policy for your Greengrass devices

Please make an empty file with below command.

``` shell
touch greengrass-v2-iot-policy.json
```

Please copy and paste the below JSON into the file, and replace [ARN of IoT Role Alias] with the IoT role alias you've made in the previous section.
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
      "Resource": "[ARN of IoT Role Alias]"
    }
  ]
}

```

Below is an example JSON with an ARN OF IoT Role Alias.

![7.jpg](/images/2/1/7.png)


<!-- You can check list of IoT Role Alias with below command.

``` shell
aws iot list-role-aliases
aws iot describe-role-alias --role-alias GGV2WSTokenExchangeRoleAlias
``` -->


Create an AWS IoT policy from the policy document with below command. In this workshop, you will create ***GGv2WSIoTThingPolicy*** IoT Policy.

``` shell
aws iot create-policy --policy-name GGv2WSIoTThingPolicy --policy-document file://greengrass-v2-iot-policy.json
```

The response looks similar to the following example, if the request succeeds.
![8.jpg](/images/2/1/8.png)

You can also find ***GGv2WSIoTThingPolicy*** IoT Policy in [AWS IoT > Secure > Policies](https://us-east-1.console.aws.amazon.com/iot/home?#/policyhub).
![9.jpg](/images/2/1/9.png)


You have made 
+ A token exchange IAM role, which core devices use to authorize calls to AWS services.
+ An AWS IoT role alias that points to the token exchange role.
+ An AWS IoT policy, which core devices use to authorize calls to the AWS IoT and AWS IoT Greengrass services.