+++
title = "Create Cloud9 Environment"
chapter = true
weight = 41
+++

{{% notice note %}}
In actual case, you will setup AWS IoT Fleet Provisioning on machines for administration and setup Greengrass V2 core software on edge devices.
In this lab, you will create a separate Cloud9 instance which acts as an edge devices.
{{% /notice %}}

Please go to [AWS Cloud9 console](https://console.aws.amazon.com/cloud9/home/create?region=us-east-1) and create an environment.

Please enter a name (e.g. ```Workshop GGv2 Core Device```).

![1.png](/images/3/1/1.png)

Please follow below settings and click ***Next step***.

+ Create a new EC2 instance for environment (direct access)
+ t3.small (2 GiB RAM + 2 vCPU)
+ Ubuntu Server 18.04 LTS

![2.png](/images/3/1/2.png)

Please review envionment name and settiongs, and click ***Create Environment***.

![3.png](/images/3/1/3.png)

You can see the screen Cloud9 is creating. This process can take several minutes.

![4.png](/images/3/1/4.png)


Please click 'gear icon' in the upper left, and click 'Show Home in Favorites'.
![5.png](/images/3/1/5.png)


When Cloud9 is created, the following screen is shown. If the result is printed out by entering a Linux command such as 'ls' or 'pwd' in the terminal at the bottom of the screen, it was created normally.

![6.png](/images/3/1/6.png)

Run the following command to update your AWS CLI to latest:

``` shell
curl https://aws-iot-workshop-artifacts.s3-eu-west-1.amazonaws.com/resources/2021-03-31/src/customScripts/updateCLI.sh | sh
```

Once thats complete let’s run the following command in a Cloud9 terminal to expand Cloud9’s disk space and restart your Cloud9 instance.

``` shell
curl https://aws-iot-workshop-artifacts.s3-eu-west-1.amazonaws.com/resources/2021-03-31/src/customScripts/resize_volume.sh | sh
```

While the instance is restarting, Reconnecting ... will be displayed, so please wait until the instance restart is completed. After the instance restarts, open a new terminal in Cloud9 and run the following command to check the disk space.

``` shell
df -h
```

In the command execution result, check that / is 45GB or more as shown below.

``` shell
Filesystem      Size  Used Avail Use% Mounted on
/dev/nvme0n1p1   49G  7.9G   41G  17% /
```

Please click 'gear icon' in the upper right, and please find 'AWS SETTINGS' and turn off 'AWS managed temporary creditionals'.

{{% notice warning %}}
Cloud9 has 'AWS managed temporary credentials' to call AWS CLI in default. However, edge devices where Greengrass V2 core software is installed do not have AWS credentials in default.
Therefore, you turned off 'AWS managed temporary credentials' for making similar situation to actual edge devices.
{{% /notice %}}

![7.png](/images/3/1/7.png)
