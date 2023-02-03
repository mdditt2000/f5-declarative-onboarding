## Using Declarative Onboarding to Configure BIG-IP HA

This document and video demonstrates how to use Declarative Onboarding to Configure BIG-IP HA. The following should be considered:

* Fresh devices are best. In my example both devices are licensed with management IP Address configured
* Both the deviceGroup and the trust must be identical between the two declarations
* When I POST’d my **bigip1.json** to my device1, it responded pretty quickly with a “success”. **bigip2.json** took a few minutes longer

Please note that since **external-self** is the floating address only one address will be configured on BIG-IP. Therefore you could remove external-self from the declaration

```
"external-self": {
    "class": "SelfIp",
    "address": "10.192.75.62/24",
    "vlan": "external",
    "allowService": "none",
    "trafficGroup": "traffic-group-1"
},
```

Demo on YouTube [video](https://youtu.be/eLG35MPIjfk)