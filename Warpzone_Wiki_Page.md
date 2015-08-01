# Introduction #

Warpzone is a tunneling framework tool written in python. It allows you to send data over several protocols to a designated endpoint. Common applications would be to establish IP traffic or exfiltrate data from networks that filter or significantly regulate traffic.

# Dependencies #

At a minimum, Warpzone requires the following in order.

  1. Linux (I'm using Linux Mint, but any Distro with /dev/net/tun should work)
  1. Python 2.7.3
  1. Python's Package Manager (pip)
  1. Twisted 14.0.0
  1. Service-Identity (optional, but required to get rid of an annoying warning message)

## Installing Python and pip ##

Installing Python and pip are OS-specific. However, on most Linux systems with apt-get, you can do: `sudo apt-get install python python-pip`

## Installing Twisted ##

If your system already has the Twisted framework installed, check which version as follows:
```
>>> import twisted
>>> print twisted.version
```

To upgrade:
`sudo pip install --upgrade twisted`

To install from scratch:
`sudo pip install twisted`

## Service Identity ##

Twisted complains if you do not have Service Identity installed. You can install it with the following command.

`sudo pip install service_identity`

# Usage #
Warpzone requires both a client and server. In a typical scenario, the client is able to connect to the server that is situated outside of a DMZ or is internet routable.

At a minimum, the client and server must agree on the interface and transport layer, including the transport layer's relevant settings.

## Interfaces ##
Warpzone reads data from the user through the following interfaces:
  * Standard I/O - Similar to netcat
  * TUN/TAP interface - Creates a network interface on both machines, similar to VPN
  * SOCKS 5 (Planned)
  * HTTP Proxy (Planned)

## Transport Layers ##
Warpzone can transfer data over the following protocols:
  * TCP
  * UDP
  * HTTP
  * ICMP
  * DNS

## Examples ##
Commands to establish Standard I/O connection over TCP/8080, for server and client, respectively:
```
python warpzone.py -s -i stdio -t tcp -p 8080
python warpzone.py -c -i stdio -t tcp -p 8080 -e 198.51.100.1
```

Commands to establish IP Connection over UDP/8080, for server and client, respectively:
```
sudo python warpzone.py -s -i tun -t udp -p 8080
sudo python warpzone.py -c -i tun -t udp -p 8080 -e 198.51.100.2
```
Now set the IP addresses on the server and client, respectively:
```
sudo ifconfig warp0 10.1.1.1/24
sudo ifconfig warp0 10.1.1.2/24
```

### Additional Options ###
  * Throttling (-w [TIME](TIME.md)) - Some protocols are asynchronous, so the client must periodically poll the server for new information.
  * 