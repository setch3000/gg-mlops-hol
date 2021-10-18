+++
title = "Create ML Greengrass component"
weight = 21
+++

Please create a S3 bucket with a unique name, to upload your artifiact.
First, use the following command in the Cloud9 terminal to check the number of the AWS account currently in use.

``` shell
aws sts get-caller-identity --query Account --output text
```

Create a S3 bucket in the form of a command as follows. Please replace ***[My Account Number]*** with the 12-digit account number you checked above.

``` shell
aws s3 mb s3://mybucket-[My Account Number]/ggv2/artifacts
```

For example, if the account number is 644973932540, you can make a bucket as follows.

``` shell
aws s3 mb s3://mybucket-644973932540/ggv2/artifacts
```

Please clone source code from Git repository.

{{% notice info %}}
This example Machine Learning Greengrass component are made by ***Daekeun Kim (Sr AI/ML Solutions Architect)***, and specially allowed to be used in this workshop.
{{% /notice %}}

``` shell
git clone https://github.com/daekeun-ml/ggv2-cv-mlops-workshop.git
```

Please edit ***config.json*** under ***1.byom-imgclassification-from-scratch*** directory.
Please replace "Account" with the 12-digit account number you checked above, and replace "S3Bucket" with the bucket name as ***s3://mybucket-[My Account Number]*** format as below screenshot.

![1.png](/images/1/1.png)

Please run ***init_ubuntu.sh*** as below.

The script will performs:

+ Modify recipe file for Greengrass component in recipes folder.
+ Modify config_utils.py in artifacts folder.
+ Compress all contents of the artifacts folder into zip file and upload it to your S3 bucket.
+ Register your custom component to Greengrass v2 using ```aws greengrassv2 create-component-version --inline-recipe fileb://com.example.ImgClassification-1.0.0.json```

``` shell
cd ggv2-cv-mlops-workshop/1.byom-imgclassification-from-scratch/
sudo chmod +x init_ubuntu.sh
./init_ubuntu.sh 
```

Please input ```Y``` during the script is executed.

![2.png](/images/1/2.png)

You will find similar response, if the create-component-version request in the script succeeds.

![3.png](/images/1/3.png)

Please see each component and its latest version defined in your AWS account in the current Region. 

``` shell
aws greengrassv2 list-components
```

You will find similar response, if the componet was successfully registered.

![4.png](/images/1/4.png)
