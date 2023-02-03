## Using Declarative Onboarding to Configure BIG-IP HA

Demo on YouTube [video](https://youtu.be/eLG35MPIjfk)

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

* Should the declaration not work as expected, review the declaration and confirm all machines on the cluster can reach the "remoteHost" FQDN/IP address. DNS or other network limitations can often prove an issue on newer network setups.

If the declaration appears correct, yet you are still receiving failures there are two methods to proceed

1. The most effective method is to get a new machine (this is most easily done in a virtual environment), and try again with the new declaration. If that still continues to fail on the first attempt it may be something further within the declaration that is causing the problem.

2. The less effective method to "reset" a BIG-IP, do the following on whichever is failing:

Clear out the DO config (to prevent subsequent DO runs from having inaccurate information to work from):

* Send a GET to https://host/mgmt/shared/declarative-onboarding/config

![get](https://github.com/mdditt2000/f5-declarative-onboarding/blob/main/user-guides/ha-cluster/diagram/2023-02-01_15-40-48.png)

* Pull the id value from the response back. Send a DELETE to https://host/mgmt/shared/declarative-onboarding/config/id_value

![ID](https://github.com/mdditt2000/f5-declarative-onboarding/blob/main/user-guides/ha-cluster/diagram/2023-02-01_15-41-45.png)

You should receive a "[]" as the response if the DELETE was successful

* A GET to https://host/mgmt/shared/declarative-onboarding/config should give the same result if the DELETE was successful

![ID](https://github.com/mdditt2000/f5-declarative-onboarding/blob/main/user-guides/ha-cluster/diagram/2023-02-01_15-42-29.png)

2b. Reset the BIG-IP to factory default (Note: this, by design, clears out ALL configuration on the BIG-IP. **YOU HAVE BEEN WARNED** 

```
[root@localhost:Active:Standalone] config # tmsh load sys config default
Reset the system configuration to factory defaults? (y/n) y

Loading system configuration...
  /defaults/asm_base.conf
  /defaults/config_base.conf
  /defaults/ipfix_ie_base.conf
  /defaults/ipfix_ie_f5base.conf
  /defaults/low_profile_base.conf
  /defaults/low_security_base.conf
  /defaults/policy_base.conf
  /defaults/analytics_base.conf
  /defaults/apm_base.conf
  /defaults/apm_oauth_base.conf
  /defaults/apm_pua_ssh_base.conf
  /defaults/apm_saml_base.conf
  /defaults/app_template_base.conf
  /defaults/classification_base.conf
  /var/libdata/dpi/conf/classification_update.conf
  /defaults/ips_base.conf
  /var/libdata/ips/ips_update.conf
  /defaults/daemon.conf
  /defaults/pem_base.conf
  /defaults/profile_base.conf
  /defaults/sandbox_base.conf
  /defaults/security_base.conf
  /defaults/urldb_base.conf
  /usr/share/monitors/base_monitors.conf
  /defaults/cipher.conf
  /defaults/ilx_base.conf
  /usr/local/gtm/include/gtm_base_region_isp.conf
  /usr/share/monitors/gtm_base_monitors.conf
Loading configuration...
  /defaults/defaults.scf
Expiring passwords...
Resetting trust domain...
Setting flag to reset ASM data...
Setting flag to reset Nsyncd data...
Setting flag to reset Live Update data...
[root@localhost:Active:Standalone] config #
[root@localhost:Active:Standalone] config # tmsh
root@(localhost)(cfg-sync Standalone)(Active)(/Common)(tmos)# modify auth user admin prompt-for-password
changing password for admin
new password:
confirm password:
```

With root privilege run (tmsh load sys config default)

2c. After the machine(s) are available, you should be able to POST the declarations again


