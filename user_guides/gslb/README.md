# Create GSLB Configuration using AS3

AS3 can automate BIG-IP environments that use a combined GTM + LTM with virtual server discovery to create an GSLB_Pool object which populates the WideIP. In this example the the WideIP and the LTM objects are in the same declaration. This is preferred as the WideIP and the LTM objects are configured using a application tenant and not the common partition. 

* Trying to refer to a discovered ltm virtual has some timing problems. It can take several seconds (10?) for GSLB to discover LTM virtuals and allow them to be used in a GTM Pool

There are several configuration scenarios here:
* LTM virtual and GTM server are created in one declaration and referred to in another declaration in a GTM pool (GitHub issue 578). We can fix this by adding a delay after creating GTM servers that have virtual-server-discovery enabled.
* LTM virtual, GTM server, and GTM pool are all in one declaration. This is also fixed by adding a delay after GTM server creation.
* GTM server is created in one declaration (or without AS3) but LTM virtual and GTM pool are in another declaration (GitHub issue 588). In this case, if there is a use pointer to the GTM server, we can detect that virtual-server-discovery is enabled and only add a delay before creating the GTM pool in that case. In the case of a BIG-IP pointer to the GTM server, we would need to always add a delay.

In AS3-40 we added a solution to add a 10-15 second delay before creating GTM pools if there is a use that points to a GTM server with virtual discovery and always add the delay if it is a BIG-IP pointer or string

### Step 1: Create Data Center and GTM Server using AS3

```

```



