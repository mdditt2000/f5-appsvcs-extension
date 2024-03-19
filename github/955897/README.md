## Question

When the virtual address is already defined, the **tmsh command** that is used to create virtual server should use the name of the virtual address. But when using the AS3 configuration, the **tmsh command** that the AS3 is forming when creating the virtual server is not using the name.
 
## AS3 scenario:
* tmsh create ltm virtual-address test address any%100
* tmsh create ltm virtual test destination any%100:0 
 
## Working scenario with tmsh commands sent to bigip:
* tmsh create ltm virtual-address test address any%100
* tmsh create ltm virtual test destination test:0  >>> Virtual address name is used since it is already defined.
 
AS3 should translate the virtual address name while creating the tmsh command which is not happening currently

## Findings

1) Posting the declaration AS3 creates the following VirtualServer

![vs](https://github.com/mdditt2000/f5-appsvcs-extension/blob/master/github/955897/diagram/2024-03-19_10-42-53.png)