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


