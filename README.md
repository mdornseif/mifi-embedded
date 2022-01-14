# mifi-embedded
Tools for make GL.inet MiFi Routers run unattended.

## Hardware

For germany there are two relevant hartware variants:

* EP06-E LA Modem - supports more bads and is more expensive.
* EC25-E FA Modem

Both seem to work reasonably well with Aldi-Talk (E+) and Lidl Connect (Vodaphone).
Insert SIM-Cards with the missing corner to the outside and the contacts facing up.

The for dots to the left are the battery indicator,

If the battery is empty  and you just connected power you can not switch on the MiFi for a few minutes.

To switch on press for 3s the power button.

Booting tends to be slowish. Sometimes up to 5m.

To switch off press the power button for 3s. This does work differently when the MiFi hasn't fully booted jet.

## Networking

You can access the router in the local Network via [https://console.gl-inet.com](https://console.gl-inet.com) or `ssh root@console.gl-inet.com`

## Goodcloud / VPN

[https://eu.goodcloud.xyz/#/device/list](https://eu.goodcloud.xyz/#/device/list) is a nice thing.

### Local Implementation

`gl_mqtt*` is the Implementation. `/usr/bin/gl_mqtt_cli` calls the application in `/www/api` for various stuff.  `/www/src/store/api.js` seems to be the main implementation.

## Various

Default Password is goodlive

## Basic Setup

```
ssh-copy-id root@console.gl-inet.com
cp .ssh/authorized_keys /etc/dropbear/authorized_keys
```

```
export NAME=mifi-929
uci set system.@system[0].hostname=${NAME}
uci commit system
uci set dhcp.@dnsmasq[0].rebind_protection=0
uci commit dhcp
/etc/init.d/boot restart
opkg update; opkg install iputils-arping tcpdump
```

## Add Zerotier

```
opkg update
opkg install zerotier
uci set zerotier.openwrt_network=zerotier
uci add_list zerotier.openwrt_network.join='0cccb752f7fa18c9'
uci set zerotier.openwrt_network.enabled='1'
uci commit zerotier
reboot
```

## See also

* https://github.com/mdornseif/raspberry-embedded
