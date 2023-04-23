# The Internet inter-AS routing: BGP

BGP (Border Gateway Protocol) is the de facto inter-domain routing protocol, 
which is "glue that holds the Internet together".


## Index

1. [EGP and BGP](#egp-and-bgp)
2. [IGP and BGP](#igp-and-bgp)


## 1. EGP and BGP

- https://www.tutorialspoint.com/what-is-exterior-gateway-protocol-egp
- https://www.techopedia.com/definition/6987/exterior-gateway-protocol-egp

Exterior Gateway Protocol (EGP) is an obsolete routing protocol that 
was used for data exchange between neighboring gateway hosts in autonomous 
systems. EGP was frequently used by research institutes, universities, 
government agencies and private organizations, but was replaced by Border 
Gateway Protocol (BGP)

Border Gateway Protocol (BGP) is the only one EGP used in modern TCP/IP. 
BGP is very important since it is used on the current Internet and 
other larger networks. The Exterior Gateway Protocol (EGP) is an 
obsolete protocol that was used for communication between non-core 
routers and the router core in the early Internet.
Therefore, the Internet currently uses a single interdomain routing 
protocol: the Border Gateway Protocol (BGP). The current version 
of BGP is defined in RFC 4271.

[Back to index](#index)


## 2. IGP and BGP

> **IGP handles all internal routes and BGP handles all global 
internet routes.**

For scalability and administrative autonomy, routers on the 
Internet are aggregated into autonomous systems (ASes) that are 
independently controlled by different organizations and companies. 

BGP was purpose-built for exchanging routes between autonomous systems. 
It is designed as a protocol that carries routes with next-hops. It 
does not carry topology information or information on how to reach 
those next-hops. It is the equivalent of someone saying "To get to the 
pneumatic store go to the March 4 Lane" without telling how to get to 
the March 4 Lane.

IGPs were not designed to handle the scale of routing information and 
their functioning would be negatively impacted by the large scale of 
the Internet routing table.

Interior Gateway Protocols (**IGP**s) such as RIP, OSPF and EIGRP 
are used to communicate routing information between routers 
within an AS. On the otherhand, Exterior Gateway Protocols 
(**EGP**s) are a set of routing protocols to send routing information 
between ASes. 
Thus infact, IGP means Interior Routing Protocol, and EGP – Exterior 
Routing Protocol. OSPF, IS-IS, RIP and EIGRP are IGP Protocols. 
**BGP** is the only available EGP Protocol. The original intent of 
protocol designers was to use IGP within the boundaries of a network 
and EGP outside of these boundaries.

The details of what happens within an AS are hidden from other 
ASes, which allows the administrator of an AS to have the independence 
to control the AS, including selection of one or more from a 
variety of different interior routing protocols. In contrast, to 
reliably connect ASes together, it is essential that each one be 
running the same exterior routing protocol. The result 
of this is that in TCP/IP there is generally only one exterior 
routing protocol in widespread use at a given time.

Border Gateway Protocol (BGP) is a standardized exterior gateway 
protocol designed to exchange routing and reachability information 
between autonomous systems (AS) on the Internet. This protocol is 
a distance-vector routing protocol.

IGPs are used to exchange routing information within an AS. In 
contrast, BGP is for determining network reachability between ASes 
and **makes use of IGPs to resolve routes within an AS**.

**Is BGP a dynamic routing protocol?**\
Example of dynamic routing protocols are BGP, EIGRP, OSPF and RIP 
that you can choose according to your topology, specific requirements 
( like scenario: WAN, Internet Edge, Data Center, SP networks), 
technical capabilities (vendor, type of devices, supported protocols) 
and so on.

**Is IGP an iBGP?**\
IBGP is TCP based protocol, and IGP is IP based protocols, so in 
order to get full connectivity with in AS we need IP based protocol.

**Is BGP a layer 3 protocol?**\
BGP in networking is based on TCP/IP. It operates on the OSI 
Transport Layer (Layer 4) to control the Network Layer (Layer 3).

**Why is BGP used?**\
Border Gateway Protocol (BGP) is used to Exchange routing information 
for the internet and is the protocol used between ISP which are 
different ASes. The protocol can connect together any internetwork 
of autonomous system using an arbitrary topology.

**What is AS routing?**\
AS routing is a collection of connected Internet Protocol (IP) 
routing prefixes under the control of one or more network 
operators on behalf of a single administrative entity or domain, 
that presents a common and clearly defined routing policy to 
the Internet.

**What is OSPF in networking?**\
OSPF is a link-state routing protocol that was developed for IP 
networks and is based on the Shortest Path First (SPF) algorithm. 
In an OSPF network, routers or systems within the same area 
maintain an identical link-state database that describes the 
topology of the area.

**Is BGP faster than OSPF?**\
OSPF has faster convergence times than BGP. Network convergence 
is the speed at which a router can adjust the path used to a 
destination network if a network outage occurs.

OSPF prefers fastest path rather than shortest path.
While BGP prefers best path.

**Why use iBGP over OSPF?**\
Each router uses BGP to speak to the outside networks. There is 
an iBGP mesh for each router to exchange the routes internally 
it knows about from each external network. The usual setup is to 
use OSPF to distribute the connected routes, as the routes via iBGP 
will still have the next hop set to their original value.

**为啥需要IBGP:**
\
路由量问题， IGP处理路由量有限， 路由量大的话就只能import进去。
IGP全网洪泛， A----B-----C， A要到C必须经过B的所有IGP设备。
而BGP, 只需要使能了B的设备学习就可以了。
BGP的路由控制能力也比IGP强,
IBGP防环， BGP路由注入到IGP中， 通过路由属性携带， 来防环。

**有了IBGP为啥还需要IGP：**\
IBGP是TCP连接， 不一定是直接物理相连， 所以需要IGP打通路由。
BGP不发现路由，更侧重路由的控制。

**IBGP和EBGP的区别**\
环路避免措施，EBGP靠路由属性AS-PATH， IBGP是缺省不发布到其他IBGP，
IBGP要求逻辑全连接，AS-PATH中包含as号相同则丢弃， AS-PATH越小，路径越优。
IBGP可以传递local-preference属性， EBGP不行。local-preference越大越优，
相同的话比router-ld小的优
IBGP需要路由同步,EBGP不需要
IBGP邻居间不需要物理连接， 而EBGP一般需要。

Why use iBGP inside an Autonomous System, if IGP protocols fulfill 
the need for internal communication?

why we need iBGP for the routes when we have the IGP protocols 
(OSPF, RIP) for internal communication within the AS?

- Scalability1: Imagine that you're receiving 500,000 EBGP routes 
in more than one location2, and you need to influence the per route 
exit point in your AS. BGP can handle many more routes than IGP 
protocols. Thus, iBGP is required unless you're willing to 
redistribute all the routes you've learned via eBGP

- Enforce boundaries of trust / control: BGP has more ways to filter 
peers than IGPs (for controlling what you advertise and receive).

- Flexible data structures (somewhat related to the previous bullet): 
BGP communities, BGP Extended communities, local-pref, etc... these 
make BGP an attractive way to implement custom routing policies 
within your own autonomous system (by using iBGP).

As with everything there are trade offs; the scalability, control, 
and flexibility you get from iBGP means that it's a slower 
converging protocol than IGPs (in general).




[Back to index](#index)

