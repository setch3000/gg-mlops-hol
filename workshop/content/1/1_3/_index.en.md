+++
title = "Greengrass configuration file"
weight = 23
+++

Through this lab, you will make 
+ A Greengrass configuration file


## Create a Thing Group

{{% notice warn %}}
The thing group must exist in your AWS account, before Fleet Provisioning
{{% /notice %}}

Please use below coammand for creating a thing group.

``` shell
aws iot create-thing-group --thing-group-name GGv2WSGreengrassCoreGroup
```


## Create and download a Greengrass configuration file

Please get the AWS IoT data endpoint for your AWS account.

``` shell
aws iot describe-endpoint --endpoint-type iot:Data-ATS
```

Please get the AWS IoT credentials endpoint for your AWS account.


``` shell
aws iot describe-endpoint --endpoint-type iot:CredentialProvider
```

Please make an empty file with below command to create a Greengrass configuration file.

``` shell
touch config.yaml
```

Please replace values of ***iotDataEndpoint*** and ***iotCredentialEndpoint*** with the endpoints you've chekced above.


``` yaml
---
services:
  aws.greengrass.Nucleus:
    version: "2.4.0"
  aws.greengrass.FleetProvisioningByClaim:
    configuration:
      rootPath: /greengrass/v2
      awsRegion: "us-east-1"
      iotDataEndpoint: "device-data-prefix-ats.iot.us-west-2.amazonaws.com"
      iotCredentialEndpoint: "device-credentials-prefix.credentials.iot.us-west-2.amazonaws.com"
      iotRoleAlias: "GGv2WSTokenExchangeRoleAlias"
      provisioningTemplate: "GGv2WSFleetProvisioningTemplate"
      claimCertificatePath: "/greengrass/v2/claim-certs/claim.pem.crt"
      claimCertificatePrivateKeyPath: "/greengrass/v2/claim-certs/claim.private.pem.key"
      rootCaPath: "/greengrass/v2/AmazonRootCA1.pem"
      templateParameters:
        ThingName: "GGv2WSMyGreengrassCore"
        ThingGroupName: "GGv2WSGreengrassCoreGroup"

```

![1.jpg](/images/1/3/1.png)

Please right click on ***config.yaml*** and click ***Download***, to download it.
You will use this this file for setting up Greengrass V2 with AWS IoT Fleet Provisioning in the following labs.

![2.jpg](/images/1/3/2.png)

After you completed this lab, you can close the browser tab of ***IoT ML*** Cloud9 IDE.
![3.jpg](/images/1/3/3.png)
