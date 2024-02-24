## What is this

This is a simple project of mine where I tried to approximate what an ISP does in their network.
It is probaly much less sophisticated than what is used in the real world and not that scalable,
but it helped me learn quite a bit about the way networks work.

ISP features:
- [ ] The ISP can export its network and import others' networks
- [ ] The ISP has a website
- [ ] The ISP has a database where it stores its own clients
- [ ] The ISP has a DNS server
- [ ] The ISP is capable of limitting the traffic and speed of its clients
- [ ] The ISP has two types of clients: Companies (static IPs) and Users (dyanmic IPs + NAT)

Basic details about the network:
* The segment used for all the clients is `10.206.0.0/16`
* The BGP network is assumed to have prefix `10.10.10.0/24`
* ISP's BGP IP is `10.10.10.206`
* ISP's ASN is `65206`
* ISP's private network prefix is `10.20.0.0/16`

## A little file tour

In the `scripts/` folder you can find some setup routines I used for my machines to make the
network setup less repetetive. The `DESIGN.md` file contains the network topology plus some extra
details.