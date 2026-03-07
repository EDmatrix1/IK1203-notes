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