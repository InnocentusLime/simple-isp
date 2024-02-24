## The Topology

![](img/topology.png)

Each service is hosted on its own machine. The heart of the infrastructure is the "general server".

## IP addresses

The IP addresses are allocated in the ISP net as follows:
* genserver:
    - 10.206.0.1 in the client segment
    - 10.10.10.206 in the BGP network
    - 192.168.168.1 in the internal network
* DB:
    - 192.168.168.2 in the internal network
* DNS:
    - 10.206.206.206 in the client segment
* Website:
    - 192.168.168.3 in the inernal network
    - 10.206.0.2 in the client network

## Terminology

For more convinient scripting, the interfaces are renamed as follows:

| Node | Default Name | New Name |
| --- | --- | --- |
| genserver | eth0 | global |
| Website | eth0 | global |
| DNS | eth0 | global |
| genserver | eth1 | dbcom |
| DB | eth0 | gen |
| DB | eth1 | sitecom |
| Website | eth1 | dbcom |
| genserver | eth3 | private |
| genserver | eth2 | bgpcom |

Segment names:
* `10.0.0.0/8` -- global segment
* `10.206.0.0/16` -- client segment
* `192.168.168.0/24` -- internal segment
* `172.172.0.0/16` -- private segment
* `10.10.10.0/24` -- BGP segment

## Filtering Rules

Rules for general server (in increase of priority):
* misc:
    1. All routes are banned
    2. Only echo request, echo response and "destination unreacheable" ICMP messages are allowed
    3. TCP/UDP ports 0-1024 are banned from being both source and destination
* global interface:
    3. Source IPs can only be from client segment
    4. Destination IPs can only be from global segment
    5. Destination IPs can't be from BGP segment
* private interface:
    7. Source IPs can only be from the private segment
    8. Destination IPs can only be from the global segment
    9. Destination IPs cannot be from the BGP segment
* DBcom interfaces:
    12. Source and destination IPs can only be from the internal segment
    13. Source and destination IP can only be the DB ip
    14. The only allowed protocol is TCP. The only allowed TCP destination
    port is the one used for DB connections.
* bgpcom interfaces:
    15. Source and destination IPs can only be from the Global and BGP segment
    16. Source IPs can not belong to client segment
    17. Source and destination ports can be 179, but for that both source and IP addressed must
    be from BGP segment