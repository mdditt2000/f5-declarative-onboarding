## Behavioral Changes coming to Declarative Onboarding 1.36

This document and video demonstrates the behavioral changes coming to Declarative Onboarding 1.36. The default value for 'allowService' on a 'SelfIp' will change from 'default' to 'none'

Changes from **default** to **none** which is consistent with BIG-IP UI

```
"internal-self": {
    "class": "SelfIp",
    "address": "192.168.200.61/24",
    "vlan": "internal",
    "trafficGroup": "traffic-group-local-only",
    "allowService": "Allow Default"
},
```
After D0 1.36

```
"internal-self": {
    "class": "SelfIp",
    "address": "192.168.200.61/24",
    "vlan": "internal",
    "trafficGroup": "traffic-group-local-only",
    "allowService": "none"
},
```

Demo on YouTube [video](https://youtu.be/_0DiUfOr6u0)