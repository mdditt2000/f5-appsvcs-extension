# Factors that affected API performance:

* The size of the API task

This can be defined as how much you can cram into a single transaction before it takes too long to complete or times out. For AS3, this is defined as the number of virtual servers and accompanying objects (profiles, etc) you can create with a single tenant declaration before it takes too long to complete or times out

Overall config size / as3 tenant size (object count, we can use # of virtual servers to quantify object count in our testing). Recommendation is to keep the number of applications in one tenant to a minimum as shown in the diagram and documented [AS3 Best Practices](https://clouddocs.f5.com/products/extensions/f5-appsvcs-extension/latest/userguide/best-practices.html)

* API performance problems manifest themselves as timeouts or unreasonably long time to complete a task (subjective). Increase timeout values if the REST API is timing out and increase the **restjavad memory allocation**

[Postman Collection](https://github.com/mdditt2000/f5-appsvcs-extension/blob/master/user_guides/api-performance/AS3%20Validation.postman_collection.json)