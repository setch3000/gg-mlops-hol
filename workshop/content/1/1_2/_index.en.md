+++
title = "Create a fleet provisioning template"
weight = 22
+++

Through this lab, you will make 
+ An AWS IoT fleet provisioning template.
+ An AWS IoT provisioning claim certificate and private key for the fleet provisioning template

## Create a fleet provisioning template

Please make an empty file with below command to create a file to contain the provisioning template document

``` shell
touch greengrass-fleet-provisioning-template.json
```

Please copy and paste the below JSON into the file, and please don't forget to save the file.


``` json
{
  "Parameters": {
    "ThingName": {
      "Type": "String"
    },
    "ThingGroupName": {
      "Type": "String"
    },
    "AWS::IoT::Certificate::Id": {
      "Type": "String"
    }
  },
  "Resources": {
    "MyThing": {
      "OverrideSettings": {
        "AttributePayload": "REPLACE",
        "ThingGroups": "REPLACE",
        "ThingTypeName": "REPLACE"
      },
      "Properties": {
        "AttributePayload": {},
        "ThingGroups": [
          {
            "Ref": "ThingGroupName"
          }
        ],
        "ThingName": {
          "Ref": "ThingName"
        }
      },
      "Type": "AWS::IoT::Thing"
    },
    "MyPolicy": {
      "Properties": {
        "PolicyName": "GGv2IoTThingPolicy"
      },
      "Type": "AWS::IoT::Policy"
    },
    "MyCertificate": {
      "Properties": {
        "CertificateId": {
          "Ref": "AWS::IoT::Certificate::Id"
        },
        "Status": "Active"
      },
      "Type": "AWS::IoT::Certificate"
    }
  }
}

```


Please copy the value of ***GGv2WSFleetProvisioningRole*** in ***Outputs*** tab of the CloudFormatino stack, you've made in the previsous lab.

![1.jpg](/images/1/2/1.png)

Please use below command to create a fleet provisioning template. Please replace [ARN OF Fleet Provisioning IAM Role] with the value of ***GGv2WSFleetProvisioningRole***, before running the command.

``` shell
aws iot create-provisioning-template \
  --template-name GGv2FleetProvisioningTemplate \
  --description "A provisioning template for Greengrass core devices." \
  --provisioning-role-arn "[ARN of Fleet Provisioning IAM Role]" \
  --template-body file://greengrass-fleet-provisioning-template.json \
  --enabled
```

The response looks similar to the following example, if the request succeeds.
![2.jpg](/images/1/2/2.png)


## Create a provisioning claim certificate and private key

First, use the following command in the terminal of Cloud9 IDE to check the AWS account id currently in use.

```
aws sts get-caller-identity --query Account --output text
```

Please make a empty file for AWS IoT policy document that Greengrass core devices require

```
touch greengrass-provisioning-claim-iot-policy.json
```

Please coyp belo JSON file and paste it into the empty file. Please replace [account-id] with your AWS account id, which you've checked above.

``` json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "iot:Connect",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "iot:Publish",
        "iot:Receive"
      ],
      "Resource": [
        "arn:aws:iot:us-east-1:[account-id]:topic/$aws/certificates/create/*",
        "arn:aws:iot:us-east-1:[account-id]:topic/$aws/provisioning-templates/GGv2FleetProvisioningTemplate/provision/*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": "iot:Subscribe",
      "Resource": [
        "arn:aws:iot:us-east-1:[account-id]:topicfilter/$aws/certificates/create/*",
        "arn:aws:iot:us-east-1:[account-id]:topicfilter/$aws/provisioning-templates/GGv2FleetProvisioningTemplate/provision/*"
      ]
    }
  ]
}
```

Below screenshot shows an example of AWS IoT policy document after [account-id] was replaced with your AWS account number.

![3.jpg](/images/1/2/3.png)

Please create an AWS IoT policy from the policy document with belwo command.

``` shell
aws iot create-policy --policy-name GGv2WSProvisioningClaimPolicy --policy-document file://greengrass-provisioning-claim-iot-policy.json
```

The response looks similar to the following example, if the request succeeds.
![4.jpg](/images/1/2/4.png)


Create and save a certificate and private key to use for provisioning. AWS IoT provides client certificates that are signed by the Amazon Root certificate authority (CA).

``` shell
mkdir claim-certs
aws iot create-keys-and-certificate \
  --certificate-pem-outfile "claim-certs/claim.pem.crt" \
  --public-key-outfile "claim-certs/claim.public.pem.key" \
  --private-key-outfile "claim-certs/claim.private.pem.key" \
  --set-as-active
```

The response looks similar to the following example, if the request succeeds. 

Please copy "certificateArn" from the response and replace [certificateArn] with it in below command.
![5.jpg](/images/1/2/5.png)

``` shell
aws iot attach-policy --policy-name GGv2WSProvisioningClaimPolicy --target [certificateArn]
```

The command doesn't have any output if the request succeeds.


## Download claim certificates to your laptop

Please right click on ***claim-certs*** , and click ***Download***. Then, you can download claim certificates and private key with a ZIP file named 'claim-certs.tar.gz'.
You will use this ZIP file for setting up Greengrass V2 with AWS IoT Fleet Provisioning in the following labs.

![5.jpg](/images/1/2/6.png)
