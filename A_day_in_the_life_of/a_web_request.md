# A day in the life of a web request

A host conn to a network wants to visit a website, but it only knows the domain name of the website.

### Step 1: Application layer

* Host uses the browser
    - user types in URL
    - browser wants to send req to domain
    - browser doesn't know where the domain is
    - contacts DNS (DNS lookup)

### Step 2: Network + Link Layer

* If the DNS server is on the local network
    - host floods network using ARP to find server's MAC addr
    - uses IP given by DHCP

* If the DNS server is not on the local network
    - sends frame to router over LAN
        - dest MAC: router
        - src MAC: host
        - DATA: IP packet
    - NAT
        - translates src IP -> public IP
        - send via new port num
        - saves translation pair in table
    - packet gets forwarded to final destination
        - path based on routing tables of routers
            - OSPF, RIP and/or BGP
            - encaps packet in new L2 frame
    - ICMP
        - send error if anything goes wrong:
            - TTL counts down every hop, until 0
            - destination unreachable
        - message back to sender (public IP)

## step 3: DNS process

* DNS receives domain name from host

> Case 1: cache hit

* DNS server knows the IP of the domain server
    - sends IP addr back to host

> Case 2: cache miss

1. DNS sends query to root server
    - root responds with TLD server
2. DNS sends query to TLD server
    - TLD returns auth server (IP) of the domain
3. DNS asks auth server
    - auth server returns IP of domain

## Step 4: DNS sends back domain IP

* reverse steps taken in step 2
    - server sends frame along the shortest path
    - dest IP is the hosts public IP (from router)
    - NAT translates the public IP -> host IP
    - NAT restores port num
    - router forwards to host over LAN

## Step 5: Host sends HTTP req to domain server

* browser can finally send its generated req
    - dest IP addr given by DNS
* The req is sent over TCP
    - threway handshake:
        1. Client -> SYN -> Server
        2. Server -> SYN-ACK -> Client
        3. Client -> ACK -> server
* same procedure as in step 2 (NAT, forwarding, ICMP)

## Step 6: Packet reaches web server

* Passes segment to OS
* reassembles data
* web server generates HTTP response:

    HTTP/1.1 200 OK

    Content-Type: text/html

* Sent back over TCP, reverse of step 2

## Step 7: browser renders page

* more resource reqs sent
    - same TCP conn
    - until all web page resources are retrieved

## step 8: DNS cache

* DNS caches the IP, HTTP headers and TCP conn