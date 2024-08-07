### Create GSLB objects in Commom Partition using AS3

#### DataCenters are logical groupings of devices that share a gateway. Two examples below are SanJose-DC-A and SanJose-DC-B

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

**Note** virtualServerDiscoveryMode": "disabled and therefore using generic-host

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

#### Topology Regions

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
#### Monitors

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
#### Pools

            "Site_F5Demo_Pool": {
                "class": "GSLB_Pool",
                "enabled": true,
                "fallbackIP": "10.192.75.60",
                "lbModeAlternate": "global-availability",
                "lbModeFallback": "fallback-ip",
                "lbModePreferred": "topology",
                "members": [
                    {
                        "ratio": 1,
                        "server": {
                            "use": "/Common/Shared/SiteA_F5Demo"
                        },
                        "virtualServer": "/LB_LTM_APP/apps_443"
                    },
                    {
                        "ratio": 1,
                        "server": {
                            "use": "/Common/Shared/SiteB_F5Demo"
                        },
                        "virtualServer": "//LB_LTM_APP/apps_443"
                    }
                ],
                "monitors": [
                    {
                        "use": "http_monitor"
                    }
                ],
                "resourceRecordType": "A",
                "ttl": 30,
                "verifyMemberEnabled": true
            },
            "Site_F5Demo_WIP": {
                "class": "GSLB_Domain",
                "domainName": "f5demo.example.com",
                "poolLbMode": "topology",
                "pools": [
                    {
                        "use": "Site_F5Demo_Pool"
                    }
                ],
                "resourceRecordType": "A"
            },