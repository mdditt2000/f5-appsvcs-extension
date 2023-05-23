## Seeing the following error which does make sense based on the docs and BIG-IP UI

Declaration below works nicely

* Yeah, that looks like what I'd expect for using a use pointer in the declaration. And you should see the GSLB_Server under /Common, which is expected behavior.

![ui](https://github.com/mdditt2000/f5-appsvcs-extension/blob/master/github/715/diagram/2023-05-23_14-38-10.png)

```
{
    "$schema": "https://raw.githubusercontent.com/F5Networks/f5-appsvcs-extension/master/schema/3.44.0/as3-schema.json",
    "class": "ADC",
    "schemaVersion": "3.40.0",
    "id": "GitHub15",
    "Common": {
        "class": "Tenant",
        "Shared": {
            "class": "Application",
            "template": "shared",
            "Default_DC": {
                "class": "GSLB_Data_Center"
            },
            "bigip1.f5demo.com": {
                "class": "GSLB_Server",
                "dataCenter": {
                    "use": "Default_DC"
                },
                "proberPreferred": "pool",
                "proberPool": {
                    "use": "ProberPool"
                },
                "devices": [
                    {
                        "address": "10.192.125.63"
                    }
                ],
                "virtualServerDiscoveryMode": "enabled",
                "exposeRouteDomainsEnabled": true
            },
            "ProberPool": {
                "class": "GSLB_Prober_Pool",
                "enabled": true,
                "lbMode": "round-robin",
                "members": [
                    {
                        "server": {
                            "use": "/Common/bigip1.f5demo.com"
                        },
                        "memberOrder": 0
                    }
                ],
                "lbMode": "global-availability"
            }
        }
    }
}
```