A day in the life of a host seeking connection
===============================================

## The flow

A host that has just recently connected to a network does not yet have an IP addr, nor does it know anything about the network. To get this information it looks for a DHCP server.

* The DORA process
    - Client broadcasts DHCP Discover to the network
    - The server(s) hearing the Discover message, resps with a DHCP Offer
    - The client sends a DHCP Request to the server it deems suitable
    - The server resps with a DHCP ACK to confirm the conn
    - DHCP sends a multitude of info:
        - IP addr
        - subnet mask
        - default gateway (IP)
        - DNS server addr (IP)
* The host and server send info using _UDP_

* Before the host can send info outside its LAN
    - needs MAC addr to default gateway
        1. floods network using _ARP_
        2. Asks for MAC addr to the IP given by DHCP
        3. The router resps with MAC addr

Wired networks
--------------

Uses collision _detection_ when ARP:ing the local network.

> Waits when busy

> Sends when idle

Wireless networks
-----------------

Uses collison _avoidance_ when ARP:ing the local network. Can use RTS/CTS handshake.

> listens to network

- waits when busy
- sends when idle
- backoff timer

> RTS/CTS handshake

1. Device -> RTS -> Router
2. Router -> CTS -> Device
3. Device -> DATA -> Router
4. Router -> ACK -> Device