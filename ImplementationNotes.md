# Implementation Notes

## Edge Router/VLAN Configuration

| Port Group Name | VLAN ID | Subnet           |
|-----------------|---------|------------------|
| vm-network-136  | 136     | 192.168.136.0/24 |
| vm-network-137  | 137     | 192.168.137.0/24 |

The VLANs need to be trunked together.

In the outer port group for each VLAN associated with this install (136 and 137), edit the settings to
accept promiscuous mode and forged transmits.

Make sure that DNS wil listen to the VLANs!

## DNS Entries

| Name                                          | Address         |
|-----------------------------------------------|-----------------|
| vcsa.tanzubasic.tanzuathome.net               | 192.168.136.112 |
| tanzu-basic-esxi-1.tanzubasic.tanzuathome.net | 192.168.136.113 |
| tanzu-basic-esxi-2.tanzubasic.tanzuathome.net | 192.168.136.114 |
| tanzu-basic-esxi-3.tanzubasic.tanzuathome.net | 192.168.136.115 |
| haproxy.tanzubasic.tanzuathome.net            | 192.168.136.116 |

## VM Setup

Increased the size of the disk and memory of the nested ESXI VMs. As it stood, there was very little room left in the VSAN.

## HAProxy Load Balancer Range

William's script has the load balancer range overlapping the gateway on the workload netowrk. This caused issues in my setup.

I changed the range to 192.168.137.64/26 (192.168.137.64-192.168.137.127)

## HAProxy CA Cert

You will need the CA cert from HA proxy. Here's how to get it:

- `ssh root@haproxy.tanzubasic.tanzuathome.net`
- `cat /etc/haproxy/ca.crt`

If you have rebuilt the envirnment, you might get an error from SSH about the host key changing. If that happens, remove the prior key like this:

`ssh-keygen -R haproxy.tanzubasic.tanzuathome.net`

## Mac Gatekeeper

When you download the VCSA ISO, it will be blocked from running by Mac Gatekeeper. To turn this off:

`sudo xattr -r -d com.apple.quarantine VMware-VCSA-all-7.0.1-17491101`


## Other

There will be a warning about JSON being truncated while running the script - you can ignore this warning.

## Install Times

1. Initial running of the script: 35-45 minutes
1. Sync the content library: about 30 minutes
