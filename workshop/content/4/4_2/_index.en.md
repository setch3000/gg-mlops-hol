+++
title = "On device ML inference"
weight = 52
+++



Please visit [IAM > Roles](https://console.aws.amazon.com/iamv2/home#/roles) console and please paste ```GGv2WSTokenExchangeRole``` in search filed to find the token exchange IAM role for Greengrass V2.

You can find the role named similar to 'GGv2Workshop-GGv2WSTokenExchangeRole-X65E90JE1G7O'. Please click the role.

![1.png](/images/4/2/1.png)

Please click ***Attach Policies***.
![2.png](/images/4/2/2.png)


Please switch to JSON tab.
Please copy below JSON and paste, don't forget to replace ***[Greengrass Bucket]*** with the bucket name you made in [lab 1](/en/1/1_1.html). Please click ***Review Policy***.

``` json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::[Greengrass Bucket]/*"
    }
  ]
}
```

![3.png](/images/4/2/3.png)

Please paste ```GGv2WSComponentArtifactPolicy``` in Name field and click ***Create policy***.

![4.png](/images/4/2/4.png)

You can find ***GGv2WSComponentArtifactPolicy*** is attached.

![5.png](/images/4/2/5.png)


<!-- ``` shell
aws iam create-policy \
  --policy-name GGv2WSComponentArtifactPolicy \
  --policy-document file://component-artifact-policy.json
```

``` shell
aws iam attach-role-policy \
  --role-name GGv2WSTokenExchangeRole \
  --policy-arn arn:aws:iam::123456789012:policy/GGv2WSComponentArtifactPolicy
``` -->


## Deploy ImgClassification component

Please go to [AWS IoT > Greengrass > Components](https://console.aws.amazon.com/iot/home?#/greengrass/v2/components), and Please click ***com.example.ImgClassification*** component you made in [lab 1](/en/1/1_1.html).

![6.png](/images/4/2/6.png)

Please click ***Deploy***.

![7.png](/images/4/2/7.png)

Please check ***Deployment for GGv2WSMyGreengrassCore*** and click ***Next***, then you will redirected to the Deployment.

![8.png](/images/4/2/8.png)

Please click ***Next*** in Step 1.

![9.png](/images/4/2/9.png)

Please leave as ***com.example.ImgClassification*** component is checked and click ***Next*** in Step 2.

![10.png](/images/4/2/10.png)

Please click ***Next*** in Step 3.

![11.png](/images/4/2/11.png)

Please leave all settings as default in Step 4.

![12.png](/images/4/2/12.png)

Please click ***Deploy*** in Step 5.

![13.png](/images/4/2/13.png)
