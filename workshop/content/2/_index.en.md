+++
title = "Set up Fleet Provisioning"
chapter = true
weight = 30
pre = "<b>1. </b>"
+++

These resources enable devices to register themselves with AWS IoT and operate as Greengrass core devices.

+ A token exchange IAM role, which core devices use to authorize calls to AWS services.
+ An AWS IoT role alias that points to the token exchange role.
+ (Optional) An AWS IoT policy, which core devices use to authorize calls to the AWS IoT and AWS IoT Greengrass services.
+ An AWS IoT fleet provisioning template.
+ An AWS IoT provisioning claim certificate and private key for the fleet provisioning template