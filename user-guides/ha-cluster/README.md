## Using Declarative Onboarding to Configure BIG-IP HA

This document and video demonstrates how to use Declarative Onboarding to Configure BIG-IP HA. The following cluster configuration tips should be considered:

* When setting up a cluster, it is, from our experience, best practice to use fresh and clean BIG-IPs (e.g. brand new VMs).. In my example both devices are licensed with management IP Address configured
* Both the deviceGroup and the trust must be identical between the two declarations
* When I POST’d my **bigip1.json** to my device1, it responded pretty quickly with a “success”. **bigip2.json** took a few minutes longer
* Please note that since **external-self** is the floating address only one address will be configured on BIG-IP. Therefore you could remove external-self from the declaration or provide a different address as in my case

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

* Should the declaration not work as expected, review the declaration and confirm all machines on the cluster can reach the "remoteHost" FQDN/IP address. DNS or other network limitations can often prove an issue on newer network setups.

If the declaration appears correct, yet you are still receiving failures there are two methods to proceed.

1. The most effective method is to get a new machine (this is most easily done in a virtual environment), and try again with the new declaration. If that still continues to fail on the first attempt it may be something further within the declaration that is causing the problem.

2. The less effective method to "reset" a BIG-IP, do the following on whichever is failing:

Clear out the DO config (to prevent subsequent DO runs from having inaccurate information to work from):

* Send a GET to https://host/mgmt/shared/declarative-onboarding/config
Pull the id value from the response back
Send a DELETE to https://host/mgmt/shared/declarative-onboarding/config/id_value
You should receive a "[]" as the response if the DELETE was successful
A GET to https://host/mgmt/shared/declarative-onboarding/config should give the same result if the DELETE was successful
2b. Reset the BIG-IP to factory default (Note: this, by design, clears out ALL configuration on the BIG-IP. YOU HAVE BEEN WARNED):

With root privilege run (tmsh load sys config default)
2c. After the machine(s) are available, you should be able to POST the declarations again


