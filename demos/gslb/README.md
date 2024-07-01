### Create GSLB objects in Commom Partition using AS3

#### Datacenters are logical groupings of devices that share a gateway. Two examples below are **SanJose-DC-A** and **SanJose-DC-B**

```
{
    "SanJose-DC-A": {
        "class": "GSLB_Data_Center",
        "enabled": true,
        "contact": "Mark Dittmer",
        "location": "San Jose, CA"
    },
    "SanJose-DC-B": {
        "class": "GSLB_Data_Center",
        "enabled": true,
        "contact": "Mark Dittmer",
        "location": "San Jose, CA"
    }
}
```

#### Server objects need to be defined and grouped into a Datacenter

```
{
    "SiteA_F5Demo": {
        "class": "GSLB_Server",
        "dataCenter": {
            "use": "SanJose-DC-A"
        },
        "devices": [
            {
                "address": "10.192.75.63"
            }
        ]
    },
    "SiteB_F5Demo": {
        "class": "GSLB_Server",
        "dataCenter": {
            "use": "SanJose-DC-B"
        },
        "devices": [
            {
                "address": "10.192.125.63"
            }
        ]
    }
}
```

Topology Regions

```
{
    "Any": {
        "class": "GSLB_Topology_Region",
        "members": [
            {
                "matchType": "subnet",
                "matchValue": "0.0.0.0/0"
            }
        ]
    }
}
```
Monitors

```
{
    "http_monitor": {
        "class": "GSLB_Monitor",
        "monitorType": "http",
        "target": "*:*",
        "interval": 5,
        "probeTimeout": 5,
        "send": "GET /healthcheck.htm",
        "receive": "",
        "timeout": 16
    }
}
```