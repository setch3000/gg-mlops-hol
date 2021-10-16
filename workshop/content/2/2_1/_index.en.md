+++
title = "Create Cloud9 Environment"
chapter = true
weight = 31
+++

{{% notice note %}}
In actual case, you will setup AWS IoT Fleet Provisioning on machines for administration and setup Greengrass V2 core software on edge devices.
In this lab, you will create a separate Cloud9 instance which acts as an edge devices.
{{% /notice %}}

Please go to [AWS Cloud9 console](https://console.aws.amazon.com/cloud9/home/create?region=us-east-1) and create an environment.

Please enter a name (e.g. ```Workshop GGv2 Core Device```).

![1.png](/images/2/1/1.png)

Please follow below settings and click ***Next step***.
+ Create a new EC2 instance for environment (direct access)
+ t3.small (2 GiB RAM + 2 vCPU)
+ Ubuntu Server 18.04 LTS

![2.png](/images/2/1/2.png)

Please review envionment name and settiongs, and click ***Create Environment***.

![3.png](/images/2/1/3.png)

You can see the screen Cloud9 is creating. This process can take several minutes.

![4.png](/images/2/1/4.png)


Please click 'gear icon' in the upper left, and click 'Show Home in Favorites'.
![5.png](/images/2/1/5.png)


When Cloud9 is created, the following screen is shown. If the result is printed out by entering a Linux command such as 'ls' or 'pwd' in the terminal at the bottom of the screen, it was created normally.

![6.png](/images/2/1/6.png)


Please click 'gear icon' in the upper right, and please find 'AWS SETTINGS' and turn off 'AWS managed temporary creditionals'.

{{% notice note %}}
Cloud9 has AWS managed temporary credentials' to call AWS CLI in default. However, edge devices where Greengrass V2 core software is installed do not have AWS credentials in default.
Therefore, you turned off 'AWS managed temporary credentials' for making similar situation to actual edge devices.
{{% /notice %}}

![7.png](/images/2/1/7.png)
