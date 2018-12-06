
# 0 Intro

For MAAS HA, there are mainly two topics:

- Rack controller HA
- Region controller HA

# 1 Rack Controller HA

Rank controller handles BMC control and DHCP.

## 1.1 Install Another Rack Controller

https://docs.maas.io/2.4/en/installconfig-rack#install-a-rack-controller

## 1.2 BMC control

HA for BMC control (node power cycling) is provided out of the box once a second rack controller is present.

## 1.3 DHCP

If DHCP is being enabled for the first time after a second rack controller is added then enable it according to Enabling DHCP.

However, if the initial rack controller already has DHCP enabled then a reconfiguration of DHCP is in order. Simply access the VLAN in question (via the 'Subnets' page) and choose action 'Reconfigure DHCP'. There you will see the second rack controller appearing in the 'Secondary controller' field. All you should have to do is press the 'Reconfigure DHCP' button.

# 2 Region Controller HA

Implementing region controller HA involves setting up:

- PostgreSQL HA
- Secondary API server(s)
- Virtual IP address

## 2.1 Postgres HA

MAAS stores all state information in the PostgreSQL database. It is therefore recommended to run it in HA mode.

Refer to this doc for more details to setup a second hot standby

https://docs.maas.io/2.4/en/manage-ha-postgresql

## 2.2 API Server HA

Details see here: https://docs.maas.io/2.4/en/manage-ha#secondary-api-server

Load balancing can be added with the HAProxy for API servers.

## 2.3 Virtual IP

A virtual IP (VIP) will be used as the effective IP address of all region API servers. This will be done with the aid of the Keepalived.

Details see: https://docs.maas.io/2.4/en/manage-ha#virtual-ip

For more information on the whole setup, see: https://docs.maas.io/2.4/en/manage-ha

# 3 Summary

As for first release, since we only have limited number of machines, we probably don't have enough resources to setup HA for neither region nor rack controller.

For the final production state, we definitely need them both.

