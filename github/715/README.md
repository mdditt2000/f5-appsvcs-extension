## Seeing the following error which does make sense based on the docs and BIG-IP UI

![ui](https://github.com/mdditt2000/f5-appsvcs-extension/blob/master/github/715/diagram/2023-05-23_14-38-10.png)

```
{
    "id": "8ad27bd6-f73e-42b8-859b-0eb0840461dc",
    "results": [
        {
            "code": 422,
            "message": "declaration failed",
            "response": "01020037:3: The requested Prober Pool (/Common/ProberPool) already exists.",
            "host": "localhost",
            "tenant": "Common",
            "runTime": 4631
        },
        {
            "code": 422,
            "message": "declaration failed",
            "response": "01020037:3: The requested Prober Pool (/Common/ProberPool) already exists.",
            "host": "localhost",
            "tenant": "Common",
            "runTime": 4307
        }
    ],
    "declaration": {
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
                    "devices": [
                        {
                            "address": "10.192.125.63"
                        }
                    ],
                    "virtualServerDiscoveryMode": "enabled",
                    "exposeRouteDomainsEnabled": true
                }
            }
        },
        "$schema": "https://raw.githubusercontent.com/F5Networks/f5-appsvcs-extension/master/schema/3.44.0/as3-schema.json",
        "class": "ADC",
        "schemaVersion": "3.40.0",
        "id": "GitHub15",
        "updateMode": "selective",
        "controls": {
            "archiveTimestamp": "2023-05-23T22:07:49.912Z"
        }
    }
}
```