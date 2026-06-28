# Let's start by accessing the web interface

- I gave my firewall a different subnet for testing
- Managing it with a test vm in the same subnet

![](../../Images/opsense.png)

___
# Configuration wizard

For simplicity I'm using the configuration wizard for initial setup and anything more complex i'll do manually later

![](../../Images/OPNSense/1%20-%20Configuration%20Wizard.png)

- Next

![](../../Images/OPNSense/2%20-%20Fallback%20dns%20server.png)

- Here all I'm doing is setting a fallback DNS server in case Unbound resolver fails

- **Override DNS**: Leaving it checked means OPNsense pushes its own DNS to DHCP clients
- **Enable Resolver**: Unbound (local DNS resolver)


## WAN

![](../../Images/OPNSense/3%20-%20WAN.png)

###### Type DHCP

- Gets an ip from my router via **vmbr0** (that i configured earlier in the installation) since I'm using this firewall between my private network and a test VM

![](../../Images/OPNSense/4.png)

###### Block RFC1918 Private Networks

- I'd leave this check but again since my wan is actually a private IP from my home network i need to uncheck it


## LAN

![](../../Pngs/Pasted%20image%2020260628154258.png)

Comes pre-configured with OPNsense's IP and subnet

- Leaving DHCP on for tests later


## Deployment type

![](../../Images/OPNSense/6%20-%20Deployment%20type.png)

- **Optimize for Multiwan**: checked by default, no harm in a single-WAN setup

- **Automatic DHCP/DNS registration**: hostnames of DHCP clients get registered in Unbound automatically

- **Optimize for IPsec**: Leaving this unchecked since i'm not doing VPN tunnels here


## Password and finish

- Just set up a root password and finish the setup