Yes. Since you've now given me the **complete Chapter 1 PPT material**, we can build a proper **exam question bank** before moving to the next chapter.

I would not limit it to direct-definition questions. For CS 606T, your professor can turn the same concepts into **theory, comparison, numerical, table/diagram, scenario, troubleshooting, and application-based questions**.

Below is a comprehensive question bank organized by type.

# CS 606T — Chapter 1 Question Bank

## Basic Computer Networks + Switching Techniques + VC + MPLS

---

# Part A — Basic Conceptual Questions

### Q1.

Why are point-to-point links alone insufficient for building a global network?

### Q2.

Why can't a single Ethernet network practically serve as the entire Internet?

### Q3.

What is internetworking?

### Q4.

What is the fundamental purpose of internetworking?

### Q5.

Differentiate between a switch and a router.

### Q6.

What is the primary function of a network switch?

### Q7.

What is meant by a multi-input, multi-output switching device?

### Q8.

Why are switched networks considered more scalable than shared-media networks?

### Q9.

What is aggregate throughput of a switch?

### Q10.

Why is high aggregate throughput an important switch-design goal?

### Q11.

How does a switch determine the output interface for an incoming packet?

### Q12.

What information does a switch/router generally inspect to make a forwarding decision?

### Q13.

What is network switching?

### Q14.

What are the two fundamental approaches to network switching discussed in the chapter?

### Q15.

Differentiate between connectionless and connection-oriented switching.

### Q16.

What is datagram switching?

### Q17.

Why is packet switching also called datagram switching?

### Q18.

What information must a datagram contain to enable forwarding?

### Q19.

What is a forwarding table?

### Q20.

Why is maintaining forwarding tables difficult in a large, dynamic network?

---

# Part B — Packet Switching

### Q21.

Explain the working principle of packet switching.

### Q22.

What does **store-and-forward** mean?

### Q23.

Why is packet switching described as a best-effort service?

### Q24.

Why can two packets from the same source to the same destination follow different paths?

### Q25.

What happens to packet forwarding when the network topology changes?

### Q26.

What are the major advantages of packet switching?

### Q27.

What are the limitations of packet switching?

### Q28.

Why is packet switching suitable for bursty traffic?

### Q29.

Explain statistical multiplexing in packet switching.

### Q30.

Why can packet switching utilize bandwidth more efficiently than circuit switching?

### Q31.

What happens when several packets simultaneously require the same outgoing link?

### Q32.

Explain the relationship between packet switching and queuing delay.

---

# Part C — Circuit Switching

### Q33.

What is circuit switching?

### Q34.

Explain the basic operation of circuit switching.

### Q35.

Why is circuit switching called connection-oriented?

### Q36.

What happens during circuit establishment?

### Q37.

Why does circuit switching provide predictable performance?

### Q38.

What is the major disadvantage of circuit switching for bursty traffic?

### Q39.

Why can circuit switching waste network resources?

### Q40.

Compare circuit switching and packet switching.

### Q41.

Why is circuit switching suitable for applications requiring predictable delay?

### Q42.

What happens if a circuit-switched connection becomes unavailable?

---

# Part D — Packet vs Circuit Switching

### Q43.

Compare packet switching and circuit switching with respect to:

* setup
* bandwidth allocation
* delay
* queuing
* reliability
* resource utilization
* bursty traffic
* path selection

### Q44.

A user sends data continuously for several hours. Which switching approach may be suitable and why?

### Q45.

A user sends a few bytes, waits for 10 seconds, then sends another few bytes. Which switching technique is more resource-efficient and why?

### Q46.

Why does packet switching generally provide better statistical resource utilization?

### Q47.

Why can circuit switching provide more predictable delay?

### Q48.

Can packet switching provide guaranteed bandwidth? Explain in the context of the basic models discussed in class.

---

# Part E — Virtual Circuit Switching

This is one of the **highest-priority sections**.

### Q49.

What is virtual circuit switching?

### Q50.

Why is virtual circuit switching considered a hybrid of circuit and packet switching?

### Q51.

Explain the three phases of virtual circuit communication.

### Q52.

What happens during VC connection establishment?

### Q53.

What happens during the VC data-transfer phase?

### Q54.

What is a Virtual Circuit Identifier (VCI)?

### Q55.

Why are VCIs locally significant?

### Q56.

Why does a VC not require the same VCI throughout the entire network?

### Q57.

What information is stored in a VC table?

### Q58.

Explain the meaning of:

$$
(Incoming\ Interface, Incoming\ VCI)
\rightarrow
(Outgoing\ Interface, Outgoing\ VCI)
$$

### Q59.

How does a switch use the VC table to forward a packet?

### Q60.

Why does the outgoing VCI potentially differ from the incoming VCI?

### Q61.

Explain VCI translation with an example.

### Q62.

Why does VC switching require a connection setup phase?

### Q63.

What is the setup delay associated with virtual circuits?

### Q64.

Why is approximately one RTT required before data transmission?

### Q65.

What happens if a VC path fails after the connection has been established?

### Q66.

Why does VC switching require failure recovery?

### Q67.

How do buffers affect VC switching?

### Q68.

Can VC switching experience queuing delay? Explain.

### Q69.

Why is VC switching considered more flexible than traditional circuit switching?

### Q70.

Why can VC switching provide better QoS than basic packet switching?

### Q71.

Why is VC QoS still potentially less predictable than circuit switching?

### Q72.

Why is VC lookup generally faster than IP forwarding in the simplified model discussed in class?

### Q73.

What is the difference between exact matching and longest-prefix matching?

### Q74.

Why does VC switching use exact matching?

### Q75.

What is the maximum number of VC table entries according to the simplified formula given in the lecture?

---

# Part F — VC Table Problems

### Q76.

A switch receives a packet through interface 2 with VCI 17.

Its VC table contains:

| Incoming Interface | Incoming VCI | Outgoing Interface | Outgoing VCI |
| -----------------: | -----------: | -----------------: | -----------: |
|                  1 |           10 |                  3 |           20 |
|                  2 |           17 |                  4 |           31 |
|                  3 |           25 |                  1 |           15 |

What should the switch do?

---

### Q77.

A packet arrives at interface 4 with VCI 25. The table contains:

| Incoming Interface | VCI | Outgoing Interface | Outgoing VCI |
| -----------------: | --: | -----------------: | -----------: |
|                  4 |  25 |                  2 |           11 |

Determine the outgoing interface and VCI.

---

### Q78.

Consider:

```text
A → S1 → S2 → S3 → B
```

VCIs are:

```text
A-S1 : 10
S1-S2: 25
S2-S3: 7
S3-B : 31
```

Explain how a packet's VCI changes at every switch.

---

### Q79.

Why is the following perfectly valid?

```text
Link 1 → VCI 10
Link 2 → VCI 10
```

---

### Q80.

A switch has 8 interfaces and each interface supports 256 VCIs. Using the formula from the lecture, calculate the maximum number of VC table entries.

---

# Part G — Numerical: Transmission Delay

### Q81.

A 5000-bit packet is transmitted over a 5 Mbps link. Calculate transmission delay.

### Q82.

A 20 KB packet is transmitted over a 10 Mbps link. Calculate transmission time.

### Q83.

A 10 MB file is transmitted over a 100 Mbps link. Calculate transmission delay.

### Q84.

A link has bandwidth 50 Mbps. How long does it take to transmit a 2 MB file?

### Q85.

A 12,000-bit packet takes 3 ms to transmit. Determine the link bandwidth.

---

# Part H — Propagation Delay

### Q86.

A 2000 km link has a propagation speed of \(2\times10^5\) km/s. Find propagation delay.

### Q87.

A signal travels 5000 km at \(2.5\times10^5\) km/s. Calculate propagation delay.

### Q88.

If the distance between two routers doubles, what happens to propagation delay?

### Q89.

Does increasing bandwidth reduce propagation delay? Explain.

### Q90.

Does increasing propagation speed reduce propagation delay? Explain.

---

# Part I — Four Types of Network Delay

### Q91.

Define processing delay.

### Q92.

Define queuing delay.

### Q93.

Define transmission delay.

### Q94.

Define propagation delay.

### Q95.

Write the formula for transmission delay.

### Q96.

Write the formula for propagation delay.

### Q97.

Write the total nodal delay formula.

$$
D_{total}=?
$$

### Q98.

Which delay depends on packet size and bandwidth?

### Q99.

Which delay depends on physical distance?

### Q100.

Which delay depends strongly on congestion?

### Q101.

Which delay occurs because a router must process a packet?

---

# Part J — Pipeline / Multiple Packet Numericals

### Q102.

Three packets each of 2000 bits travel through 3 hops. Each link operates at 2 Mbps. Ignore propagation and queuing delays. Find total time.

### Q103.

Five packets traverse four hops. Each packet requires 3 ms transmission time per link. Find total delivery time.

### Q104.

A message consists of 10 packets and crosses 6 links. Each packet requires 2 ms transmission time per link. Calculate the total time using pipeline transmission.

### Q105.

Why isn't the total delay simply:

$$
P\times N\times\frac LR
$$

for multiple packets crossing multiple hops?

### Q106.

Derive/explain:

$$
\boxed{(P+N-1)\frac LR}
$$

for \(P\) packets crossing \(N\) hops.

### Q107.

A 4-packet message crosses 5 links. Each packet has 1000 bits and every link has 1 Mbps capacity. Ignore other delays. Calculate the total time.

---

# Part K — Scenario-Based Questions

These are particularly useful because your professor already uses scenario questions.

### Q108.

A medical robotic system requires extremely predictable communication delay. Which switching approach would you consider and why?

### Q109.

An online shopping website experiences unpredictable traffic bursts. Which switching technique is more naturally suited and why?

### Q110.

A company wants a logical connection between two branches over shared infrastructure. Which switching approach from this chapter provides this concept?

### Q111.

A network carries voice traffic continuously for a long duration. Explain the relevant trade-offs between circuit, packet, and VC switching.

### Q112.

A network experiences heavy congestion and packets spend increasing amounts of time waiting in router buffers. Which type of delay is increasing?

### Q113.

A packet's physical transmission time remains constant but its end-to-end delay suddenly increases during peak traffic. Explain why.

### Q114.

A network administrator wants traffic to follow a predetermined route instead of the route normally selected by IP routing. Which technology from this chapter could help?

### Q115.

A company wants VPN services over a provider's shared infrastructure. Explain how MPLS can be relevant.

---

# Part L — MPLS Fundamentals

### Q116.

What does MPLS stand for?

### Q117.

What is the fundamental idea behind MPLS?

### Q118.

Why is MPLS considered related to virtual circuit switching?

### Q119.

What is an MPLS label?

### Q120.

Why are MPLS labels relatively short?

### Q121.

What does it mean that MPLS labels have local scope?

### Q122.

What is an LER?

### Q123.

What is an LSR?

### Q124.

Differentiate between LER and LSR.

### Q125.

What is Label Distribution Protocol?

### Q126.

Why is label distribution necessary?

---

# Part M — MPLS Label Distribution

### Q127.

Explain destination-based forwarding in MPLS.

### Q128.

How can a router associate labels with prefixes?

### Q129.

How are labels distributed between neighboring MPLS routers?

### Q130.

What information is conceptually exchanged during label distribution?

### Q131.

Explain the relationship:

$$
Prefix \rightarrow Label
$$

### Q132.

Why doesn't every MPLS router need to use the same label for a particular destination?

### Q133.

Explain the difference between:

$$
Destination\ Prefix
$$

and

$$
MPLS\ Label
$$

### Q134.

Does MPLS eliminate IP routing? Explain.

---

# Part N — MPLS Label Swapping

### Q135.

What is label swapping?

### Q136.

Explain label swapping using an example.

### Q137.

Consider:

```text
R1 → R2 → R3 → R4
```

R2 performs:

$$
10\rightarrow25
$$

and R3 performs:

$$
25\rightarrow40
$$

What label does the packet carry when it reaches R4?

---

### Q138.

Given:

| Router | Incoming Label | Outgoing Label |
| ------ | -------------: | -------------: |
| R2     |             15 |             22 |
| R3     |             22 |             31 |
| R4     |             31 |             45 |

A packet enters R2 with label 15. Trace its labels.

---

### Q139.

Why can the same label value be reused at different locations in an MPLS network?

### Q140.

Compare MPLS label swapping with VC switching VCI translation.

---

# Part O — Explicit Routing

### Q141.

What is explicit routing?

### Q142.

How does MPLS support explicit routing?

### Q143.

Why might a network administrator want to use an explicit route?

### Q144.

Normal routing selects:

```text
R1 → R2 → R4 → R7
```

but the administrator wants:

```text
R1 → R3 → R6 → R7
```

What MPLS feature is relevant?

### Q145.

How can explicit routing help with traffic engineering?

---

# Part P — MPLS + VPN

### Q146.

What is a VPN?

### Q147.

What is tunneling?

### Q148.

Does a VPN necessarily require a physically dedicated link?

### Q149.

How can MPLS support VPN services?

### Q150.

Why is MPLS useful for service-provider VPNs?

---

# Part Q — Comparison Questions

These are **very likely exam material** because they test whether you actually understand the differences.

### Q151.

Packet switching vs circuit switching.

### Q152.

Packet switching vs virtual circuit switching.

### Q153.

Circuit switching vs virtual circuit switching.

### Q154.

Virtual circuit switching vs MPLS.

### Q155.

VCI vs MPLS label.

### Q156.

VC table vs MPLS forwarding table.

### Q157.

IP forwarding vs MPLS forwarding.

### Q158.

Longest-prefix matching vs exact matching.

### Q159.

LER vs LSR.

### Q160.

Physical circuit vs virtual circuit.

---

# Part R — "Explain Why" Questions

These are excellent conceptual exam questions.

### Q161.

Why is packet switching more suitable for bursty traffic?

### Q162.

Why can circuit switching waste bandwidth?

### Q163.

Why can packet switching experience variable delay?

### Q164.

Why does VC switching require setup?

### Q165.

Why does VC switching have a setup delay of approximately one RTT?

### Q166.

Why are VCIs locally significant?

### Q167.

Why can VCI values change from one switch to another?

### Q168.

Why can VC switching use exact matching?

### Q169.

Why can VC lookup be faster than longest-prefix matching?

### Q170.

Why can VC switching still experience queuing delay?

### Q171.

Why does MPLS use labels?

### Q172.

Why can MPLS labels be changed at every router?

### Q173.

Why is label swapping useful?

### Q174.

Why can MPLS support explicit routing?

### Q175.

Why is MPLS useful for VPN services?

---

# Part S — True/False / Correct-the-Statement

### Q176.

Every packet in datagram switching follows the same path.

### Q177.

VCIs must be globally unique.

### Q178.

MPLS labels must be globally unique.

### Q179.

Increasing bandwidth directly reduces propagation delay.

### Q180.

Increasing physical distance increases propagation delay.

### Q181.

Queuing delay is generally associated with congestion.

### Q182.

Transmission delay depends on packet size and link bandwidth.

### Q183.

Propagation delay depends on packet size.

### Q184.

Circuit switching dynamically shares reserved resources with other users.

### Q185.

Packet switching requires a dedicated physical path before data transfer.

### Q186.

VC switching establishes a logical path before data transfer.

### Q187.

VC packets can experience queuing delay.

### Q188.

MPLS can use label swapping.

### Q189.

LER stands for Label Edge Router.

### Q190.

LSR stands for Label Switching Router.

### Q191.

MPLS cannot support explicit routing.

### Q192.

MPLS labels have local significance.

---

# Part T — Assertion/Reason Questions

### Q193.

**Assertion:** Packet switching is efficient for bursty traffic.

**Reason:** Packet-switched networks allow network resources to be statistically shared among users.

Determine whether both statements are true and whether the reason explains the assertion.

---

### Q194.

**Assertion:** Increasing bandwidth does not directly reduce propagation delay.

**Reason:** Propagation delay depends primarily on distance and propagation speed.

---

### Q195.

**Assertion:** VCIs do not need to be globally unique.

**Reason:** VCIs have local scope.

---

### Q196.

**Assertion:** MPLS can use different labels at different hops.

**Reason:** MPLS labels have local significance.

---

### Q197.

**Assertion:** VC switching can experience queuing delay.

**Reason:** VC switches can contain buffers where packets wait for outgoing links.

---

# Part U — Case Study Questions

### Q198.

A multinational company has unpredictable data traffic between its offices. It wants efficient sharing of network capacity.

Compare packet switching and circuit switching for this scenario.

---

### Q199.

A hospital requires highly predictable communication for a control system.

Compare the relevant characteristics of circuit and packet switching.

---

### Q200.

A service provider has multiple possible paths between two cities and wants traffic from one customer to follow a predefined path.

Explain how MPLS could help.

---

### Q201.

A packet enters an MPLS network with label 20.

At three consecutive routers:

```text
R1: 20 → 35
R2: 35 → 12
R3: 12 → 50
```

Trace the packet.

---

### Q202.

A VC connection has:

```text
A → S1 → S2 → S3 → B
```

with mappings:

```text
S1: 10 → 25
S2: 25 → 7
S3: 7 → 31
```

Explain the complete forwarding process.

---

# Part V — Difficult / Professor-Level Questions

These are the questions I'd especially practice after you've mastered the basics.

### Q203.

Explain why virtual circuit switching is not equivalent to traditional circuit switching even though both establish a connection before data transfer.

### Q204.

A VC has a fixed logical route but uses shared physical resources. Explain why this does not guarantee zero queuing delay.

### Q205.

Why does a VC switch need both incoming and outgoing VCIs instead of simply storing one globally unique connection identifier?

### Q206.

Explain how local VCI assignment improves scalability.

### Q207.

Compare the forwarding complexity of:

```text
IP → Longest Prefix Match
VC → Exact Match
MPLS → Label Lookup
```

### Q208.

Explain how MPLS combines ideas from connection-oriented and connectionless networking.

### Q209.

Explain the complete lifecycle of an MPLS packet from entering the MPLS network to leaving it.

### Q210.

Explain how MPLS can provide explicit routing even though conventional IP routing may choose a different path.

### Q211.

Explain why label swapping is analogous to VCI translation.

### Q212.

A network has multiple possible paths and different traffic classes. Explain conceptually how MPLS could be used for traffic engineering.

---

# Part W — Integrated Numerical + Conceptual Questions

These are particularly valuable for your exam preparation.

### Q213.

A 5 MB file is sent over a 10 Mbps circuit-switched link.

Given:

* Setup = 50 ms
* Distance = 2000 km
* Propagation speed = \(2\times10^5\) km/s

Calculate total delay assuming one propagation delay and one transmission delay.

---

### Q214.

A message is divided into 5 packets, each 4000 bits. It travels over 4 hops, each operating at 4 Mbps. Ignore propagation and queuing.

Calculate total delivery time using pipelining.

---

### Q215.

A packet-switched network has:

* Transmission delay = 5 ms
* Propagation delay = 10 ms
* Processing delay = 2 ms
* Queuing delay = 8 ms

Calculate total nodal delay.

---

### Q216.

A network changes from:

$$
10Mbps\rightarrow100Mbps
$$

What happens to transmission delay for a fixed-size packet?

What happens to propagation delay?

Explain.

---

### Q217.

A link's physical distance changes from 1000 km to 3000 km while bandwidth remains unchanged.

Determine which delay changes and by what factor.

---

# 🔥 The 15 Questions I Would Prioritize First

Don't try to memorize all 217 questions immediately.

For your **first exam practice round**, I'd start with these:

1. **Explain internetworking and why it is necessary.**
2. **Switch vs router.**
3. **Explain datagram/packet switching.**
4. **Explain circuit switching.**
5. **Packet vs circuit switching.**
6. **Explain VC switching and why it is a hybrid.**
7. **Explain VC setup and VC table creation.**
8. **Given a VC table, determine the outgoing interface and VCI.**
9. **Explain why VCIs have local scope.**
10. **Calculate transmission + propagation + setup delay.**
11. **Solve the multiple-packet/multiple-hop pipeline problem.**
12. **Explain MPLS, LER, and LSR.**
13. **Explain label distribution and prefix-to-label mapping.**
14. **Trace an MPLS packet through multiple label swaps.**
15. **Explain explicit routing and MPLS-based VPNs.**

### And one important point

For your exam, don't prepare these as **217 isolated answers**. The better approach is to practice them in rounds:

**Round 1:** conceptual + definitions
**Round 2:** numericals
**Round 3:** VC-table/MPLS-table tracing
**Round 4:** scenario-based questions
**Round 5:** difficult conceptual questions
**Round 6:** mixed mock exam

That will test whether you can **apply** the chapter rather than just recognize definitions.
