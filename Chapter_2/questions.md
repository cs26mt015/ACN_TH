Absolutely. Since you've now completed **Router Architectures + Switching Fabrics**, this is a good point to do an **exam-oriented question bank** before starting the next chapter.

I'll make the questions harder than simple definitions and cover **theoretical, practical, scenario-based, numerical, analytical, comparison, and design questions**.

# CS 606T — Router Architectures & Switching Fabrics

## Comprehensive Exam Question Bank

---

# Part A — Core Theory Questions

### Q1.

What are the two fundamental functions performed by a router? Explain the difference between routing and packet forwarding.

### Q2.

Differentiate between a **routing table** and a **forwarding table**.

### Q3.

Why is the forwarding table generally implemented using specialized/high-speed hardware while routing decisions are associated with software/control-plane processing?

### Q4.

Explain the complete sequence of operations performed by a router when an IP packet arrives at an ingress interface.

### Q5.

What is the purpose of IP header validation at a router?

### Q6.

Explain TTL processing at a router. Why is TTL necessary?

### Q7.

Why does the router need to update/recompute checksum information after modifying the TTL?

### Q8.

What is route lookup? What information is obtained from a forwarding lookup?

### Q9.

What is packet fragmentation? Under what condition might a router need to fragment an IPv4 packet?

### Q10.

Explain packet classification and give three examples of fields that can be used for classification.

---

# Part B — Routing and Forwarding

### Q11.

Explain the relationship:

$$
Routing\ Algorithm
\rightarrow
Routing\ Table
\rightarrow
Forwarding\ Table
$$

### Q12.

Why can't the routing table simply be searched directly for every arriving packet?

### Q13.

A routing protocol detects that a link has failed. Explain what happens to the routing table and forwarding table.

### Q14.

Differentiate between:

* Routing
* Forwarding
* Packet classification
* Packet scheduling

### Q15.

Explain why forwarding must operate at a much higher rate than routing-protocol processing.

---

# Part C — Router Architecture

### Q16.

Explain the three major components of a router from the functional perspective:

1. Line cards
2. Switching fabric
3. Routing processor

### Q17.

Explain the role of a line card in a router.

### Q18.

What is the purpose of the switching fabric?

### Q19.

What is the role of the routing processor?

### Q20.

Explain the complete packet journey:

$$
Ingress\ Line\ Card
\rightarrow
Switching\ Fabric
\rightarrow
Egress\ Line\ Card
$$

### Q21.

Why does a line card need memory?

### Q22.

What happens if packets arrive at an ingress line card faster than the switching fabric can transfer them?

### Q23.

Why are buffers required at both input and output sides of a switching fabric?

---

# Part D — Linux PC-Based Router

### Q24.

A Linux PC is being used as a router. Explain the sequence:

$$
NIC
\rightarrow
PCI\ Bus
\rightarrow
Memory
\rightarrow
CPU
\rightarrow
Memory
\rightarrow
NIC
$$

### Q25.

Why does the PCI bus become a bottleneck in the Linux PC router architecture?

### Q26.

Why is the throughput of the example Linux PC router approximately half the PCI bus bandwidth?

### Q27.

How can the performance of a software-based Linux router be improved?

### Q28.

Why would using multiple CPU cores improve router performance?

### Q29.

Why would avoiding unnecessary packet copying improve throughput?

---

# Part E — Numerical Questions: Router Performance

### Q30.

A router has:

* PCI bus speed = 4 Gbps
* Packet header = 80 bytes
* CPU frequency = 2.5 GHz
* CPU instructions required = 250

Assume the packet must be copied from NIC → memory and memory → NIC.

Calculate:

1. NIC-to-memory transfer time
2. CPU processing time
3. Memory-to-NIC transfer time
4. Total processing time
5. Approximate throughput

---

### Q31.

A Linux-based router processes a 60-byte header.

PCI bus speed:

$$
3\text{ Gbps}
$$

CPU frequency:

$$
3\text{ GHz}
$$

Number of instructions:

$$
300
$$

Calculate the approximate total packet-processing time assuming two PCI transfers.

---

### Q32.

A router processes each packet in 500 ns.

Calculate:

1. Packets per second
2. Throughput if each packet is 100 bytes
3. Throughput in Gbps

---

### Q33.

A router has a processing time of 250 ns per packet.

How many packets can it process per second?

---

# Part F — Switching Fabrics

### Q34.

What is a switching fabric?

### Q35.

Why is the switching fabric sometimes called a switching backplane?

### Q36.

Explain the difference between:

* Shared backplane
* Switched backplane

### Q37.

Why does a shared backplane have limited throughput?

### Q38.

Why can a switched backplane achieve higher aggregate throughput?

### Q39.

What are the three important performance metrics of a switching fabric?

### Q40.

Explain:

* Throughput
* Latency
* Path diversity

in the context of switching fabrics.

---

# Part G — Crossbar Switch

### Q41.

What is an \(N\times N\) crossbar switch?

### Q42.

Why does an \(N\times N\) crossbar contain \(N^2\) crosspoints?

### Q43.

Calculate the number of crosspoints for:

a. \(4\times4\)
b. \(8\times8\)
c. \(16\times16\)
d. \(32\times32\)

---

### Q44.

Explain the function of a crosspoint.

### Q45.

What is the role of the centralized scheduler in a crossbar switch?

### Q46.

Why can two inputs not normally transmit to the same output simultaneously?

### Q47.

Why can a crossbar support multiple simultaneous transfers?

### Q48.

What happens when packets arrive at fixed intervals?

### Q49.

When would a crossbar need to calculate its schedule dynamically?

---

# Part H — Crossbar Scheduling

### Q50.

What is a matching in a crossbar switch?

### Q51.

Explain maximal matching and maximum matching.

### Q52.

Why is a maximum matching desirable?

### Q53.

Why might a scheduler use an approximate matching algorithm rather than computing the mathematically maximum matching?

### Q54.

Explain the concept of a timeslot in a switched backplane.

### Q55.

Why must the connections selected for a timeslot be conflict-free?

---

# Part I — Numerical Crossbar Questions

### Q56.

A \(4\times4\) crossbar operates for 5 timeslots.

At most 4 packets can be transferred per timeslot.

If 15 packets are transferred, calculate utilization.

---

### Q57.

An \(8\times8\) crossbar operates for 10 timeslots.

A maximum of 8 packets can be transmitted per slot.

Only 52 packets are actually transferred.

Calculate:

1. Maximum possible transfers
2. Actual transfers
3. Switch utilization
4. Number of unused transfer opportunities

---

### Q58.

A \(16\times16\) crossbar operates for 100 timeslots.

If utilization is 75%, calculate the number of packets transferred.

---

### Q59.

A \(32\times32\) crossbar has \(N^2\) crosspoints.

If each crosspoint requires one control bit, calculate the memory required to store the crosspoint state.

---

# Part J — HOL Blocking

### Q60.

What is Head-of-Line blocking?

### Q61.

Why does FIFO input queueing suffer from HOL blocking?

### Q62.

Explain HOL blocking using a diagram.

### Q63.

Consider:

```text
Input 1:
P1 → Output 1
P2 → Output 2
```

Output 1 is currently unavailable but Output 2 is free.

Explain what happens under FIFO input queueing.

---

### Q64.

Why does HOL blocking reduce crossbar utilization?

### Q65.

Does HOL blocking mean that the switching fabric itself is physically incapable of transmitting packets?

Explain.

---

# Part K — Output Queueing

### Q66.

Explain output queueing.

### Q67.

Why does output queueing eliminate HOL blocking?

### Q68.

What is the major disadvantage of output queueing?

### Q69.

Why might an output queue need to receive packets at \(N\) times the input line rate?

### Q70.

A \(4\times4\) router has input links of 1 Gbps.

What minimum switching-fabric speed may be required in the worst case for output queueing?

---

# Part L — VOQ

### Q71.

What is Virtual Output Queueing?

### Q72.

Why was VOQ introduced?

### Q73.

How does VOQ eliminate HOL blocking?

### Q74.

How many VOQs are required for an \(N\times N\) switch?

### Q75.

Calculate the number of VOQs for:

* \(4\times4\)
* \(8\times8\)
* \(16\times16\)
* \(64\times64\)

---

### Q76.

What is the main disadvantage of VOQ?

### Q77.

Why does VOQ increase scheduler complexity?

### Q78.

Compare ordinary input queueing with VOQ.

---

# Part M — PIM

### Q79.

What is Parallel Iterative Matching?

### Q80.

Why is PIM useful in VOQ-based switches?

### Q81.

Why does PIM use multiple iterations?

### Q82.

Explain how iterative matching can improve switch utilization.

### Q83.

What is the trade-off between scheduling quality and scheduling speed?

### Q84.

Why can't a scheduler spend unlimited time calculating the perfect matching?

---

# Part N — Fast Path / Slow Path

### Q85.

What is the Fast Path?

### Q86.

What is the Slow Path?

### Q87.

List five Fast Path operations.

### Q88.

List five Slow Path operations.

### Q89.

Why must Fast Path processing be extremely fast?

### Q90.

Explain why routing-table updates are generally associated with the slow/control path rather than normal packet forwarding.

### Q91.

Differentiate Fast Path and Slow Path.

---

# Part O — Scenario-Based Questions

### Q92.

A router receives millions of packets per second. Each packet requires destination lookup, TTL decrement, classification, queue selection, and scheduling.

Which path handles these operations and why?

---

### Q93.

A link between two routers suddenly fails. The routing protocol exchanges information and calculates a new route.

Which processing path is involved?

Explain.

---

### Q94.

A \(16\times16\) crossbar has a single FIFO queue at each input. The first packet at Input 1 is destined for Output 5, which is currently busy. The second packet is destined for Output 7, which is free.

What happens?

Identify the problem and propose a solution.

---

### Q95.

A network operator replaces input FIFO queues with VOQs.

What problem are they trying to solve?

What new problem does this introduce?

---

### Q96.

A router has 32 input ports and 32 output ports.

The designer wants to use VOQ.

Calculate the total number of queues required.

---

### Q97.

A router designer chooses output queueing instead of VOQ.

Each input link operates at 10 Gbps and there are 8 inputs.

What switching-fabric rate may be required in the worst case?

---

### Q98.

A \(64\times64\) crossbar is experiencing low utilization even though many packets are waiting at input ports.

Give two possible reasons based on the concepts you've studied.

---

# Part P — Design / Analytical Questions

### Q99.

You are designing a high-speed router.

Compare these three architectures:

1. Input Queueing
2. Output Queueing
3. VOQ

Which architectural properties would you consider when choosing among them?

---

### Q100.

Design a conceptual architecture for a router that needs:

* High throughput
* Low latency
* Multiple simultaneous packet transfers
* QoS support
* Avoidance of HOL blocking

Explain which switching-fabric and queueing techniques you would use and why.

---

# 🔥 Very Important Numerical/Analytical Set

These are particularly useful for practicing calculation-based questions.

### Q101.

A \(4\times4\) crossbar receives the following requests:

```text
I1 → O1
I2 → O1
I3 → O2
I4 → O3
```

Can all four transfers occur simultaneously?

If not, determine the maximum number of simultaneous transfers.

---

### Q102.

Requests are:

```text
I1 → O2
I2 → O3
I3 → O1
I4 → O4
```

Determine whether this is a valid matching.

---

### Q103.

Requests:

```text
I1 → O1
I2 → O2
I3 → O2
I4 → O4
```

How many transfers can occur simultaneously?

---

### Q104.

An \(8\times8\) crossbar has 8 input queues.

During a timeslot, only 5 input-output connections can be scheduled.

Calculate instantaneous utilization relative to the maximum possible 8 transfers.

---

### Q105.

A switch transfers 7 packets in a timeslot where 8 transfers are theoretically possible.

Calculate utilization.

---

# 🔴 Higher-Difficulty Professor-Style Questions

These are the ones I'd **definitely practice** because they test whether you understand the architecture rather than memorizing definitions.

### Q106.

> Explain why increasing the switching-fabric bandwidth alone does not necessarily solve the performance problem of an input-queued crossbar switch.

---

### Q107.

> A packet behind a blocked packet cannot be transmitted even though its destination output is idle. Explain the exact mechanism responsible for this behavior and propose an architectural solution.

---

### Q108.

> Output queueing theoretically achieves 100% throughput. Why is it nevertheless difficult to implement in a high-speed \(N\times N\) router?

---

### Q109.

> VOQ eliminates HOL blocking, but why does it not completely solve the switching problem?

---

### Q110.

> Explain why a scheduler is necessary even when a crossbar provides a physical connection between every input and output.

---

### Q111.

> A router has 64 input ports. Compare the scaling implications of ordinary input queueing and VOQ.

---

### Q112.

> Explain the relationship between crossbar size, number of crosspoints, number of VOQs, scheduler complexity, and scalability.

---

### Q113.

> Why is packet reordering undesirable inside a switching fabric even though TCP can theoretically reorder packets at the destination?

---

### Q114.

> Explain why QoS requirements make switching-fabric scheduling more complicated.

---

### Q115.

> A crossbar has sufficient physical bandwidth but still achieves less than 100% utilization. Explain possible reasons.

---

# ⭐ The Most Important 15 to Practice First

If you don't want to solve all 115 immediately, start with these:

**Q2** — Routing table vs forwarding table
**Q4** — Packet processing sequence
**Q20** — Packet journey through router
**Q26** — Why PC router throughput is limited
**Q30** — Router performance numerical
**Q36** — Shared vs switched backplane
**Q42** — \(N^2\) crosspoints
**Q45** — Crossbar scheduler
**Q60** — HOL blocking
**Q63** — HOL scenario
**Q68** — Output queueing trade-off
**Q73** — Why VOQ eliminates HOL
**Q74** — \(N^2\) VOQs
**Q81** — Why PIM uses iterations
**Q112** — Complete architecture/scalability analysis

These 15 cover essentially all the **important reasoning patterns** in this chapter.
