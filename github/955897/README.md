## Findings

1) Posting the declaration AS3 creates the following VirtualServer

![vs](https://github.com/mdditt2000/f5-appsvcs-extension/blob/master/github/955897/diagram/2024-03-19_10-42-53.png)

AS3 pulls the VirtualServer name from the description

```
  "Tenant": {
            "class": "Tenant",
            "app_shared_0": {
                "class": "Application",
                "allow_0.0.0.0_0_pkadbap": {  ----------- VirtualServer name
```

## Question

1) What is incorrect about the VirtualServer?

Customer can define the VirtualServer name