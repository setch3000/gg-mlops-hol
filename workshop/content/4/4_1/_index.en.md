+++
title = "Local development"
weight = 51
+++

## Greengrass CLI

The Greengrass CLI component (aws.greengrass.Cli) provides a local command-line interface that you can use on core devices to develop and debug components locally.

please run below command to check if Greengrass CLI component is installed.

``` shell
/greengrass/v2/bin/greengrass-cli help
```

If above command is successful, please run below command to see component list on you Greengrass Core device.

``` shell
sudo /greengrass/v2/bin/greengrass-cli component list
```

## Creating a Greengrass componet

Please create a directory and empty file for a recipe file.

``` shell
mkdir -p ~/GGv2Dev/recipes
touch ~/GGv2Dev/recipes/com.example.HelloMqtt-1.0.0.json
```

Please use below json for recipe.

``` json
{
	"RecipeFormatVersion": "2020-01-25",
	"ComponentName": "com.example.HelloMqtt",
	"ComponentVersion": "1.0.0",
	"ComponentDescription": "My first AWS IoT Greengrass component.",
	"ComponentPublisher": "Amazon",
	"ComponentConfiguration": {
		"DefaultConfiguration": {
			"accessControl": {
				"aws.greengrass.ipc.mqttproxy": {
					"com.example.HelloMqtt:mqttproxy:1": {
						"policyDescription": "Allows access to publish to all AWS IoT Core topics.",
						"operations": [
							"aws.greengrass#PublishToIoTCore"
						],
						"resources": [
							"*"
						]
					}
				}
			}
		}
	},
	"Manifests": [{
		"Platform": {
			"os": "linux"
		},
		"Lifecycle": {
			"Install": {
				"RequiresPrivilege": true,
				"Script": "sudo pip3 install awsiotsdk"
			},
			"Run": "python3 {artifacts:path}/hello_mqtt.py"
		}
	}]
}

```

Please create a directory and empty file for a artifact file.

``` shell
mkdir -p ~/GGv2Dev/artifacts/com.example.HelloMqtt/1.0.0
touch ~/GGv2Dev/artifacts/com.example.HelloMqtt/1.0.0/hello_mqtt.py
```

Please use below source code for artifact.

```python
import json
import time
import os
import random

import awsiot.greengrasscoreipc
import awsiot.greengrasscoreipc.model as model

if __name__ == '__main__':
    ipc_client = awsiot.greengrasscoreipc.connect()

    while True:
        telemetry_data = {
            "timestamp": int(round(time.time() * 1000)),
            "battery_level": random.randrange(98, 101),
            "location": {
                "longitude": round(random.uniform(101.0, 120.0),2),
                "latitude": round(random.uniform(30.0, 40.0),2),
            },
        }

        op = ipc_client.new_publish_to_iot_core()
        op.activate(model.PublishToIoTCoreRequest(
            topic_name="ggv2/{}/telemetry".format(os.getenv("AWS_IOT_THING_NAME")),
            qos=model.QOS.AT_LEAST_ONCE,
            payload=json.dumps(telemetry_data).encode(),
        ))
        try:
            result = op.get_response().result(timeout=1.0)
            print("successfully published message:", result)
        except Exception as e:
            print("failed to publish message:", e)

        time.sleep(5)
```

Please create a local deployment.

``` shell
sudo /greengrass/v2/bin/greengrass-cli deployment create \
  --recipeDir ~/GGv2Dev/recipes \
  --artifactDir ~/GGv2Dev/artifacts \
  --merge "com.example.HelloMqtt=1.0.0"
```

![1.png](/images/4/1/1.png)


Please open New terminal and check component log.

``` shell
sudo tail -f /greengrass/v2//logs/com.example.HelloMqtt.log
```

Please go to [AWS IoT > MQTT test client](https://console.aws.amazon.com/iot/home?#/test) and subscribe ```ggv2/GGv2WSMyGreengrassCore/#```.

![2.png](/images/4/1/2.png)

## References

To remove local Greengrass component.

``` shell
sudo /greengrass/v2/bin/greengrass-cli deployment create --remove="com.example.HelloMqtt"
```

To restart local Greengrass component.

``` shell
sudo /greengrass/v2/bin/greengrass-cli component restart --names "com.example.HelloWorld"
```

To list installed Greengrass component.

``` shell
sudo /greengrass/v2/bin/greengrass-cli component list
```
