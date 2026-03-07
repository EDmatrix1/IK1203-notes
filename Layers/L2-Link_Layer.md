Link Layer
==========

Datagrams are now encapsulated into frames, which are sent with MAC addrs instead of IP addrs. Wireless and wired conns!

* flow control
    - pacing between adjacent sending and receiving nodes
* error detection + correction
    - receiver detects errors
    - receiver corrects bit error(s) without retransmission
    - has to request retransmit, or drops frame
* half-duplex and full-duplex
    - duplex means bidirectional comms
    - half-duplex means both can transmit, but not at the same time
* A host needs a NIC (network interface card or chip)
    - ETHernet, WiFi card or chip
    - imeplements link layer and physical layer
* combines hardware, software and firmware

* Error detection
    - parity checking (checking amount of 1's on a line, adds a parity bit so the line has even number of 1's)
    - 2D bit parity corrects detected bit errors
    - checksum (one's complement to add together segment content, adds checksum value to packet and sends. Receiver computes one's complement as well and compares it to the included checksum value)
    - not equal -> error
    - equal -> no error

Multiple access links + protocols
---------------------------------

* point-2-point
    - p-2-p link between ETH switch and host
    - PPP for dial-up access
* broadcast (shared wire/medium)
    - old-fashioned ETH
    - upstream HFC in cable access network
    - 802.11 WLAN, 4G/4G, satellite
* singe shared broadcast channel
* only one node can send at a time
    - collision if two or more send at the same time

_given:_ MAC of rate R b/s
_desiderata:_
1. When one node wants to transmit, it can send at rate R
2. When M nodes want to transmit, each can send at average rate R/M
3. fully decentralized:
    * no special node to coordinate transmissions
    * no synch needed
4. simple

Taxonomy - MAC protocols
------------------------
### three broad classes:
* channel partitioning
    - divide channel into smaller "pieces" (time slots, frequency, code)
    - allocate piece to node for exclusive use
    - TDMA, FDMA, WDMA, CDMA
* random access
    - channel not divided
        - collision could happen
        - "recover" from collision
    - ETH (IEEE 802.3), WiFi (IEEE 802.11)
* nodes take turns
    - nodes with more to send can take longer
* BT, IEEE 802.11 PCF (Point Coordination Function)

### TDMA - Time Division Multiple Access

* access to channel in "rounds"
* each station gets fixed time slot (packet transmission time) in each round
* unused slots go idle

### FDMA - Frequency Division Multiple Access

* channel divided into different frequency bands
* each station assigned fixed freq band
* unused transmission time in freq bands go idle

Random access protocols
------------------------

* When node has packet to send
    - transmit at full channel data rate R
    - no _a priori_ coordination among nodes
* two or more transmitting nodes: "collision"
* random access MAC protocol specifies:
    - how to detect collisions
    - how to recover from collisions
* ALOHA
* CSMA/CD
* CSMA/CA

### CSMA/CD (wired networks)
Simple CSMA: listen before transmit:
* _if channel sensed idle:_ transmit entire frame
* _if channel sensed busy:_ defer transmission

Don't interrupt others!

* collisions detected within short time
* colliding transmissions aborted - reducing channel wastage

* coll-det easier in wired LANs
    - measure signal strength
    - comapre transmitted and received signals
* difficult in wireless LANs
    - received signal much weaker than transmitted signal
    - receiver overwhelmed by local transmitter

The polite conversationalist

### CSMA/CA (wireless networks)
* In wireless networks:
    - Devices share the same radio channel
    - If two devices transmit at the same time, their signals collide
    - cannot raliably detect collisions while transmitting
    - avoidance

CSMA/CA steps:
    1. Listens to the channel before transmitting
    2. Wait a random time if the channel is busy
    3. When idle -> send message
    4. After sent, backoff timer to avoid future collisons
        - only counts down while channel is idle
    5. receiver sends back ACK when successful
        - if no ACK is sent to the source host, it assumes collision and retransmits later

* RTS/CTS
    - RTS (Request To Send)
    - CTS (Clear To Send)
    - Sender -> RTS -> Reciever
    - Receiver -> CTS -> Sender
    - Sender -> DATA -> Receiver
    - Receiver -> ACK -> Sender

The patient observer

MAC addr - Multiple Access Channel
-----------------------------------

Used locally to get frame from one interface to another physically-connected interface (same subnet, in IP-addressing sense)

* 48-bit MAC addr, hardware based, sometimes software settable
* each interface on LAN
    - has unique MAC addr
    - also has a locally unique IP addr
* MAC addr allocation administered by IEEE
* Manufacturers buys portion of MAC addr space to ensure uniqueness
- MAC addr: like Social Security Number/Personnummer
- Like postal addr
* Can move interface interface from one LAN to another
* IP addr not portable
    - depends on IP subnet to which node is attached

ARP - Address Resolution Protocol
----------------------------------

Used to map an IP addr to a MAC addr inside a LAN. It translates IP to MAC addr when sending data over the network.

* When IP is known
    - finds the MAC addr using ARP
* Broadcast requests floods LAN
    - Unicast resps
    - The device with the requested IP sends back its MAC addr along with its IP
* The sender computer saves info in cache
    - ARP table updates according to resp
    - no need to resend broadcast in the future for that IP
    - entries expire, TTL value
* Sending to device outside LAN
    - MAC to default gateway
    - ARP to find router MAC

ETH Switch
-----------

* L2 device: takes an _active_ role
    - store, forward ETH frames
    - examine incoming frame's MAC addr, _selectively_ forward frame to one-or-more outgoing links when frame is to be forwarded on segment, uses CSMA/CD to access segment

* transparent: hosts _unaware_ of presence of switches
* plug-n-play, self-learning
    - switches do not need to be configd

* hosts have dedicated, direct connection to switch
* switches buffer packets
* ETH protocol used on _each_ incoming link, so:
    - no collisions, full duplex
    - each link is its own collision domain

Four divices connected to the same switch (A - D), A sends data to B at the same time as C sends data to D. This will not cause any collisions. What cannot happen however, is A sending to B while C also sends to B.

* Switch table includes
    - MAC addr to host, interface to reach host, time stamp
    - similar to routing table

* Switch learns which hosts can be reached through which interfaces
    - when frame received, switch "learns" location of sender
    - records sender/location pair in switch table

* When frame received at switch:
    1. record incoming link, MAC addr of sender
    2. index switch table using MAC dest addr
    3. _If_ entry found for destination

        then {

            if (destination on segment from which arrived){

                drop frame

            } else {

                forward frame on interface indicated by entry

            }

        }

        else flood, forward on all interfaces except arriving interface.

* Switches can be conn together
    - works the same way