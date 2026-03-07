Application layer
==================

Social networking, e-mail, messaging, voice over IP, video calls, searching in browser, multiplayer games, streaming, cloud, crypto
& P2P filesharing.

An application definition from the perspective of a network:
-------------------------------------------------------------

* running on multiple end-systems
* communicates over a network/networks
    - End-2-end comms
    - End-2-server comms

* Network-core devices
    - routing devices connecting networks & hosts
    - does not run user applications
    - routers, switches, etc

Client-server paradigm
-----------------------

Servers are always-on hosts, stored at well-known locations with _PERMANENT_ IP addrs. Data centers or other use-cases.

Clients are end-users that _initiates_ comms with a server. Conns with a server comes and goes. Usually has dynamic IP given by "local" DHCP servers, leased. Browsers, mail, etc.

Peer-peer architecture
----------------------

* no server comms
    - end-systems directly comm

* peers request service from other peers, provides service in return to other peers
* peers are intermittently conn and change IP addrs.
    - complex management

Many-many Comm
--------------

Client process: initates comms
Server process: awaits comms

* Many client processes on same host, comms with diifferent server processes
* A server process comms with many client processes

* A process must have an _identifier_ to recieve messages
* each host has a unique IP addr
    - IP is not enough, hence the use of _port numbers_ to associate process from host

* servers use pre-def port numbers
* clients can use any port numbers
    - must be free to use

Appli-layer (L5) protocol defines:
----------------------------------

* types of messages
    - request, response
* message syntax
    - what fields are in a message
* message semantics
    - meaning of information in the field's order
* rules for when and how reqs and resps are sent
*Such protocols are called RFCs
    - HTTP
    - SMTP
    - DNS
    - FTP
    - DHCP

Application layer protocols
============================

HTTP - Hypertext Transfer Protocol
----------------------------------

The most well-known protocol, HTTP is used to transfer web content between client and server. The client requests a resource, a website, through a browser or app and the server provides the web page image, or whatever else the client has requested.

* Resources
    - URLs specify what is requested
* req-resp based
    - client wants, client gets
* methods
    - GET
    - POST
    - PUT
    - DELETE
    - HEAD

* Can return status codes, 3 digits long
    - 1XX (Informational)
    - 2XX (Success)
    - 3XX (Redirection)
    - 4XX (Client error)
    - 5XX (Server error)

* HTTPS for secure web page fetching

SMTP - Simple Mail Transfer Protocol
------------------------------------
SMTP is design to send emails from a client to an outgoing mail server and relay these emails between themselves and other mail servers.

* Client opens TCP conn to server
* Server responds
    - HELO command
* Specify FROM, TO and DATA/BODY
* QUIT conn
* SMTPS for security agianst eavesdropping

* other protocols are used by receipient to read sent emails
    - POP3
    - IMAP

* POP3
    - Download email locally from server, deleting it from the server
    - POP3S for security
* IMAP
    - View emails from the server, not stored locally
    IMAPS for security
* Both rely on TCP

DNS - Domain Name System
------------------------

* Humans are bad at numbers, text is easier
    - Names are given to domains (web pages)
* computers are bad a text, numbers are easier
    - every domain has an IP that the computer reads
* DNS translates names to IP addrs
    - or IP's to names in reverse lookup

* Clients ask for an IP addr by giving the DN
* DNS servers store domain info and searches for the DN
    - Root servers hold TLD servers
    - TLD servers hold domains like .com or .org
    - Authoritative servers contain the sought-after IP the client wants

* DNS records store information
    - A -> IPv4
    - AAAA -> IPv6
    - CNAME -> alias of another domain
    - MX -> mail server for domain
    - NS -> Nameserver for domain
    - PTR -> reverse mapping from IP to DN

* DNS uses UDP as default
    - larger responses or zone transfers use TCP

* Caches results from client requests
    - locally and through ISP resolvers
    - records have TTL values

DHCP - Dynamic Host Configuration Protocol
------------------------------------------

Automatically leases IP addrs to hosts along with giving hosts a plethora of other useful info from the DHCP server.

* DHCP gives info
    - IP addr
    - Subnet mask
    - Default gateway
    - DNS server

* DHCP steps
    1. DHCP Discover
        - Client _broadcasts a message_ to 255.255.255.255
        - wants to find DHCP server
    2. DHCP offer
        - DHCP server responds to client
        - gives info
        - sent to client's MAC addr
    3. DHCP Request
        - Client accepts an offer given by a DHCP server
    4. DHCP ACK
        - server confirms the lease
    - called DORA process
* given info is _temporary_
* renewed periodically
* prevents IP conflict
* Uses UDP for comms

Transport service needed in protocols
-------------------------------------

* data integrity
    - reliable data transfer
* timing
    - low delays
* security
    - encryption