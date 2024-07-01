### Create Datacenters

Datacenters are logical groupings of devices that share a gateway. Two examples below are **SanJose-DC** and **Charlotte-DC**

```
{
    "class": "ADC",
    "$schema": "https://raw.githubusercontent.com/F5Networks/f5-appsvcs-extension/main/schema/latest/as3-schema.json",
    "schemaVersion": "3.50.0",
    "remark": "DataCenter Example",
    "Common": {
        "class": "Tenant",
        "Shared": {
            "class": "Application",
            "template": "shared",
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
    }
}  
```