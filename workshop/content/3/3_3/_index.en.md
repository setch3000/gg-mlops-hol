+++
title = "Deploy Greengrass CLI component"
weight = 43
+++

## Greengrass CLI component

The Greengrass CLI component (aws.greengrass.Cli) provides a local command-line interface that you can use on core devices to develop and debug components locally. The Greengrass CLI lets you create local deployments and restart components on the core device.

Please run below command to check if the Greengrass CLI component (aws.greengrass.Cli) is installed.

``` shell
/greengrass/v2/bin/greengrass-cli -V
```

You will find below error, since the Greengrass CLI component (aws.greengrass.Cli) is NOT automatically installed.

![picture1.png](/images/3/3/picture1.png)

Let's start to deploy the Greengrass CLI component (aws.greengrass.Cli) manually in this lab.

## Create a deployment

Please go to [AWS IoT > Greengrass > Deployments](https://console.aws.amazon.com/iot/home?#/greengrass/v2/deployments), and click ***Create***.

![2.png](/images/3/3/2.png)

Please use ```Deployment for GGv2WSMyGreengrassCore``` for Name and select ***GGv2WSMyGreengrassCoreGroup*** as Target name, and click ***Next***.

![3.png](/images/3/3/3.png)

Please check ***aws.greengrass.Cli*** in Public components, in Step 2.

![4.png](/images/3/3/4.png)

Please click ***Next***, in Step 3.

![5.png](/images/3/3/5.png)

Please leave settings as default and click ***Next***, in Step 4.

![6.png](/images/3/3/6.png)

Please click ***Deploy*** in Step 5.

![7.png](/images/3/3/7.png)

Please try below command after a minute.

``` shell
/greengrass/v2/bin/greengrass-cli -V
```

The response looks similar to the following example, if the request succeeds.

![8.png](/images/3/3/8.png)

Please try below command to retrieve the names, component information, and runtime arguments for components installed.

``` shell
sudo /greengrass/v2/bin/greengrass-cli component list
```

![9.png](/images/3/3/9.png)
