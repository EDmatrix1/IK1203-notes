Transport Layer
===============

Internet transport protocols
=============================

TCP - Transport Control Protocol
---------------------------------

* TCP
    - reliable transport
    - flow control
    - congestion control
    - conn-oriented

TCP stands for _Transport Control Protocol_ and is SAFE compared to UDP. Before data sends, there is a handshake between sender and receiver:

    1. SYN (client reqs conn)
    2. SYN-ACK (server acknowledges req)
    3. ACK (client confirms conn)

The data is transfered in controlled segments that include ID, payload and control flags. It guarantees delivery through acknowledgements from the reciever. If an ACK has not been sent it means that something went wrong and the lost segment is resent by the sender. The ID ensures that the data is sent IN ORDER.

TCP makes sure the data does not overwhelm the receiver through flow and congestion control. The receiver tells the sender how much it can handle through CONGESTION WINDOWS.

Congestion control is done with three algorithms:

    1. Slow start
        - includes a threshold that TCP does not exceed regardless of the receivers cwnd being larger
        - cwnd doubles every RTT (round-trip time), exponential growth despite the name
        - stops when packet loss occurs or cwnd reaches ssthresh

    2. Congestion avoidance
        - Once close to the network capacity of the conn, it slows the growth to linear.
        - each RTT: cwnd = cwnd + 1 segment
        - In case of packet loss:
            1. ssthresh = cwnd / 2
            2. cwnd is reduced
            3. TCP enters Slow start or Fast retransmit

    3.   Fast retransmit
        - The reciever sends an ACK message for every message received. When a packet is lost, the receiver resends ACK for the last received segment.
        - 3 duplicate ACKs tells TCP that a packet has been lost and resends the missing packet, does not wait for timeout.

UDP - User Datagram Protocol
-----------------------------

* UDP
    - fast
    - unreliable transport
    - connectionless, no handshake

Data is sent without establishing a conn with the receiver in packets called datagrams (UDP version of TCP segments).

Unlike TCP, UDP does not guarantee delivery, order or duplication protection. This implies that all of these may occur when using UDP. Despite this, UDP has its use-cases and is implemented by a lot of L5 protocols.

When speed is more important than reliable data, the applications can tolerate some packet loss and/or they have their own reliability mechanisms.

Speed is highly valued in the real applications, which makes UDP much better than TCP. Low latency is prefered over useless data. Think video and audio calls. Resending a datagram might not be as relevant when something can be shown or said again in a case of bad conn.

* Can detect and drop flipped bits, cannot report
* uses checksum

Example calculations with TCP:
------------------------------

Q1:
A client establishes a TCP connection to a server to transfer 48 kB of data. The one-way delay is 3 ms and the advertized receiver window is 16 kB. Assume an initial congestion window of 2 kB. There is no congestion in the network and the transmission time is negligible. The time it takes to establish a connection should be considered.

Calculate the total transfer time.

A1:
One-way delay of 3 ms means the RTT is 6 ms.

In Slow start the sent information increases per RTT as follows:

2 -> 4 -> 8 -> 16 (ssthresh) => 30 kB has been sent

In Congestion avoidance:

Keeps sending with 16 kB as thresh since receiver window is 16 kB. To send the last 18 kB, two RTTs are needed.

6 RTTs in total needed
6 ms/RTT
9 ms threeway-handshake to establish conn

6 RTT * 6 ms/RTT + 9 ms = 45 ms

42 ms is also an acceptable answer since only SYN + SYN ACK are needed to start transferring data.


Q2:
A sends a 2000-byte packet to B over a link that is 4000 meter long. The total transfer time, from that A sends the first bit until B receives the last bit, is 40 microseconds.

What is the bitrate of the link, assuming that the propagation speed over the link is 2x10^8 m/s?

A2:
The total time it takes for the transfer is misleading. It includes both the transmission delay and the propagation delay, which we need to subtract.

T_pd = d / V = 4000 m / 2x10^8 m/s = 2x10^-5 s

2x10^-5 s => 20 ms

T_td = 40 ms - 20 ms = 20 ms

D / T_td = 2000 * 8 / 20 ms = 8x10^9 b/s

8x10^9 b/s => 800 Mb/s

Good equations to have in mind:

D / T = B_r

S / V = t

D - data, convert to bytes (B)

T - time, usually in ms, RTT never OWD (s)

B_r - Bitrate, capacity, how much data can be sent over the wire (b/s or bits/s or B/s)


S - distance, wire length (m)

V - speed, through the wire (m/s)

t - time, propagation delay usually (s)