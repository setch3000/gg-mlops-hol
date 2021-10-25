+++
title = "Install with Fleet Provisioning"
weight = 42
+++

## Copy claim certificates

Please click ***Workshop GGv2 Core Devices*** on FILE SYSTEM, and click ***File*** and ***Upload Local Files...***.

![1.png](/images/3/2/1.png)

Please find and upload ***claim-certs.tar.gz*** file which you've downloaded in the pervious lab.

![2.png](/images/3/2/2.png)


Please make a directoy where Greengrass V2 core software will be installed.

``` shell
sudo mkdir -p /greengrass/v2
sudo chmod 755 /greengrass
```

Please extract claim certificates and private key.

``` shell
sudo tar -xvf claim-certs.tar.gz -C /greengrass/v2
```


Please download the Amazon root certificate authority (CA) certificate. AWS IoT certificates are associated with Amazon's root CA certificate by default.

``` shell
sudo curl -o /greengrass/v2/AmazonRootCA1.pem https://www.amazontrust.com/repository/AmazonRootCA1.pem
```

Pleae check if all files are located correctly.

``` shell
sudo ls /greengrass/v2 -al
sudo ls /greengrass/v2/claim-certs -al
```

![3.png](/images/3/2/3.png)


## Download the AWS IoT Greengrass Core software

On your Cloud9 which acts as an edge device, please download the AWS IoT Greengrass Core software to a file named greengrass-nucleus-latest.zip.

``` shell
curl -s https://d2s8p88vqu9w66.cloudfront.net/releases/greengrass-nucleus-latest.zip > greengrass-nucleus-latest.zip
```

Please Unzip the AWS IoT Greengrass Core software to a folder.

``` shell
unzip greengrass-nucleus-latest.zip -d GreengrassInstaller && rm greengrass-nucleus-latest.zip
```

Please run the following command to see the version of the AWS IoT Greengrass Core software.

``` shell
java -jar ./GreengrassInstaller/lib/Greengrass.jar --version
```

![4.png](/images/3/2/4.png)


## Download the AWS IoT fleet provisioning plugin

``` shell
curl -s https://d2s8p88vqu9w66.cloudfront.net/releases/aws-greengrass-FleetProvisioningByClaim/fleetprovisioningbyclaim-latest.jar > GreengrassInstaller/aws.greengrass.FleetProvisioningByClaim.jar
```

## To install the AWS IoT Greengrass Core software

Please click ***GreengrassInstaller*** directory on FILE SYSTEM, and click ***File*** and ***Upload Local Files...***.

![5.png](/images/3/2/5.png)


Please ensure the config file is correclty located with below command.

``` shell
cat GreengrassInstaller/config.yaml 
```

Please run the Greengrass V2 installer.

``` shell
sudo -E java -Droot="/greengrass/v2" -Dlog.store=FILE \
  -jar ./GreengrassInstaller/lib/Greengrass.jar \
  --trusted-plugin ./GreengrassInstaller/aws.greengrass.FleetProvisioningByClaim.jar \
  --init-config ./GreengrassInstaller/config.yaml \
  --component-default-user ggc_user:ggc_group \
  --setup-system-service true
```

You can verify the installation by viewing the files in the root folder.

``` shell
ls /greengrass/v2
```

Please open new terminal in Cloud9 and run below command to check Greengrass core software log.

``` shell
sudo tail -f /greengrass/v2/logs/greengrass.log
```

![6.png](/images/3/2/6.png)


You can find ***GGv2WSMyGreengrassCore*** is created in [AWS IoT > Greengrass > Core devices](https://console.aws.amazon.com/iot/home?#/greengrass/v2/cores) menu, if every step succeeds.

![7.png](/images/3/2/7.png)
