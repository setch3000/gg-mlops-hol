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


``` shell
git clone https://github.com/daekeun-ml/ggv2-cv-mlops-workshop.git
cd 1.byom-imgclassification-from-scratch/
```

``` shell
sudo chmod +x init_ubuntu.sh
./init_ubuntu.sh 
```
