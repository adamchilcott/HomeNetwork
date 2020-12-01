# Home Network Configuration

![Network Diagram](/Diagrams/Exports/Network.svg)

This repository will hold configuration details for a relatively complex home network revolving around Ubiquiti EdgeMAX & TP-Link Products.

I would like to thank [maxslug](https://github.com/maxslug), [mjp66](https://github.com/mjp66) and [Dong Ngo](https://dongknows.com/about-dong-ngo).

## Inspiration

- https://github.com/mjp66/Ubiquiti
- https://github.com/maxslug/mikrotik_maxslug
- https://dongknows.com/tp-link-omada-class-diy-mesh-review

### Goals

- Purchase inexpensive, off the-shelf products that are easily available without compromising entirely on quality or features.
- Isolate devices to each VLAN with some exceptions. I do not want devices talking across VLANs.
- Force Guest and IoT devices to use specific DNS Servers.
- Keep the setup small, but available to expansion if required.
- Implement Firewall Zones.
- Have fun and learn a thing or two along the way!

### Equipment

- 1 x Ubiquiti EdgeRouter X (ERX)
- 1 x TP-Link TL-SG105PE V1
- 1 x TP-Link EAP225 V3

### VLANs

VLAN  |IP                |Usage
------|------------------|-----------------
 100  |192.168.100.0/24  |Staging
 110  |192.168.110.0/24  |Management
 120  |192.168.120.0/24  |LAN
 130  |192.168.130.0/24  |Guest/IoT
 140  |192.168.140.0/24  |VoIP
 150  |192.168.150.0/24  |DMZ

### Firewall Zones

There are a number of Firewall Zones:

- WAN - ISP/Outside Traffic.
- LOCAL - Traffic inside EdgeRouter X.
- MGMT - Routers/Switches.
- LAN - PCs, Trusted Devices.
- Guest - SmartPhones, UnTrusted Devices.
- VoIP - IP Telephony.
- DMZ - Demilitarised Zone.
- BKP - Spare Port to Access ERX.

Staging VLAN has no Firewall Zone. It is a Sheol space with no Internet access.

### Wireless

- TP-Link EAP225 V3 in Standalone Mode. No Omada Controller. 
- Each 2.4Ghz SSID has an accompanying 5Ghz SSID of the same name and key. This allows Beam Forming feature to be enabled.

### DNS

- All devices on the Guest VLAN are forced to use [Quad9](https://www.quad9.net) DNS Servers.
- Some manual entries applied to `/etc/hosts` to resolve statically assigned local addresses:

```
127.0.0.1 localhost

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters

####################
# START MANUAL ENTRY
####################

192.168.10.1     ERX.abc.bkp.local
192.168.100.1    ERX.abc.stage.local
192.168.110.1    ERX.abc.mgmt.local
192.168.120.1    ERX.abc.lan.local
192.168.130.1    ERX.abc.guest.local

192.168.110.2    TL-SG105PE.abc.mgmt.local

##################
# END MANUAL ENTRY
##################

192.168.120.1    MYPC.abc.lan.local    #on-dhcp-event 0:1f:13:5a:bd:99
127.0.1.1        ERX     #vyatta entry
```

### Diagrams

- Rough layout of what is happening mapped out with [app.diagrams.net](https://app.diagrams.net)

### Files

- Two folders for each type of ERX configuration exported via the following commands:

      show configuration
      show configuration commands

- Splits are in the order the configuration should be applied to the ERX (I haven't tested this yet).
- If you want one long, continous configuration file you can concatenate the splits with your platform of choice.
- Screenshots of the TP-Link TL-SG105PE V1 Web Interface Settings.

### Other

- Please check the `TODO` Please fork/contribute!
- [EdgeRouter - Configuration and Operational Mode](https://help.ui.com/hc/en-us/articles/204960094)
- [EdgeRouter - Copy and Replace Configuration Sections](https://help.ui.com/hc/en-us/articles/205223490-EdgeRouter-Copy-and-Replace-Configuration-Sections)
