# GTM + LTM using AS3

AS3 can automate BIG-IP to configure both GTM + LTM in a single declaration. In combination with using virtual server discovery to create an GSLB_Pool object which populates the WideIP. In this example the the WideIP and the LTM objects are in the same declaration. The WideIP and the LTM objects are configured using a application tenant and not the common partition

* Demo on YouTube [video]()

## Important information when using GTM + LTM using AS3

* Trying to refer to a discovered ltm virtual has some timing problems. It can take several seconds (10?) for GSLB to discover LTM virtuals and allow them to be used in a GTM Pool

There are several configuration scenarios here:

* LTM virtual, GTM server, and GTM pool are all in one declaration. In AS3-40 we added a solution to add a 10-15 second delay before creating GTM pools if there is a use that points to a GTM server with virtual discovery and always add the delay if it is a BIG-IP pointer or string

Below is the AS3 deployment for creating the Data Center and Server. Diagram displays the AS3 declarations deployed

![diagram](https://github.com/mdditt2000/f5-appsvcs-extension/blob/master/user_guides/gslb/diagram/2022-11-16_11-23-08.png)

### Step 1: Create Data Center and GTM Server using AS3

```
{
    "class": "ADC",
    "schemaVersion": "3.40.0",
    "id": "GSLB_Common",
    "Common": {
        "class": "Tenant",
        "Shared": {
            "class": "Application",
            "template": "shared",
            "Default_DC": {
                "class": "GSLB_Data_Center"
            },
            "ocp.f5demo.com": {
                "class": "GSLB_Server",
                "dataCenter": {
                    "use": "Default_DC"
                },
                "devices": [
                    {
                        "address": "10.192.125.60"
                    }
                ],
                "virtualServerDiscoveryMode": "enabled",
                "exposeRouteDomainsEnabled": true
            }
        }
    }
}
```
bigip-gslb [repo](https://github.com/mdditt2000/f5-appsvcs-extension/blob/master/user_guides/gslb/bigip-gslb-common.json)

### Step 1: Create GTM + LTM using AS3

Below is the AS3 declaration to deploy the GTM + LTM using a single AS3 declaration to BIG-IP. This AS3 declaration also installs a WAF profile and creates a LTM pool member for NGINX Ingress Controller. 

```
{
    "$schema": "https://raw.githubusercontent.com/F5Networks/f5-appsvcs-extension/master/schema/latest/as3-schema.json",
    "class": "ADC",
    "schemaVersion": "3.40.0",
    "updateMode": "selective",
    "NginxIngress": {
        "class": "Tenant",
        "Nginx.Ingress": {
            "class": "Application",
            "NginxIngress_vs": {
                "class": "Service_HTTP",
                "virtualAddresses": [
                    "10.192.125.65"
                ],
                "policyEndpoint": "nginxOrgForwardPolicy",
                "policyWAF": {
                    "use": "NginxIngress_OWASP_Auto_Tune"
                },
                "persistenceMethods": [
                    "cookie"
                ],
                "profileHTTP": "basic",
                "layer4": "tcp",
                "profileTCP": "normal"
            },
            "nginx_pl": {
                "class": "Pool",
                "monitors": [
                    "tcp"
                ],
                "members": [
                    {
                        "servicePort": 80,
                        "serverAddresses": [
                            "10.131.0.89"
                          ]
                    }
                ]
            },
            "nginxOrgForwardPolicy": {
                "class": "Endpoint_Policy",
                "rules": [
                    {
                        "name": "forward_to_pool",
                        "conditions": [
                            {
                                "type": "httpUri",
                                "path": {
                                    "operand": "contains",
                                    "values": [
                                        "/"
                                    ]
                                }
                            }
                        ],
                        "actions": [
                            {
                                "type": "forward",
                                "event": "request",
                                "select": {
                                    "pool": {
                                        "use": "nginx_pl"
                                    }
                                }
                            }
                        ]
                    }
                ]
            },
            "NginxIngress_OWASP_Auto_Tune": {
                "class": "WAF_Policy",
                "url": "https://raw.githubusercontent.com/f5devcentral/f5-asm-policy-templates/master/owasp_ready_template/owasp-auto-tune-v1.1.xml",
                "ignoreChanges": true
            },
            "nginxDomain": {
                "class": "GSLB_Domain",
                "domainName": "cafe.example.com",
                "resourceRecordType": "A",
                "poolLbMode": "ratio",
                "pools": [
                    {
                        "use": "NginxIngressPool"
                    }
                ]
            },
            "NginxIngressPool": {
                "class": "GSLB_Pool",
                "enabled": true,
                "lbModeAlternate": "ratio",
                "lbModeFallback": "ratio",
                "manualResumeEnabled": true,
                "verifyMemberEnabled": false,
                "qosHitRatio": 10,
                "qosHops": 11,
                "qosKbps": 8,
                "qosLinkCapacity": 35,
                "qosPacketRate": 5,
                "qosRoundTripTime": 75,
                "qosTopology": 3,
                "qosVirtualServerCapacity": 2,
                "qosVirtualServerScore": 1,
                "members": [
                    {
                        "ratio": 1,
                        "server": {
                            "bigip": "/Common/ocp.f5demo.com"
                        },
                        "virtualServer": "/NginxIngress/Nginx.Ingress/NginxIngress_vs",
                        "dependsOn": "none"
                    }
                ],
                "bpsLimit": 5,
                "bpsLimitEnabled": true,
                "ppsLimit": 4,
                "ppsLimitEnabled": true,
                "connectionsLimit": 3,
                "connectionsLimitEnabled": true,
                "maxAnswersReturned": 10,
                "monitors": [
                    {
                        "bigip": "/Common/http"
                    }
                ],
                "resourceRecordType": "A",
                "fallbackIP": "1.1.1.1"
            }
        }
    }
}
```
bigip-nginx [repo](https://github.com/mdditt2000/f5-appsvcs-extension/blob/master/user_guides/gslb/working-bigip-nginx.json)

