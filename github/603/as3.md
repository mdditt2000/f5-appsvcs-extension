### Recreate Issue

Created the following pool

```
            "Pool_1": {
                "class": "Pool",
                "members": [
                    {
                        "servicePort": 80,
                        "serverAddresses": [
                            "10.1.0.1",
                            "10.1.0.2"
                        ],
                        "enable": true,
                        "connectionLimit": 0,
                        "rateLimit": -1,
                        "dynamicRatio": 1,
                        "ratio": 1,
                        "priorityGroup": 0,
                        "adminState": "enable",
                        "addressDiscovery": "static",
                        "shareNodes": false
                    }
                ],
```

[diagram](https://github.com/mdditt2000/f5-appsvcs-extension/blob/master/github/603/diagram/2022-05-04_13-14-34.png)
