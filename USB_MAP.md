# Mapping USB ports

Here's the USB port map for the Gigabyte AORUS Z390 MASTER that the original author discovered using [USBMap script](https://github.com/corpnewt/USBMap).

 * **HS01/SS01:** Front USB-C
 * **HS02/SS02:** Unknown
 * **HS03/SS03:** Rear USB 3.1 (red, second from Ethernet)
 * **HS04/SS04:** Rear USB 3.1 (red, next to Ethernet)
 * **HS05/SS05:** Rear USB 3.1 (red, next to USB-C)
 * **HS06/SS06:** Rear USB-C
 * **HS07/SS07:** Rear USB 3.0 (yellow) - next to HDMI
 * **HS08/SS08:** Rear USB 3.0 (yellow)
 * **HS09/SS09:** Front USB 3.0 #1
 * **HS10/SS10:** Front USB 3.0 #2
 * **HS11:** Rear USB 2 (all ports)
 * **HS12:** Internal USB2
 * **HS13:** Internal USB2
 * **HS14:** Bluetooth/Wifi
 * **USR1:** Unknown
 * **USR2:** Unknown

## Ports kept enabled:

 * HS03/SS03
 * HS04/SS04
 * HS05/SS05
 * HS06/SS06
 * HS09/SS09
 * HS10/SS10
 * HS11
 * HS12
 * HS13

**Total:** 15 ports

Disabled:

 * both yellow USB3 ports next to the HDMI port
 * the Front USB-C header
 * HS2 (unknown)
 * BT/WiFi
 * USR1/USR2 (unknown)

## USB.plist

The author included the [`USB.plist`](assets/USB.plist) file generated with USBMap. This is specific to the Gigabyte AORUS Z390 MASTER board and will NOT work with similar boards (such as the Elite or Pro). Each port is named to make things a bit easier.

In order to use the USB map, just copy the included `USB.plist` to `USBMap/Scripts`. By using it, you will not spend time discovering ports.
