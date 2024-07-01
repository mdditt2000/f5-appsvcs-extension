### Create Datacenters

Datacenters are logical groupings of devices that share a gateway. Two examples below are **SanJose-DC** and **Charlotte-DC**

```
{
    "SanJose-DC": {
        "class": "GSLB_Data_Center",
        "enabled": true,
        "contact": "Mark Dittmer",
        "location": "San Jose, CA"
    },
    "Charlotte-DC": {
        "class": "GSLB_Data_Center",
        "enabled": true,
        "contact": "Mark Dittmer",
        "location": "Charlotte, NC"
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