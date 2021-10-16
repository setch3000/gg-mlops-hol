+++
title = "Pages Organization"
weight = 41
+++

In **Hugo**, pages are the core of your site. All of your workshop steps should be developed as pages.

#/greengrass/v2/bin/greengrass-cli help


## Creating a componet


mkdir -p ~/GGv2Dev/recipes
touch ~/GGv2Dev/recipes/com.example.HelloMqtt-1.0.0.json

mkdir -p ~/GGv2Dev/artifacts/com.example.HelloMqtt/1.0.0
touch ~/GGv2Dev/artifacts/com.example.HelloMqtt/1.0.0/hello_mqtt.py

sudo /greengrass/v2/bin/greengrass-cli deployment create \
  --recipeDir ~/GGv2Dev/recipes \
  --artifactDir ~/GGv2Dev/artifacts \
  --merge "com.example.HelloMqtt=1.0.0"

aws s3 cp \
  ~/GGv2Dev/artifacts/com.example.HelloMqtt/1.0.0/hello_mqtt.py \
  s3://sehyul-us-east-1/gg/artifacts/com.example.HelloMqtt/1.0.0/hello_mqtt.py


Artifact가 업로드되어 있는 S3의 리전이 gg와 같아야 함. why?


aws greengrassv2 create-component-version \
  --inline-recipe fileb://recipes/com.example.HelloMqtt-1.0.0.json

aws s3 cp \
  ~/GGv2Dev/artifacts/com.example.HelloWorld/1.0.0/hello_world.py \
  s3://DOC-EXAMPLE-BUCKET/artifacts/com.example.HelloWorld/1.0.0/hello_world.py


aws greengrassv2 create-component-version \
  --inline-recipe fileb://recipes/com.example.HelloWorld-1.0.0.json


# References

sudo /greengrass/v2/bin/greengrass-cli component restart \
  --names "com.example.HelloWorld"
sudo /greengrass/v2/bin/greengrass-cli component list

sudo /greengrass/v2/bin/greengrass-cli deployment create --remove="com.example.HelloMqtt"

aws s3 cp \
  ~/GGv2Dev/artifacts/com.example.HelloWorld/1.0.0/hello_world.py \
  s3://sehyul/gg-workshop/artifacts/com.example.HelloWorld/1.0.0/hello_world.py



  s3://DOC-EXAMPLE-BUCKET/artifacts/com.example.HelloWorld/1.0.0/hello_world.py


s3://sehyul/gg-workshop/