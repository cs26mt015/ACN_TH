# CS 606T — Chapter 1 Exam Answers

These answers follow the question numbering in `questions.md`. Numerical answers use decimal networking units unless stated otherwise: 1 MB = 10^6 bytes and 1 KB = 10^3 bytes.

## How to use this in the exam

- For a **2-mark question**, write the definition and one key point from the numbered answer.
- For a **5-mark question**, write the definition, 3–4 bullet points, and a small diagram/table where suitable.
- For a **10-mark question**, use the detailed write-ups below, then add the relevant example or numerical from the numbered answers.
- Underline technical terms such as **setup phase**, **statistical multiplexing**, **VCI**, **label swapping**, and **longest-prefix matching**.

## Detailed long-answer material for theory questions

### 1. Internetworking and switching

**Internetworking** means connecting independent networks so that hosts on different networks can communicate. It is necessary because one LAN cannot cover the whole world: LAN technologies have limited distance, capacity, and administrative scope.

Write these points:

1. A direct point-to-point link between every pair of networks is not scalable; the number of links grows as `n(n−1)/2`.
2. Different organizations may use different LANs, link technologies, and addressing domains.
3. Routers connect these networks and forward packets from one network to another.
4. A router uses the destination IP address and a forwarding table to choose the next hop.
5. Therefore, internetworking creates one logical global communication system from many physical networks.

### 2. Packet/datagram switching

**Definition:** Packet switching breaks a message into packets. Each packet is stored, examined, and forwarded independently at each router.

Write these points:

1. No end-to-end connection or bandwidth reservation is required before transmission.
2. Every packet carries a destination address; routers use forwarding tables to select the next hop.
3. The method is called **store-and-forward** because a router receives a packet before transmitting it on the next link.
4. Links are shared by **statistical multiplexing**: inactive users consume no capacity, while active users share it.
5. It is efficient for bursty computer traffic and can route later packets around a failed link.
6. Its limitations are variable delay, queueing, loss under congestion, and possible out-of-order delivery.

### 3. Circuit switching

**Definition:** Circuit switching establishes a dedicated end-to-end communication path before data transfer and reserves resources on that path.

Write these points:

1. **Setup phase:** a route is selected and bandwidth/channel resources are reserved at every intermediate switch.
2. **Data-transfer phase:** the source sends along the established circuit with predictable capacity and low contention.
3. **Teardown phase:** the circuit is released and resources become available to other users.
4. Its main advantage is predictable bandwidth and delay, making it useful for continuous delay-sensitive traffic.
5. Its main disadvantage is poor utilization for bursty traffic, because reserved capacity remains unused during idle periods.
6. A failure on the circuit path interrupts the connection until a replacement circuit is established.

### 4. Packet switching versus circuit switching

| Point | Packet switching | Circuit switching |
|---|---|---|
| Connection setup | Not required | Required before transfer |
| Path | May change packet by packet | Fixed for the connection |
| Resource allocation | Shared dynamically | Reserved in advance |
| Delay | Variable due to queues | More predictable after setup |
| Best use | Bursty data traffic | Continuous, delay-sensitive traffic |
| Efficiency | High average utilization | Can waste idle reserved bandwidth |

**Conclusion:** Packet switching is usually preferred for data networks because traffic is bursty. Circuit switching is preferable when predictable service is more important than efficient sharing.

### 5. Virtual-circuit switching

**Definition:** Virtual-circuit switching creates a logical connection through a packet network before data is sent. It combines circuit-like connection setup with packet-based transmission.

Write the three phases:

1. **Connection setup:** the network selects a route and creates VC-table entries at each switch.
2. **Data transfer:** packets carry a **VCI (Virtual Circuit Identifier)**. A switch looks up the incoming interface and VCI, selects an outgoing interface, and replaces the VCI.
3. **Teardown:** the connection state is removed and allocated resources, if any, are released.

Use this forwarding rule:

`(incoming interface, incoming VCI) → (outgoing interface, outgoing VCI)`

Important points:

- A VCI has **local significance**, so it needs to be unique only on its local link/interface.
- Different links can use the same VCI value without conflict.
- VC forwarding uses **exact matching**, which is simpler than IP longest-prefix matching.
- A VC has a fixed logical path, but it can still experience queueing because physical links and buffers may be shared.
- It can offer better QoS control than basic datagram switching, but it is not automatically the same as a dedicated physical circuit.

### 6. Four types of delay

The total nodal delay is:

`d_nodal = d_processing + d_queueing + d_transmission + d_propagation`

1. **Processing delay:** time to inspect the packet, check errors, and determine forwarding action.
2. **Queueing delay:** time spent waiting in a buffer; it rises sharply during congestion.
3. **Transmission delay:** time to place all bits on the link. Formula: `d_trans = L/R`, where `L` is packet length in bits and `R` is bandwidth in bps.
4. **Propagation delay:** time for a signal to travel over the physical medium. Formula: `d_prop = distance/propagation speed`.

Remember: increasing bandwidth reduces **transmission delay**, whereas increasing distance increases **propagation delay**.

### 7. MPLS

**MPLS (Multiprotocol Label Switching)** forwards traffic through an MPLS domain using short labels rather than repeatedly making a full IP-prefix decision in the core.

Write these points:

1. An ingress **LER (Label Edge Router)** classifies an incoming IP packet and pushes an MPLS label.
2. A core **LSR (Label Switching Router)** looks up the incoming label in its forwarding table.
3. The LSR selects an outgoing interface and normally replaces the old label with a new one. This is **label swapping**.
4. At the egress LER, the label is removed and ordinary IP forwarding can continue.
5. Labels have **local significance**, so a label can change at every hop and may be reused elsewhere in the network.
6. Label bindings such as `prefix/FEC → label` are distributed between neighbors using a protocol such as **LDP**.
7. MPLS supports **traffic engineering/explicit routing** by binding traffic to a chosen label-switched path instead of only the default IP route.
8. MPLS also supports provider VPNs by keeping customer traffic in separate label/forwarding contexts over shared infrastructure.

Use this core forwarding rule:

`(incoming label, context) → (outgoing interface, outgoing label)`

## A. Basic concepts (Q1–Q20)

**Q1.** Point-to-point links alone do not scale to a global network. With `n` sites, a fully connected design needs `n(n−1)/2` links, which becomes costly and difficult to manage. Intermediate switching devices allow many networks to communicate without a direct link between every pair.

**Q2.** One Ethernet cannot serve the whole Internet because it has limited physical reach and bandwidth, and a single shared broadcast domain would cause excessive traffic, collisions/congestion, and poor fault isolation. The Internet must interconnect many independent networks.

**Q3.** Internetworking is the interconnection of separate networks so that devices on one network can communicate with devices on another network.

**Q4.** Its purpose is to provide end-to-end communication across heterogeneous, separately administered networks through routers and common protocols.

**Q5.** A switch normally forwards frames within a LAN using MAC addresses (Layer 2). A router forwards packets between different IP networks using destination IP addresses and routing/forwarding tables (Layer 3).

**Q6.** A network switch receives a frame on one interface and forwards it to the appropriate outgoing interface, allowing simultaneous communication among connected devices.

**Q7.** It means the device can accept traffic on several input ports and independently send it out through one or more output ports. Its switching fabric connects inputs to the required outputs.

**Q8.** Switched networks give hosts dedicated links to the switch and can support several simultaneous conversations. They avoid one large shared collision/broadcast medium and can be expanded by adding switches and links.

**Q9.** Aggregate throughput is the total rate at which a switch can forward traffic across all of its ports at the same time.

**Q10.** High aggregate throughput lets many ports operate concurrently with low blocking and delay. It prevents the switch fabric from becoming the bottleneck.

**Q11.** The device examines the forwarding information in the received frame/packet, looks it up in a forwarding table, and chooses the matching output interface.

**Q12.** A Layer-2 switch generally examines a destination MAC address. A router generally examines the destination IP address, usually applying longest-prefix matching.

**Q13.** Network switching is the process of receiving data at an input interface, selecting a path/output interface, and forwarding it toward its destination.

**Q14.** The two basic approaches are connectionless packet/datagram switching and connection-oriented switching, such as circuit switching or virtual-circuit switching.

**Q15.** Connectionless switching sends each packet independently, without prior setup; packets may take different paths. Connection-oriented switching first establishes a path/state, then forwards subsequent data using that connection.

**Q16.** Datagram switching is connectionless packet switching in which every packet carries a destination address and is forwarded independently.

**Q17.** It is called datagram switching because each packet is a self-contained datagram carrying the addressing information needed for forwarding.

**Q18.** A datagram must include a destination address, normally the destination IP address, so each router can make a forwarding decision.

**Q19.** A forwarding table maps a destination address or prefix (or another forwarding identifier) to an outgoing interface/next hop.

**Q20.** In a large dynamic network, links and routes can fail or change, destinations grow in number, and routing updates must keep forwarding tables correct and consistent.

## B. Packet switching (Q21–Q32)

**Q21.** Packet switching divides data into packets. Each router receives a packet, stores it briefly, examines its destination, and forwards it to the next hop. Network links are shared statistically among users.

**Q22.** Store-and-forward means a switch/router receives the complete packet before beginning to transmit that packet on the outgoing link.

**Q23.** It is best-effort because the network normally does not reserve bandwidth or guarantee delivery, delay, order, or a fixed path for every packet.

**Q24.** Each packet is forwarded independently using the forwarding table available at that time. Different congestion conditions, equal-cost routes, or routing changes can therefore select different paths.

**Q25.** Routing protocols update routes and forwarding tables. Subsequent packets can be sent along an available new path, while packets already in transit may be delayed or lost.

**Q26.** Advantages are efficient sharing of bandwidth, good support for bursty traffic, no setup delay, resilience through alternate paths, and scalability.

**Q27.** Limitations include variable delay and jitter, queueing and packet loss during congestion, possible out-of-order delivery, and no inherent bandwidth guarantee.

**Q28.** Bursty sources use capacity only when they have data. Idle sources consume no reserved capacity, so many users can share the same links efficiently.

**Q29.** Statistical multiplexing is dynamic sharing of a link among active packets. Capacity is assigned packet by packet according to demand rather than permanently reserved for one user.

**Q30.** Circuit switching reserves capacity even while a user is idle. Packet switching gives that capacity to other active packets, so average utilization is higher for variable traffic.

**Q31.** Packets contend for the same output link. One is transmitted; the others wait in a buffer, and packets may be dropped if the buffer fills.

**Q32.** Queueing delay is created when packet arrival rate temporarily exceeds the output-link service rate. Higher congestion produces longer queues and more variable end-to-end delay.

## C–D. Circuit switching and comparison (Q33–Q48)

**Q33.** Circuit switching establishes an end-to-end circuit before data transfer and reserves communication resources along the selected path for that connection.

**Q34.** It has setup, data-transfer, and teardown phases. During setup, each switch reserves a channel/bandwidth; during transfer the source uses the reserved path; teardown releases resources.

**Q35.** It is connection-oriented because communication cannot begin until a connection/circuit has been established.

**Q36.** Circuit establishment chooses a route and reserves the required resources on every link along that route.

**Q37.** Once capacity is reserved, competing traffic cannot use that reserved portion. Hence queueing is limited and bandwidth/delay are more predictable.

**Q38.** For bursty traffic, reserved resources remain unused during silent periods, wasting bandwidth.

**Q39.** A circuit holds its allocated bandwidth for its whole lifetime, whether or not it is carrying data.

**Q40.** Circuit switching reserves a fixed path/capacity after setup and offers predictable performance, but wastes idle capacity. Packet switching has no setup or fixed reservation, shares links efficiently, but has variable delay and congestion.

**Q41.** Reserved resources reduce contention and queueing, making delay and bandwidth more predictable for real-time continuous traffic.

**Q42.** The connection fails; data transfer stops until the network re-establishes a circuit, possibly via a different path.

**Q43.**

| Aspect | Packet switching | Circuit switching |
|---|---|---|
| Setup | None | Required before transfer |
| Bandwidth | Shared dynamically | Reserved end-to-end |
| Delay | Variable; includes queues | More predictable after setup |
| Queuing | Can be significant | Reduced for reserved traffic |
| Reliability | Can reroute later packets | Circuit failure breaks session |
| Utilization | High for bursty traffic | Idle reservation can waste capacity |
| Path | May vary per packet | Fixed for connection |

**Q44.** Circuit switching can suit continuous long-duration traffic because its reservation is used most of the time and gives predictable performance.

**Q45.** Packet switching is more efficient because no bandwidth is reserved during each 10-second idle interval.

**Q46.** It statistically multiplexes users: inactive sources release capacity to active sources instead of holding dedicated bandwidth.

**Q47.** A circuit reserves capacity and follows a fixed path, so it avoids most contention and variable queueing after setup.

**Q48.** Basic packet switching gives no inherent guaranteed bandwidth. Guarantees require additional reservation, admission-control, or QoS mechanisms beyond the basic best-effort model.

## E–F. Virtual circuits (Q49–Q80)

**Q49.** Virtual-circuit (VC) switching establishes a logical path through a packet network before transfer. Packets carry a short VC identifier rather than a full destination address at every hop.

**Q50.** It is hybrid because it has circuit-like setup and a fixed logical path, but packet-like store-and-forward transmission and shared physical links.

**Q51.** The phases are: (1) setup—create VC state/path, (2) data transfer—forward packets using VCIs, and (3) teardown—remove state and release any allocated resources.

**Q52.** Setup selects a path and installs a table entry at each switch mapping incoming interface/VCI to outgoing interface/VCI.

**Q53.** Each packet carries the VCI. At every switch, the matching VC-table entry selects the output and replaces the incoming VCI with the outgoing VCI.

**Q54.** A Virtual Circuit Identifier (VCI) is a short label that identifies a particular VC on a local link/interface.

**Q55.** VCIs are locally significant because they only need to be unique in their local interface/link context, not throughout the entire network.

**Q56.** Each switch translates its incoming VCI to the VCI chosen for the next link; therefore the same end-to-end VC may use different identifiers at different hops.

**Q57.** A VC table stores `(incoming interface, incoming VCI) → (outgoing interface, outgoing VCI)`, and may also include QoS/resource state.

**Q58.** It means that a packet arriving on a particular interface with a particular VCI is forwarded on the specified output interface after its VCI is changed to the specified outgoing value.

**Q59.** The switch exactly matches the packet's incoming interface and VCI against the VC table, swaps the VCI, and sends the packet on the stored output interface.

**Q60.** The outgoing VCI is allocated locally on the next link, independently of the incoming link's identifier space.

**Q61.** If a table entry is `(port 1, VCI 10) → (port 3, VCI 25)`, a packet arriving on port 1 labelled 10 leaves port 3 labelled 25. This replacement is VCI translation.

**Q62.** Setup is required so every switch can create the forwarding state and, where applicable, reserve/admit the required resources before packets arrive.

**Q63.** Setup delay is the time for a setup request to travel to the destination and for a confirmation to return; it is approximately one round-trip time (RTT), ignoring processing details.

**Q64.** A request must reach the far end and a reply/confirmation must return before the sender knows that the VC is ready, requiring roughly a forward and reverse propagation interval.

**Q65.** The VC's table state/path becomes unusable. Packets may be lost, and the connection must be repaired or a new VC established.

**Q66.** VC forwarding depends on preinstalled state along one logical path. A failure invalidates that state and cannot be handled simply by independent next-packet routing.

**Q67.** Buffers hold packets when their output link is busy. They absorb short bursts but add queueing delay and can overflow, causing loss.

**Q68.** Yes. A VC fixes logical forwarding state but normally shares physical links; if multiple VCs need the same output link, their packets can queue.

**Q69.** Unlike a traditional physical circuit, a VC uses shared packet infrastructure and can establish different logical paths/classes without dedicating an entire physical channel.

**Q70.** During setup, the network can allocate state/resources and apply traffic policies, allowing more controlled delay, bandwidth, and priority than plain best-effort datagrams.

**Q71.** Unless resources are strictly reserved and enforced, VCs still share buffers and links, so congestion may cause queueing and variable delay.

**Q72.** VC lookup is an exact match on a short local identifier, whereas IP forwarding commonly requires longest-prefix matching over variable-length prefixes.

**Q73.** Exact matching requires identical key values. Longest-prefix matching selects, among all matching IP prefixes, the most specific (longest) prefix.

**Q74.** The `(interface, VCI)` pair identifies one already-established local connection, so no hierarchy of prefixes has to be considered.

**Q75.** Using the lecture's simplified formula, maximum VC entries = number of interfaces × VCIs per interface.

**Q76.** Match `(interface 2, VCI 17)`. Forward through interface **4** and replace VCI **17** with **31**.

**Q77.** Forward through outgoing interface **2** with outgoing VCI **11**.

**Q78.** The packet starts with VCI 10 on A–S1. S1 changes `10 → 25`; S2 changes `25 → 7`; S3 changes `7 → 31`. It reaches B with VCI 31 on the final link.

**Q79.** It is valid because the two occurrences are on different links/local contexts. VCI 10 need not be globally unique.

**Q80.** Maximum entries = `8 × 256 = 2048` VC-table entries.

## G–J. Delays and pipeline numericals (Q81–Q107)

**Q81.** `d_trans = L/R = 5000/(5×10^6) = 0.001 s = 1 ms`.

**Q82.** `20 KB = 160,000 bits`. `d_trans = 160,000/(10×10^6) = 0.016 s = 16 ms`.

**Q83.** `10 MB = 80×10^6 bits`. `d_trans = 80×10^6/(100×10^6) = 0.8 s`.

**Q84.** `2 MB = 16×10^6 bits`. `d_trans = 16×10^6/(50×10^6) = 0.32 s = 320 ms`.

**Q85.** `R=L/d = 12,000/0.003 = 4,000,000 bps = 4 Mbps`.

**Q86.** `d_prop = distance/speed = 2000/(2×10^5) = 0.01 s = 10 ms`.

**Q87.** `5000/(2.5×10^5) = 0.02 s = 20 ms`.

**Q88.** Propagation delay doubles because it is directly proportional to distance.

**Q89.** No. Bandwidth affects transmission delay, not the signal's travel time over a fixed distance.

**Q90.** Yes. Since `d_prop = distance/propagation speed`, a higher speed lowers propagation delay.

**Q91.** Processing delay is the time a router takes to examine a packet, check for errors, and determine its forwarding action.

**Q92.** Queueing delay is the time a packet waits in an input/output buffer before it can be transmitted.

**Q93.** Transmission delay is the time needed to place all `L` packet bits onto a link of rate `R`: `L/R`.

**Q94.** Propagation delay is the time for a signal to travel across a physical link: `distance/propagation speed`.

**Q95.** `d_trans = L/R`.

**Q96.** `d_prop = d/s`, where `d` is distance and `s` is propagation speed.

**Q97.** `d_nodal = d_processing + d_queueing + d_transmission + d_propagation`.

**Q98.** Transmission delay depends on packet size and bandwidth.

**Q99.** Propagation delay depends on physical distance (and propagation speed).

**Q100.** Queueing delay depends most strongly on congestion.

**Q101.** Processing delay.

**Q102.** Per-link transmission time = `2000/(2×10^6)=1 ms`. Pipeline time = `(P+N−1)L/R = (3+3−1)(1) = 5 ms`.

**Q103.** `(5+4−1)×3 ms = 24 ms`.

**Q104.** `(10+6−1)×2 ms = 30 ms`.

**Q105.** Packets are pipelined: after the first packet advances to the next link, the following packet can use the previous link. Thus links work simultaneously rather than serializing every packet over every hop.

**Q106.** The first packet needs `N(L/R)` to cross `N` links. Each of the remaining `P−1` packets emerges one packet-transmission time later. Total = `[N + P − 1]L/R`.

**Q107.** `L/R = 1000/10^6 = 1 ms`. Total = `(4+5−1)×1 = 8 ms`.

## K. Scenarios (Q108–Q115)

**Q108.** Circuit switching, or a VC with strict resource reservation, is appropriate because predictable delay and bandwidth are required. Traditional circuit switching gives the strongest basic predictability but may be inefficient.

**Q109.** Packet switching is naturally suited because shopping traffic is bursty and links can be shared statistically among many active users.

**Q110.** Virtual-circuit switching provides a logical connection/path between the branches while using shared infrastructure.

**Q111.** Circuit switching gives predictable service but can waste capacity; packet switching uses capacity efficiently but gives variable delay; VC switching offers a fixed logical path and potential QoS control, while still sharing links and possibly queueing.

**Q112.** Queueing delay is increasing.

**Q113.** Increased peak-time congestion makes packets wait longer in router buffers. Transmission time is unchanged, but queueing delay raises end-to-end delay.

**Q114.** MPLS explicit routing/traffic engineering can establish a label-switched path along the administrator's chosen route.

**Q115.** MPLS can carry customer traffic over provider infrastructure using separate labels/forwarding state, enabling scalable VPN services and optionally traffic-engineered paths.

## L–P. MPLS and VPN (Q116–Q150)

**Q116.** MPLS stands for **Multiprotocol Label Switching**.

**Q117.** MPLS forwards packets in its core using short labels. The ingress edge assigns a label, intermediate routers use label tables, and labels are swapped at hops.

**Q118.** Like VC switching, MPLS uses short, locally significant identifiers, forwarding state, and per-hop identifier swapping along a logical path.

**Q119.** An MPLS label is a short forwarding identifier placed in an MPLS label header/stack to tell an MPLS router what forwarding action to take.

**Q120.** Labels are short fixed-size values, enabling simple efficient lookup compared with repeated full IP-prefix decisions.

**Q121.** Local scope means a label is meaningful only to the router/link forwarding context that assigned it; it need not be globally unique.

**Q122.** An LER is a **Label Edge Router** at the boundary of an MPLS domain. It classifies incoming traffic and commonly pushes labels, or removes labels as traffic exits.

**Q123.** An LSR is a **Label Switch Router**, normally inside the MPLS domain, that forwards packets by looking up and swapping labels.

**Q124.** An LER operates at the MPLS edge and imposes/removes labels; an LSR operates in the core and swaps/forwards labels.

**Q125.** LDP is the **Label Distribution Protocol**. Neighboring MPLS routers use it to distribute label bindings for forwarding equivalence classes/prefixes.

**Q126.** Distribution lets an upstream router learn which label to use when sending traffic to its downstream neighbor for a particular destination/FEC.

**Q127.** In destination-based MPLS forwarding, routing determines the next hop for a destination prefix and an MPLS label is associated with forwarding traffic toward that prefix.

**Q128.** The router uses its routing table to identify a reachable prefix/FEC, allocates a local label, and advertises the prefix-to-label binding to neighbors.

**Q129.** Neighboring MPLS routers exchange label bindings using a label distribution mechanism such as LDP.

**Q130.** Conceptually they exchange `destination prefix/FEC → label` bindings and related reachability/next-hop context.

**Q131.** `Prefix → Label` means traffic classified for that IP destination prefix is associated with the stated locally assigned MPLS label in the relevant forwarding context.

**Q132.** Labels are locally assigned. Each router may select a different label for forwarding toward the same destination and swaps it at the hop boundary.

**Q133.** A destination prefix is an IP-address range used for reachability and routing, such as `10.0.0.0/8`. An MPLS label is a short local forwarding identifier used after MPLS classification.

**Q134.** No. MPLS relies on/control-plane routing information to establish reachability and forwarding paths; it complements rather than eliminates IP routing.

**Q135.** Label swapping is replacing an incoming MPLS label with the outgoing label specified by the LSR's forwarding table and sending the packet on the selected interface.

**Q136.** If an LSR table maps incoming label 20 to output port 3 and label 35, it receives label 20, replaces it with 35, and sends the packet through port 3.

**Q137.** R2 changes `10 → 25`; R3 changes `25 → 40`. The packet reaches R4 carrying label **40**.

**Q138.** At R2: `15 → 22`; at R3: `22 → 31`; at R4: `31 → 45`. It leaves R4 with label **45** if R4's stated entry is applied.

**Q139.** The same number can be reused because its meaning depends on local label space and interface/context, not on global uniqueness.

**Q140.** Both operations map a local incoming identifier to an outgoing interface and a new local identifier. VC switching swaps VCIs; MPLS swaps labels.

**Q141.** Explicit routing is selecting a particular path for traffic rather than allowing ordinary destination-based routing alone to choose the path.

**Q142.** MPLS can create label-switched paths with specified hops/constraints and bind traffic to them through labels.

**Q143.** It can avoid congestion, use spare capacity, meet policy/QoS requirements, improve resilience, or balance traffic.

**Q144.** MPLS explicit routing, commonly used for traffic engineering, is the relevant feature.

**Q145.** It lets operators steer selected flows across underused paths, rather than concentrating all traffic on the shortest/default IP route.

**Q146.** A VPN is a logically private network/service built over a shared public or provider network, with traffic separation and controlled reachability.

**Q147.** Tunneling encapsulates one packet/protocol inside another so it can traverse an intermediate network as a logical tunnel.

**Q148.** No. A VPN is normally logical and can share physical provider infrastructure with other customers.

**Q149.** MPLS supports VPNs by maintaining customer-specific label/forwarding context so provider routers can carry separated customer traffic across the same backbone.

**Q150.** It scales better than dedicated links, shares backbone capacity, separates customer routes, and can combine VPN service with QoS and traffic engineering.

## Q. Comparisons (Q151–Q160)

**Q151.** Packet switching is connectionless, uses full destination addresses and dynamic sharing, and has variable delay. Circuit switching requires setup and reserves fixed resources/path, providing predictable service but wasting idle bandwidth.

**Q152.** Packet switching makes an independent IP decision for every packet; no setup is required and paths can vary. VC switching first installs a logical path, then uses short VCIs and exact lookup; it has setup state and can support more controlled QoS.

**Q153.** Both establish a connection before data. Circuit switching reserves dedicated link resources; VC switching establishes logical state over shared packet links, so it is more flexible but may still queue.

**Q154.** Both use local identifiers and per-hop swapping. VC switching is a general connection-oriented packet-switching model; MPLS is a practical label-based architecture that works with IP routing and supports traffic engineering/VPNs.

**Q155.** A VCI identifies a virtual circuit on a local interface. An MPLS label identifies a forwarding equivalence class/action in a local MPLS context. Both are locally significant and can change per hop.

**Q156.** A VC table maps `(incoming interface, VCI)` to `(outgoing interface, VCI)` for a connection. An MPLS forwarding table maps incoming label/context to an outgoing label and interface for a label-switched forwarding action.

**Q157.** IP forwarding usually uses destination IP and longest-prefix matching. MPLS core forwarding uses an incoming label lookup and performs a label operation (swap, push, or pop) plus output selection.

**Q158.** Longest-prefix matching finds the most specific matching IP network prefix. Exact matching requires the complete lookup key to match exactly, as for a VCI/label entry.

**Q159.** An LER is at the MPLS edge and adds/removes labels; an LSR is a label-switching router in the MPLS core that forwards using labels.

**Q160.** A physical circuit reserves actual transmission resources for a connection. A virtual circuit is a logical connection/state over shared physical links.

## R–T. Explain-why, true/false, and assertion/reason (Q161–Q197)

**Q161.** Packet switching is suitable for bursty traffic because idle users do not reserve capacity; active packets share links statistically.

**Q162.** A circuit's reserved bandwidth remains unavailable to others even while its user sends nothing.

**Q163.** Packets compete for shared links and buffers, so changing load causes changing queueing delays and sometimes different routes.

**Q164.** VC setup installs per-VC forwarding state and, if required, allocates/admission-controls resources before data transfer.

**Q165.** The setup signal must travel to the destination and acknowledgment/confirmation must return, requiring approximately an RTT.

**Q166.** A VCI is only used to distinguish connections on a local interface/link, allowing reuse elsewhere.

**Q167.** Each outgoing link has its own local VCI allocation; switches translate one link's identifier to the next link's identifier.

**Q168.** A VC has preinstalled state, so its `(interface, VCI)` pair is a complete unique lookup key.

**Q169.** Exact matching on a short identifier is simpler than selecting the longest matching prefix from multiple IP prefixes.

**Q170.** A fixed logical VC path can still share an output link/buffer with other VCs; contention causes queues.

**Q171.** Labels allow efficient forwarding actions, engineered paths, and service classification without full destination-prefix lookup at every MPLS core hop.

**Q172.** MPLS labels have only local significance, so every LSR may independently map its incoming label to a locally valid outgoing label.

**Q173.** Swapping permits local label allocation/reuse while preserving an end-to-end label-switched forwarding path.

**Q174.** MPLS can bind a flow to a deliberately created label-switched path rather than only the default IP-selected path.

**Q175.** MPLS provides scalable, separated forwarding contexts for customers across a shared provider backbone, with optional QoS/traffic engineering.

**Q176.** **False.** Datagram packets may follow different paths.

**Q177.** **False.** VCIs have local scope.

**Q178.** **False.** MPLS labels have local significance.

**Q179.** **False.** Bandwidth affects transmission delay, not propagation delay.

**Q180.** **True.** With unchanged propagation speed, greater distance increases propagation delay.

**Q181.** **True.** Congestion creates queues and increases queueing delay.

**Q182.** **True.** `d_trans = L/R`.

**Q183.** **False.** Propagation delay depends on distance and propagation speed, not packet size.

**Q184.** **False.** Circuit resources are reserved, not dynamically shared while the circuit exists.

**Q185.** **False.** Packet switching does not require a dedicated path before transfer.

**Q186.** **True.** VC switching establishes a logical path/state first.

**Q187.** **True.** Shared output links/buffers can cause VC packets to queue.

**Q188.** **True.** MPLS performs label swapping.

**Q189.** **True.** LER means Label Edge Router.

**Q190.** **True.** LSR means Label Switch(ing) Router.

**Q191.** **False.** MPLS can support explicit routing and traffic engineering.

**Q192.** **True.** MPLS labels are locally significant.

**Q193.** Both assertion and reason are true, and the reason correctly explains the assertion: statistical sharing makes packet switching efficient for bursty traffic.

**Q194.** Both are true, and the reason correctly explains the assertion: propagation delay is governed by distance and propagation speed, not link bandwidth.

**Q195.** Both are true, and the reason correctly explains the assertion: local scope means global uniqueness is unnecessary.

**Q196.** Both are true, and the reason correctly explains the assertion: local label significance allows labels to differ at each hop.

**Q197.** Both are true, and the reason correctly explains the assertion: VC switch buffers can hold packets awaiting an output link.

## U–W. Case studies and advanced answers (Q198–Q217)

**Q198.** For unpredictable multinational-office traffic, packet switching is preferable because it statistically shares capacity and does not waste bandwidth when offices are idle. Circuit switching gives predictability, but permanently reserved capacity is inefficient unless traffic is continuously high and delay-sensitive.

**Q199.** A hospital control system needs predictable, low delay. Circuit switching provides the clearest basic guarantee through reserved resources; packet switching is more efficient but has variable queueing delay. A resource-controlled VC/MPLS service may be a practical compromise if QoS is enforced.

**Q200.** The provider can establish an MPLS label-switched path through the specified cities/routers and bind that customer's traffic to it. Label swapping forwards traffic along that chosen route, supporting traffic engineering.

**Q201.** The label trace is `20 → 35 → 12 → 50`. R1 swaps 20 for 35, R2 swaps 35 for 12, and R3 swaps 12 for 50; the packet then carries label 50.

**Q202.** The VC trace is: S1 maps `10 → 25`, S2 maps `25 → 7`, and S3 maps `7 → 31`. Thus the packet travels A–S1 with 10, S1–S2 with 25, S2–S3 with 7, and S3–B with 31.

**Q203.** VC switching is not traditional circuit switching because a VC creates logical forwarding state over shared packet links, not necessarily a dedicated physical channel. It may therefore buffer, queue, and have variable delay unless explicit resource guarantees exist.

**Q204.** A fixed route decides where packets go, not when they are served. Other VCs can require the same physical output link; packets then wait in shared buffers, creating queueing delay.

**Q205.** A switch needs both values because each VCI is only locally meaningful. The incoming VCI identifies the arriving connection, while the outgoing VCI is the identifier allocated for the next link; the mapping enables per-hop translation and reuse.

**Q206.** Local assignment allows the same small VCI values to be reused on many unrelated links. This reduces identifier-space requirements and avoids global coordination for every VC.

**Q207.** IP forwarding performs longest-prefix matching over destination prefixes, making lookup comparatively complex. VC forwarding uses exact match on `(interface, VCI)`. MPLS uses a direct label lookup (often exact) and applies a stored output/label operation, so VC/MPLS core forwarding is simpler in this model.

**Q208.** MPLS is connection-oriented in that it maintains label-switched forwarding state and can use established paths; it retains packet-network flexibility because it works over shared IP infrastructure, supports multiple paths, and can be built using routing information.

**Q209.** At ingress, an LER classifies an IP packet and pushes a label. Each core LSR looks up the incoming label, swaps it (or performs push/pop) and forwards it. At egress, an LER removes the label as appropriate and forwards the original packet toward its destination.

**Q210.** An operator establishes a label-switched path with the desired hops/constraints and assigns traffic to it. Core LSRs follow label state for that path, so the traffic need not follow the ordinary IP shortest/default route.

**Q211.** Both replace a locally meaningful incoming identifier with a locally meaningful outgoing identifier while selecting the next interface: MPLS swaps labels; VC switches translate VCIs.

**Q212.** MPLS can define different label-switched paths for traffic classes, sending delay-sensitive traffic on a low-latency path and bulk traffic on a less congested or lower-cost path. This balances load, uses capacity efficiently, and can meet policy/QoS goals.

**Q213.** Transmission delay: `5 MB × 8 / 10 Mbps = 4 s`. Propagation delay: `2000/(2×10^5) = 0.01 s = 10 ms`. Total = `setup + transmission + propagation = 0.05 + 4 + 0.01 = 4.06 s` (**4060 ms**).

**Q214.** Per-packet transmission time is `4000/(4×10^6)=1 ms`. Pipeline delivery time = `(P+N−1)L/R = (5+4−1)(1 ms) = 8 ms`.

**Q215.** Total nodal delay = `5 + 10 + 2 + 8 = 25 ms`.

**Q216.** For a fixed-size packet, increasing bandwidth from 10 Mbps to 100 Mbps makes transmission delay one-tenth of its original value. Propagation delay does not change because distance and propagation speed are unchanged.

**Q217.** Propagation delay changes because distance changes from 1000 km to 3000 km: it becomes **three times** its original value. Transmission delay remains unchanged because packet size and bandwidth are unchanged.
