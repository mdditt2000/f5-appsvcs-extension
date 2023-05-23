{
    "id": "5aa5c07c-b123-4202-859e-257acbf5c5d8",
    "results": [
        {
            "code": 422,
            "message": "declaration failed",
            "response": "01071b5c:3: A prober pool cannot be specified for data center /Common/Default_DC unless either the prober preference or prober fallback value is set to 'pool'.",
            "host": "localhost",
            "tenant": "Common",
            "runTime": 4916
        },
        {
            "code": 422,
            "message": "declaration failed",
            "response": "01071b5c:3: A prober pool cannot be specified for data center /Common/Default_DC unless either the prober preference or prober fallback value is set to 'pool'.",
            "host": "localhost",
            "tenant": "Common",
            "runTime": 4934
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
            "archiveTimestamp": "2023-05-23T21:30:13.083Z"
        }
    }
}