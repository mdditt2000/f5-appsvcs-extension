### Recreate Issue

Created the pool

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
                        "adminState": "enable"
                    }
                ]
            }
```

![blue](https://github.com/mdditt2000/f5-appsvcs-extension/blob/master/github/603/diagram/2022-05-04_13-14-34.png)

Modify the pool using disable

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
                        "adminState": "disable"
                    }
                ]
            }
```

You can see the pool member turned from **blue to black**

![black](https://github.com/mdditt2000/f5-appsvcs-extension/blob/master/github/603/diagram/2022-05-04_13-24-01.png)