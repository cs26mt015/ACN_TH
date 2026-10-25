# ACN-TH

**User:** Anonymous  
**Created:** 9/19/2026 11:22:08  
**Updated:** 10/25/2026 17:32:20  
**Exported:** 10/25/2026 17:47:46  
**Link:** [https://chatgpt.com/c/6aae2307-7b8c-83e9-87c1-09040396fbf2](https://chatgpt.com/c/6aae2307-7b8c-83e9-87c1-09040396fbf2)  

## Response:
10/25/2026, 4:38:35 PM · gpt-6

Let's start the **Packet Scheduling** chapter for your Advanced Computer Networks (CS 606T) course. We'll cover the PPT in order, with notebook-ready notes, examples, and exam-oriented explanations. We won't move to the next chapter until we finish all the topics in this one.

# Packet Scheduling - Topic 1: Introduction and Need for Scheduling

## 1\. What is packet scheduling?

Packet scheduling is the process of deciding **which packet should be transmitted next when multiple packets are waiting in a router's output queue**\.

A router may receive packets from multiple input interfaces, but several packets may need to leave through the same output interface. Since the outgoing link has limited bandwidth, the router must decide the order in which packets are transmitted.

[![PPT - EECS 122: Introduction to Computer Networks Packet Scheduling and QoS PowerPoint Presentation - ID:6778940](https://images.openai.com/static-rsc-4/5zkMAXI6asx7H_F4-QqMGb-GTYvTBsZUTwUbjGnfqK3997QgZHe6aAbSWjVI3EAz2JmSHg0BHQxwyH1N-v0RwlpkJTn3Z6QDWGNtIgehp9dG-f8zNPRePc6X1igsY94h3cRI9XtzyShnAEhoKmmzZ7JttWd8Rh7dH6U3Zd-8qO3w_sCVY-WTmpe56CE4lXFs?purpose=fullsize)](https://www.slideserve.com/paloma-fields/eecs-122-introduction-to-computer-networks-packet-scheduling-and-qos-powerpoint-ppt-presentation)

Conceptual view: multiple packet flows competing for an output link.

For example, suppose three applications send packets through the same router\:

- Flow A: video call packets
- Flow B: file download packets
- Flow C: email packets

If all three flows need the same output link, the router must determine which packet to transmit first, which packet to transmit next, and how much bandwidth each flow should receive.

This decision-making process is called **packet scheduling**\.

## 2\. Why do we need packet scheduling?

Different applications have different network requirements.

| Application | Main requirement |
|--------|--------|
| Voice and video calls | Low delay and low jitter |
| Video streaming | Sufficient bandwidth and limited delay |
| File downloads | Fair bandwidth allocation and reliable delivery |
| Email | Usually tolerates delays |
| Online gaming | Low latency and jitter |

If a router treats every packet identically, delay-sensitive applications may experience poor performance when competing with heavy file transfers.

Packet scheduling helps the router allocate bandwidth, control delay, improve fairness, and provide different levels of Quality of Service (QoS).

## 3\. Two types of service

### A. Guaranteed service

Guaranteed service attempts to provide specified performance guarantees to a traffic flow.

Its main parameters are\:

- **Bandwidth guarantee\:** A minimum transmission rate is provided.
- **Delay bound\:** Packets must meet a specified delay limit.
- **Jitter bound\:** Variation in packet delay is limited.
- **Packet loss bound\:** Loss is controlled within a specified limit for conforming traffic.

Examples include real-time voice and interactive video under suitable QoS mechanisms.

### B. Best-effort service

Best-effort service does not promise a particular bandwidth, delay, or jitter level. The network forwards packets when resources are available.

Examples include ordinary email and many file downloads.

Best-effort does not mean packets are always lost or delayed. It means there is no guaranteed performance level.

## 4\. How scheduling provides QoS

Consider an outgoing link with a capacity of 10 Mbps.

Suppose the traffic demands are\:

- Video call: 2 Mbps
- File download: 7 Mbps
- Email: 3 Mbps

Total demand\:

$$
2+7+3=12\text{ Mbps}
$$

But the link can provide only 10 Mbps.

The router must decide how to distribute its available capacity. A scheduling algorithm can prioritize the video flow or allocate bandwidth fairly among the competing flows.

The exact result depends on the scheduling discipline and its configuration.

**Important\:** Scheduling alone cannot guarantee every requested service if the total demand exceeds the available capacity. Admission control and traffic regulation may also be necessary.

## 5\. What makes a good scheduling algorithm?

Your PPT lists four major requirements.

1. **Ease of implementation\:** The router must make scheduling decisions very quickly, often within microseconds or less.
2. **Fairness\:** Flows should receive an appropriate share of the available resources.
3. **Performance bounds\:** The algorithm should support predictable bandwidth, delay, jitter, or loss characteristics where possible.
4. **Ease of admission control\:** It should be possible to determine whether a new traffic flow can be accepted without violating existing guarantees.

We will study each requirement in detail.

## 6\. Exam-oriented questions

Try these questions before moving to the next topic.

**Next topic\:** Requirements of scheduling algorithms - especially **max-min fairness**, including unweighted and weighted numerical problems. This is an important topic for both theory and numerical questions.

## Response:
10/25/2026, 4:39:57 PM · gpt-6

# Packet Scheduling — Topic 2: Max-Min Fairness

Max-min fairness is an important concept in packet scheduling. It helps a network allocate limited bandwidth fairly among multiple flows when their bandwidth demands are different.

## 1\. What is Max-Min Fairness?

**Definition\:** A bandwidth allocation is max-min fair if increasing the allocation of any flow necessarily requires decreasing the allocation of another flow that already has an equal or smaller allocation.

In simple words\:

- Every flow gets a fair share of available bandwidth.
- A flow that needs less bandwidth should receive what it needs.
- Any remaining bandwidth is shared among flows that still need more.
- No flow should receive additional bandwidth by reducing the allocation of a flow that already receives less bandwidth.

### Key idea

Suppose a link has a capacity of 10 Mbps and three flows demand\:

| Flow | Bandwidth demand |
|--------|--------|
| $F_1$ | 2 Mbps |
| $F_2$ | 4 Mbps |
| $F_3$ | 8 Mbps |
| **Total demand** | **14 Mbps** |

The total demand exceeds the link capacity, so all three flows cannot receive their full demands.

A max-min fair allocation gives\:

- $F_1 = 2$ Mbps
- $F_2 = 4$ Mbps
- $F_3 = 4$ Mbps

The total allocation is $2+4+4=10$ Mbps.

Notice that $F_1$ and $F_2$ receive their full demands. The remaining bandwidth goes to $F_3$\.

## 2\. Water-filling algorithm

The easiest way to calculate max-min fair allocation is the **water-filling method**\.

### Algorithm

1. Initially, consider all flows active and their allocations to be zero.
2. Increase the allocation of every active flow equally.
3. If a flow reaches its demand, freeze its allocation at that demand.
4. Remove that flow from the active set and redistribute the remaining capacity among the remaining active flows.
5. Stop when the link capacity is exhausted or every flow has received its full demand.

The important distinction is that **we equalize allocations among active flows, not necessarily among all flows**\. Flows with smaller demands may stop receiving additional bandwidth earlier.

## 3\. Numerical Example 1: Unweighted Max-Min Fairness

**Question\:** A link has a total capacity of 10 Mbps. Four flows have demands of 2, 2.6, 4, and 5 Mbps. Find the max-min fair allocation.

### Given

| Flow | Demand |
|--------|--------|
| $F_1$ | 2 Mbps |
| $F_2$ | 2.6 Mbps |
| $F_3$ | 4 Mbps |
| $F_4$ | 5 Mbps |
| Link capacity | 10 Mbps |

### Step 1: Equal allocation among all flows

If we divide the capacity equally, each flow initially gets

$$
\frac{10}{4}=2.5\text{ Mbps}
$$

But $F_1$ demands only 2 Mbps, so it cannot use the full 2.5 Mbps. Freeze its allocation at 2 Mbps.

The remaining capacity is

$$
10-2=8\text{ Mbps}
$$

The remaining active flows are $F_2,F_3,F_4$\.

### Step 2: Redistribute the remaining capacity

Divide the remaining 8 Mbps equally among the three active flows\:

$$
\frac{8}{3}\approx 2.667\text{ Mbps}
$$

Flow $F_2$ demands only 2.6 Mbps, so freeze its allocation at 2.6 Mbps.

The remaining capacity becomes

$$
10-2-2.6=5.4\text{ Mbps}
$$

### Step 3: Allocate to the remaining two flows

The remaining capacity is shared equally between $F_3$ and $F_4$\:

$$
\frac{5.4}{2}=2.7\text{ Mbps}
$$

Neither flow has reached its demand, so both receive 2.7 Mbps.

### Final answer

| Flow | Demand | Allocated bandwidth |
|--------|--------|--------|
| $F_1$ | 2 | 2 Mbps |
| $F_2$ | 2.6 | 2.6 Mbps |
| $F_3$ | 4 | 2.7 Mbps |
| $F_4$ | 5 | 2.7 Mbps |
| **Total** | **13.6** | **10 Mbps** |

Check\:

$$
2+2.6+2.7+2.7=10\text{ Mbps}
$$

This is the max-min fair allocation.

## 4\. Numerical Example 2: Weighted Max-Min Fairness

In ordinary max-min fairness, active flows receive equal bandwidth. In **weighted max-min fairness**, bandwidth is shared in proportion to the flows' weights.

**Question\:** A link has a capacity of 16 Mbps. Four flows have demands and weights as follows. Find the weighted max-min fair allocation.

| Flow | Demand (Mbps) | Weight |
|--------|--------|--------|
| $F_1$ | 4 | 2.5 |
| $F_2$ | 2 | 4 |
| $F_3$ | 10 | 0.5 |
| $F_4$ | 4 | 1 |

### Step 1: Normalize the weights

Multiply all weights by 2 to obtain integer weights\:

$$
(w_1,w_2,w_3,w_4)=(5,8,1,2)
$$

The total weight is

$$
5+8+1+2=16
$$

Initially, the bandwidth shares are proportional to these weights\:

$$
x_i=C\frac{w_i}{\sum_j w_j}
$$

With $C=16$ Mbps\:

| Flow | Initial share |
|--------|--------|
| $F_1$ | $16(5/16)=5$ Mbps |
| $F_2$ | $16(8/16)=8$ Mbps |
| $F_3$ | $16(1/16)=1$ Mbps |
| $F_4$ | $16(2/16)=2$ Mbps |

However, $F_1$ demands only 4 Mbps and $F_2$ only 2 Mbps. Both receive their full demands and are frozen.

### Step 2: Redistribute the remaining capacity

The remaining capacity is

$$
16-4-2=10\text{ Mbps}
$$

Only $F_3$ and $F_4$ remain active, with weights 1 and 2. Their total weight is 3.

Their proportional shares of the remaining 10 Mbps are

$$
F_3:\quad 10\left(\frac{1}{3}\right)
=\frac{10}{3}\text{ Mbps}
$$

$$
F_4:\quad 10\left(\frac{2}{3}\right)
=\frac{20}{3}\text{ Mbps}
$$

But $F_4$ demands only 4 Mbps. Freeze $F_4$ at 4 Mbps.

### Step 3: Give the remaining capacity to $F_3$

The capacity left for $F_3$ is

$$
16-4-2-4=6\text{ Mbps}
$$

Since $F_3$ demands 10 Mbps, it can use all 6 Mbps.

### Final answer

| Flow | Demand | Final allocation |
|--------|--------|--------|
| $F_1$ | 4 | 4 Mbps |
| $F_2$ | 2 | 2 Mbps |
| $F_3$ | 10 | 6 Mbps |
| $F_4$ | 4 | 4 Mbps |
| **Total** | **20** | **16 Mbps** |

The final allocation is

$$
\boxed{(4,\ 2,\ 6,\ 4)\text{ Mbps}}
$$

**Important\:** Weighted fairness does not mean every flow must receive its weight-proportional share regardless of demand. A flow cannot be allocated more bandwidth than it needs, and its unused share is redistributed.

## 5\. Max-Min Fairness vs. Weighted Max-Min Fairness

| Feature | Max-min fairness | Weighted max-min fairness |
|--------|--------|--------|
| Bandwidth sharing | Equally among active flows | Proportional to weights |
| Priority | Same for all flows | Higher weight gets a larger share |
| Demand limit | Allocation cannot exceed demand | Allocation cannot exceed demand |
| Unused bandwidth | Redistributed to other active flows | Redistributed according to remaining weights |
| Example | Equal sharing among active flows | Weights $2:1$ give a $2:1$ share while both flows remain active |

## 6\. Exam questions for practice

Try solving these before checking your notes.

## 7\. What comes next?

The next topic is **QoS Metrics and Performance Bounds**\. We will cover bandwidth guarantees, packet loss, delay bounds (worst-case, average and percentile), jitter, and admission control.

These concepts explain how a scheduler's fairness translates into measurable network service quality.

## Response:
10/25/2026, 4:41:10 PM · gpt-6

# Packet Scheduling - Topic 3: QoS Metrics and Performance Bounds

## 1\. What is Quality of Service (QoS)?

**Quality of Service (QoS)** refers to a network's ability to provide predictable service to different types of traffic based on their requirements.

Different applications have different network requirements\:

- Voice calls require low delay and low jitter.
- Video streaming requires sufficient bandwidth and stable delivery.
- File transfers benefit from high throughput but can tolerate some delay.
- Online gaming requires low latency and minimal packet loss.

A packet scheduler helps manage these competing requirements, but **scheduling alone does not automatically guarantee QoS**\. Guarantees may also require traffic regulation, resource reservation, buffer management, and admission control.

## 2\. Four important QoS metrics

[![Network cable data transferring concept](https://images.openai.com/static-rsc-4/9G_AN5_dk9X5hHzfIGAo5DfU_CRTRa_rW7mk8WbpvBW_wCLA_PTyCSvY84MZP0ELHNjdJNEZJuo3NJbJFVegwG65D2JdZrTEy88MWhTBvWnKh3nGz3_5KdH4pExp3ZV2AT5Q6PY-wrV0Xcnx0EkQ-Rx179ilCnipIoOvyHWk0hPceRj41cXeJVDwEMtNDohA?purpose=fullsize)](https://media.gettyimages.com/id/1309707385/photo/network-cable-data-transferring-concept.jpg?b=1&c=rjcc0qTRkbG_GgLibBBcNYUdiqgpsrLCD3zcdKNfoQ0%3D&k=20&s=612x612&w=0)

**1\. Bandwidth**

The data transmission rate allocated to a flow, commonly measured in bits per second (bps).

Example: A video flow receives a guaranteed rate of 5 Mbps.

A bandwidth guarantee specifies a minimum service rate under stated conditions; it does not necessarily mean the flow continuously transmits at that rate.

---

[![Paketverlust | videocalling.app](https://images.openai.com/static-rsc-4/ID73QB_6XKuoTnHJdSzlOR9jljs7806iIvcix88Hh9x-Z8qbllKeZkOjGqV-f4gqU2tSsdQ7rtEbTgF9_AelzkZAx6yEdWvarRvP6pH3H_d-zGy6JZN9REqtZqgIbrtKWHMpA3RHUhEmeneiWOeTrMNfH93rV5D5jW0XZu3QmFgh-vAbBa0U5tjqWaV3RPDx?purpose=fullsize)](https://videocalling.app/de/glossary/packet-loss)

**2\. Packet loss**

The fraction of transmitted packets that fail to reach their destination.

$$
\text{Loss rate}=
\frac{\text{Packets lost}}{\text{Packets sent}}
$$

Example: If 20 out of 1,000 packets are lost, the loss rate is 2%.

---

[![컴퓨터 네트워크 4-1일차 : 네트워크의 4가지 지연 현상 총정리 : transmission delay / propagation delay / processing delay / queuing delay — 산하는 코딩중](https://images.openai.com/static-rsc-4/vbHw3FHSZoZ0eSh_xR0DoE7eT7R9u7ZGVTzsJBGy97gSMIK5Y7ZgaUetuD66iXvQIR60covXMeaY5biuiiNETgFWNAkG6JBzlaASOWu9OyUdMAAxh-fLNLrlyV3DmtRR-nKkB8xSyM0TgY6Mw7lElM3oLq5M2V0MMkod_brt75DlR3pktm_1e-K_ISd5i9HL?purpose=fullsize)](https://tksgk2598.tistory.com/49)

**3\. Delay (latency)**

The time taken for a packet to travel from its source to its destination, or across a specified part of the network.

Delay can include transmission, propagation, processing and queuing delays.

Example: A packet experiences 25 ms of one-way delay.

---

[![PPT - Chapter 7 Multimedia Networking PowerPoint Presentation, free download - ID:5415931](https://images.openai.com/static-rsc-4/GMqt606OmcDm020-diCYTcHWCgb_2Va1qLv_nuhVOYaBPpQq8V6z5c9lJbj-YB9lPXQ3qXTf-J7r4mzJhcD39GRU5Yb4C6yBrvtpUOkLsV9UksLeq_14_0paEnmp7XnZPRTLHD44SGcpBvcJXriDAKk_07g66hAOskIv3IQcOPelRTEVblZrhDM6nHK8k2yI?purpose=fullsize)](https://www.slideserve.com/kineks/chapter-7-multimedia-networking)

**4\. Jitter**

Variation in packet delay or packet arrival timing.

Example: If successive packets experience delays of 20 ms, 25 ms and 40 ms, their delays vary. This variation can disrupt real-time audio and video.

## 3\. Types of delay bounds

A QoS requirement may specify different types of delay limits. These are not interchangeable.

### A. Worst-case delay bound

The maximum permitted delay for any packet covered by the guarantee.

For example\:

$$
D_{\max}\leq 50\text{ ms}
$$

This means every packet within the specified traffic and operating assumptions must experience no more than 50 ms of delay.

**Use case\:** Time-critical traffic where even a single excessively delayed packet can cause problems.

### B. Average delay bound

The mean delay across packets over a specified observation period or traffic model.

$$
D_{\text{avg}}=\frac{1}{N}\sum_{i=1}^{N}D_i
$$

Example: If four packets have delays of 10, 20, 30 and 40 ms,

$$
D_{\text{avg}}=\frac{10+20+30+40}{4}
=25\text{ ms}
$$

An average of 25 ms does **not** mean every packet has a delay below 25 ms.

### C. Percentile delay bound

A percentile bound specifies a delay that a particular proportion of packets meet.

For example, a 95th-percentile delay of 40 ms means approximately 95% of packets have delays at or below 40 ms, according to the measured distribution.

This permits some packets to experience higher delays.

| Delay metric | Meaning | Example |
|--------|--------|--------|
| Worst-case | Maximum delay allowed | Every packet within the guarantee is at most 50 ms |
| Average | Mean packet delay | Mean delay is 25 ms |
| 95th percentile | Delay met by approximately 95% of packets | 95% of packets arrive within 40 ms |

**Exam point\:** An average or percentile bound is weaker than a strict worst-case guarantee. A network can have a good average delay while some packets still experience very large delays.

## 4\. Understanding jitter

Suppose packets are sent at regular intervals, but network congestion causes them to arrive at irregular intervals.

### Example: packet delays

Illustrative one-way delays

The packet delays vary from 20 ms to 40 ms. The difference between the maximum and minimum delay is

$$
J_{\text{range}}=40-20=20\text{ ms}
$$

This calculates delay range, a simple measure of variation. Other jitter definitions use differences between consecutive packet delays or statistical measures.

Why does jitter matter?

- In voice calls, variable arrival times can produce gaps or distortion.
- In video conferencing, frames may arrive too late for smooth playback.
- A playback buffer can compensate for some jitter, but it introduces additional delay.

## 5\. Numerical example: QoS metrics

**Question\:** A network transmits 2,000 packets. Of these, 30 are lost. The average packet delay is 18 ms, and the maximum observed delay is 55 ms. Calculate the packet loss rate and identify the delay metrics.

**Step 1: Calculate the packet loss rate.**

$$
\text{Loss rate}=\frac{30}{2000}\times100
=\boxed{1.5\%}
$$

**Step 2: Identify the delay values.**

- Average delay = 18 ms.
- Maximum observed delay = 55 ms.

The maximum observed delay is not necessarily a proven worst-case bound. To establish a strict worst-case guarantee, we need a valid bound under the specified network and traffic assumptions, not merely a finite set of measurements.

## 6\. Admission control

**Admission control** is the process of deciding whether a new flow can be accepted into a network while maintaining the QoS guarantees of existing flows.

When a new flow requests resources, the network checks whether sufficient bandwidth, buffers and other resources are available.

**New flow requests service**

**Check available resources**

Bandwidth, buffers and QoS requirements

**Accept**

Resources are sufficient; admit the flow.

**Reject or defer**

Resources are insufficient for the requested guarantee.

### Numerical example

A link has a capacity of 20 Mbps. Existing admitted flows have reserved bandwidths of 6 Mbps and 8 Mbps. A new flow requests a reservation of 7 Mbps.

Available bandwidth\:

$$
20-(6+8)=6\text{ Mbps}
$$

The new flow requests 7 Mbps, but only 6 Mbps remains.

Therefore, the network must **reject or defer the request** if it must preserve all existing reservations and meet the new flow's full reservation.

If the new flow instead requested 5 Mbps, the total reservation would be

$$
6+8+5=19\text{ Mbps}
$$

It could be admitted under a simple bandwidth-capacity test, provided all other resource and QoS requirements were also satisfied.

### Why is admission control important?

Without admission control, too many flows could enter the network. This can cause congestion, packet loss, excessive delay and violations of existing QoS guarantees.

Admission control is especially important for guaranteed-service traffic. Best-effort traffic can often be accepted without a reservation, but it may experience degraded performance during congestion.

## 7\. Quick revision notes

| Concept | Remember |
|--------|--------|
| Bandwidth | Data rate allocated to a flow |
| Packet loss | Fraction of packets that fail to arrive |
| Worst-case delay | Maximum delay covered by a valid guarantee |
| Average delay | Mean delay across packets |
| Percentile delay | Delay threshold met by a specified percentage of packets |
| Jitter | Variation in packet delay or arrival timing |
| Admission control | Determines whether a new flow can be accepted while preserving required service guarantees |

## 8\. Practice questions

---

**Next topic: FIFO (First-In, First-Out) Scheduling.** We will cover how FIFO works, its advantages and limitations, and why it cannot by itself guarantee fair bandwidth or different QoS requirements for competing flows.

## Response:
10/25/2026, 4:41:56 PM · gpt-6

# Packet Scheduling — Topic 4: FIFO Scheduling

## 1\. What is FIFO scheduling?

**FIFO (First-In, First-Out)**, also called **FCFS (First-Come, First-Served)**, is the simplest packet scheduling discipline.

The packet that arrives first at a queue is transmitted first. The scheduler does not consider the packet's application, priority, flow, or bandwidth requirement.

For example, suppose packets arrive in this order\:

$$
P_1 \rightarrow P_2 \rightarrow P_3
$$

The transmission order will be

$$
\boxed{P_1 \rightarrow P_2 \rightarrow P_3}
$$

Even if $P_3$ belongs to a real-time voice call and $P_1$ belongs to a file download, FIFO transmits $P_1$ first if it arrived first.

## 2\. How FIFO works

**Incoming packets**

Arrival order: P₁, P₂, P₃, P₄

**FIFO queue**

P₁ is at the front of the queue.

**Output link**

P₁ → P₂ → P₃ → P₄

The scheduler always selects the packet at the front of the queue. After that packet is transmitted, the next packet becomes eligible for transmission.

If a packet is already being transmitted, a newly arriving packet normally cannot interrupt it. This is because ordinary packet transmission is non-preemptive.

## 3\. Numerical example: FIFO transmission order

**Question\:** Three packets arrive at a router's output queue as follows. Determine their transmission order and waiting times.

Assume the link transmits one packet at a time and the times below are measured in milliseconds.

| Packet | Arrival time | Transmission time |
|--------|--------|--------|
| $P_1$ | 0 ms | 4 ms |
| $P_2$ | 1 ms | 2 ms |
| $P_3$ | 2 ms | 3 ms |

Assume the link is idle at time 0.

### Step 1: Determine the transmission order

- $P_1$ arrives at 0 ms and starts immediately.
- $P_2$ arrives at 1 ms while $P_1$ is transmitting.
- $P_3$ arrives at 2 ms while $P_1$ is still transmitting.

Therefore, FIFO serves them in the order $P_1,P_2,P_3$\.

### Step 2: Calculate start and finish times

| Packet | Start time | Finish time |
|--------|--------|--------|
| $P_1$ | 0 ms | 4 ms |
| $P_2$ | 4 ms | 6 ms |
| $P_3$ | 6 ms | 9 ms |

### Step 3: Calculate waiting time

Waiting time is the time between a packet's arrival and the start of its transmission.

$$
W_i=S_i-A_i
$$

where $S_i$ is the start time and $A_i$ is the arrival time.

- $P_1:\ 0-0=0$ ms
- $P_2:\ 4-1=3$ ms
- $P_3:\ 6-2=4$ ms

The average waiting time is

$$
W_{\text{avg}}=\frac{0+3+4}{3}
=\boxed{2.33\text{ ms}}
$$

## 4\. Advantages and limitations of FIFO

| Advantages | Limitations |
|--------|--------|
| Simple to implement | Does not prioritize delay-sensitive traffic |
| Low scheduling overhead | Does not guarantee per-flow bandwidth fairness |
| Preserves arrival order | A large packet can delay many smaller packets |
| Easy to understand and manage | Cannot independently provide different QoS guarantees |
| Works well for simple traffic scenarios | Congestion can cause queue buildup and packet loss |

### Important limitation: Head-of-line delay

Suppose a large file-transfer packet is ahead of a small voice packet in the FIFO queue. The voice packet must wait until the earlier packet finishes transmission.

This is one reason FIFO can perform poorly when traffic has different delay requirements.

**Note\:** FIFO preserves arrival order within its queue, but it does not necessarily preserve order across multiple independent queues.

## 5\. Exam-style questions

---

## Next topic: Generalized Processor Sharing (GPS)

GPS is an ideal scheduling model that provides a useful benchmark for fair packet scheduling. Unlike FIFO, it shares service among active flows according to their assigned weights.

We will cover its fluid model, how weighted sharing works, why it cannot be directly implemented for real packets, and how it helps us analyze practical schedulers.

## Response:
10/25/2026, 4:42:43 PM · gpt-6

# Packet Scheduling — Topic 5: Generalized Processor Sharing (GPS)

## 1\. What is GPS?

**Generalized Processor Sharing (GPS)** is an ideal packet-scheduling model that shares link capacity among multiple active flows according to their assigned weights.

Unlike FIFO, which serves packets in arrival order, GPS considers the flows competing for the link and allocates service to each active flow.

GPS is called an **ideal fluid model** because it assumes data is infinitely divisible into infinitesimally small pieces. Real packets cannot be divided arbitrarily during transmission, so GPS serves mainly as a theoretical benchmark for practical schedulers.

## 2\. How does GPS work?

Suppose an output link has a capacity of 12 Mbps and three flows are continuously backlogged, meaning each always has data waiting to be transmitted.

If all three flows have equal weights, GPS shares the link equally\:

$$
R_i=C\frac{w_i}{\sum_j w_j}
$$

where\:

- $R_i$ \= service rate allocated to flow $i$
- $C$ \= link capacity
- $w_i$ \= weight of flow $i$
- $\sum_j w_j$ \= total weight of all active flows

For equal weights $w_1=w_2=w_3=1$,

$$
R_1=R_2=R_3=\frac{12}{3}=4\text{ Mbps}
$$

**Output link: 12 Mbps**

**Flow 1**

## 4 Mbps

**Flow 2**

## 4 Mbps

**Flow 3**

## 4 Mbps

Each flow receives one-third of the capacity while all three remain continuously backlogged.

The important point is that GPS shares the **service rate among active flows**, rather than simply choosing the earliest packet.

## 3\. Weighted GPS

When flows have different weights, their service rates are proportional to those weights.

### Numerical example

A link has a capacity of 20 Mbps. Three continuously backlogged flows have weights 1, 2 and 3. Calculate their GPS service rates.

**Step 1: Calculate the total weight.**

$$
W=1+2+3=6
$$

**Step 2: Calculate each flow's rate.**

For Flow 1\:

$$
R_1=20\left(\frac{1}{6}\right)
=\frac{10}{3}\approx3.33\text{ Mbps}
$$

For Flow 2\:

$$
R_2=20\left(\frac{2}{6}\right)
=\frac{20}{3}\approx6.67\text{ Mbps}
$$

For Flow 3\:

$$
R_3=20\left(\frac{3}{6}\right)
=10\text{ Mbps}
$$

**Final answer**

| Flow | Weight | GPS service rate |
|--------|--------|--------|
| $F_1$ | 1 | 3.33 Mbps |
| $F_2$ | 2 | 6.67 Mbps |
| $F_3$ | 3 | 10 Mbps |
| **Total** | **6** | **20 Mbps** |

The rates are in the ratio $1:2:3$\.

### What if a flow has no data?

GPS distributes service among **backlogged flows only**\. If Flow 1 becomes idle, the other active flows share the available capacity according to their remaining weights.

For example, if only Flows 2 and 3 remain active, their weights are 2 and 3\:

$$
R_2=20\left(\frac{2}{5}\right)=8\text{ Mbps}
$$

$$
R_3=20\left(\frac{3}{5}\right)=12\text{ Mbps}
$$

Their rates increase because Flow 1 is no longer competing for service.

## 4\. Why is GPS called an ideal fluid model?

Imagine that packets were liquid flowing through a pipe. GPS could divide that liquid into arbitrarily small portions and serve several flows simultaneously.

Real networks, however, transmit complete packets over a link. A packet must generally finish transmission before the next packet can be sent.

GPS assumes\:

- Data is infinitely divisible.
- Service can be shared continuously among active flows.
- Each backlogged flow receives its weighted share of service.

These assumptions make GPS useful for mathematical analysis, but they cannot be implemented exactly with ordinary packet transmission.

## 5\. Why is GPS important?

Although GPS is not directly implementable, it provides a reference against which practical schedulers can be evaluated.

For example, suppose GPS would have served a particular flow with a certain amount of data by a given time. A real packet scheduler may transmit slightly less or more by that time because it must transmit complete packets.

By bounding the difference between actual service and GPS service, we can reason about fairness and delay.

This leads to an important distinction\:

- **GPS\:** An ideal, continuously shared service model.
- **Practical schedulers\:** Algorithms that choose complete packets while attempting to approximate GPS.

Weighted Fair Queuing (WFQ) is a prominent example of a practical scheduler designed to approximate GPS.

## 6\. GPS vs. FIFO

| Feature | FIFO | GPS |
|--------|--------|--------|
| Scheduling principle | Arrival order | Weighted sharing among active flows |
| Service model | Complete packets | Infinitesimally divisible fluid |
| Supports flow weights | Not inherently | Yes |
| Fairness across flows | Not guaranteed | Weighted fairness under its model |
| Directly implementable | Yes | No, idealized model |
| Main purpose | Simple packet ordering | Theoretical benchmark for practical schedulers |

## 7\. Exam-style questions

---

## Next topic: Fairness Bounds

Next, we will study **absolute fairness bounds and relative fairness bounds**\. These concepts help measure how closely a practical scheduler follows the ideal GPS model and how fairly it serves different flows.

## Response:
10/25/2026, 4:43:22 PM · gpt-6

# Packet Scheduling — Topic 6: Fairness Bounds

Fairness bounds help us evaluate how closely a practical packet scheduler behaves compared with an ideal scheduler such as Generalized Processor Sharing (GPS).

Remember the difference\:

- **GPS** is an ideal fluid model that shares service among active flows.
- **Practical schedulers** transmit complete packets, so their service may differ from GPS.

Fairness bounds quantify that difference.

## 1\. Absolute fairness bound

An **absolute fairness bound** compares the service received by one flow under a practical scheduling discipline with the service it would have received under GPS.

Let\:

- $S_i(t_1,t_2)$ \= bits transmitted for flow $i$ by the practical scheduler during the interval $[t_1,t_2]$\.
- $GPS_i(t_1,t_2)$ \= bits that GPS would transmit for flow $i$ during the same interval.

A common form of an absolute service bound is

$$
\boxed{S_i(t_1,t_2)\ge GPS_i(t_1,t_2)-E_i}
$$

where $E_i$ is the maximum permitted service deficit, measured in bits.

This means the practical scheduler should not fall behind the GPS reference by more than the specified bound, under the assumptions of the scheduling model.

### Numerical example

Suppose GPS would transmit 12,000 bits for a flow during a particular interval. A practical scheduler has an absolute service error bound of 1,000 bits.

Then the practical scheduler must transmit at least

$$
S_i\ge12,000-1,000
$$

$$
\boxed{S_i\ge11,000\text{ bits}}
$$

The actual service could be 11,000 bits, 11,500 bits or 12,000 bits, provided it satisfies the bound. The bound does not require the practical scheduler to match GPS exactly.

## 2\. Relative fairness bound

A **relative fairness bound** compares the service received by two flows under the same scheduling discipline.

For unweighted flows, a simple relative fairness measure compares their transmitted bits\:

$$
\Delta_{ij}=|S_i-S_j|
$$

where $S_i$ and $S_j$ are the bits transmitted for flows $i$ and $j$ over the interval being considered.

For weighted flows, service should be compared relative to each flow's weight. A common normalized comparison is

$$
\boxed{\Delta_{ij}=
\left|\frac{S_i}{w_i}-\frac{S_j}{w_j}\right|}
$$

This normalized comparison captures whether flows have received service proportional to their weights. A formal fairness bound places an upper limit on the permitted discrepancy under specified scheduling assumptions.

### Numerical example: Equal-weight flows

Suppose two equally weighted flows have received the following service\:

- Flow 1: 8,000 bits
- Flow 2: 7,500 bits

Their service difference is

$$
\Delta_{12}=|8,000-7,500|
=\boxed{500\text{ bits}}
$$

The two flows have received nearly equal service, with a difference of 500 bits.

### Numerical example: Weighted flows

Suppose Flow 1 has weight 2 and has received 8,000 bits, while Flow 2 has weight 1 and has received 4,000 bits.

Their normalized service values are

$$
\frac{S_1}{w_1}=\frac{8,000}{2}=4,000
$$

$$
\frac{S_2}{w_2}=\frac{4,000}{1}=4,000
$$

Thus,

$$
\boxed{\Delta_{12}=0}
$$

The flows have received service exactly proportional to their weights.

## 3\. Absolute vs. relative fairness bounds

| Feature | Absolute fairness bound | Relative fairness bound |
|--------|--------|--------|
| Comparison | Practical scheduler vs. GPS | One flow vs. another flow |
| Focus | How closely service follows the ideal reference | How fairly service is distributed between flows |
| Example | A flow receives at least 11,000 bits when GPS would provide 12,000 bits with a 1,000-bit deficit bound | Two equally weighted flows differ by no more than a specified number of transmitted bits |
| Main purpose | Bound the deviation from ideal service | Bound the disparity between flows |

**Important exam note\:** The exact mathematical form of a fairness bound depends on the scheduling discipline and the assumptions in the course definition. The formulas above illustrate the service comparisons; do not assume every scheduler has the same bound or that a bound exists without proving it.

## 4\. Why do fairness bounds matter?

They help network designers answer questions such as\:

- Does a flow receive approximately the service it should receive?
- Does a practical scheduler approximate GPS closely enough?
- How much can packet-level scheduling cause flows to differ?
- Can we derive useful delay or service guarantees?

For example, Weighted Fair Queuing (WFQ) is designed to approximate GPS. Its analysis can bound how far its packet-level service deviates from the ideal GPS service.

## 5\. Exam-style questions

---

## Next topic: Weighted Round Robin (WRR)

Next, we will cover **Weighted Round Robin (WRR)**, a practical scheduling algorithm that allocates service to queues according to assigned weights. We will solve both fixed-size and variable-size packet examples step by step.

## Response:
10/25/2026, 4:44:08 PM · gpt-6

# Packet Scheduling — Topic 7: Weighted Round Robin (WRR)

## 1\. What is Weighted Round Robin?

**Weighted Round Robin (WRR)** is a packet scheduling algorithm that serves packets from multiple queues in a cyclic order, giving each queue a number of transmission opportunities proportional to its assigned weight.

Unlike FIFO, which uses a single queue and serves packets in arrival order, WRR typically maintains a separate queue for each flow or traffic class.

A queue with a larger weight receives more service opportunities per scheduling cycle.

### Example

Suppose a router has three queues\:

- $Q_1$\: weight 2
- $Q_2$\: weight 3
- $Q_3$\: weight 1

For fixed-size packets, one scheduling round can serve\:

$$
\boxed{Q_1:2,\quad Q_2:3,\quad Q_3:1}
$$

The scheduler repeats this pattern while queues have packets waiting. The exact order within a round depends on the implementation, but the number of service opportunities follows the weights.

## 2\. How WRR works

**Three input queues**

**Q₁**

Weight = 2

**Q₂**

Weight = 3

**Q₃**

Weight = 1

**WRR scheduler**

One cycle: 2 packets from Q₁, 3 from Q₂, 1 from Q₃

**Output link**

The scheduler moves from one queue to the next, serving up to the number of packets specified by its weight. If a queue has fewer packets than its quota, it serves the available packets and moves on.

## 3\. WRR with fixed-size packets

When all packets have the same size, the number of packets transmitted is proportional to the number of bytes transmitted. This makes WRR straightforward to implement.

### Numerical example

**Question\:** Three queues have weights 0.5, 0.75 and 1. Their packet counts are 5, 7 and 4, respectively. Find the scheduling order using WRR.

### Step 1: Normalize the weights

Multiply all weights by 4 to obtain integers\:

$$
0.5:0.75:1
$$

$$
\boxed{2:3:4}
$$

Thus, each full scheduling round serves up to\:

- Queue 1: 2 packets
- Queue 2: 3 packets
- Queue 3: 4 packets

### Step 2: Execute the rounds

Initially, the queues contain\:

$$
(Q_1,Q_2,Q_3)=(5,7,4)
$$

Round 1\:

| Queue | Quota | Packets served | Remaining |
|--------|--------|--------|--------|
| $Q_1$ | 2 | 2 | 3 |
| $Q_2$ | 3 | 3 | 4 |
| $Q_3$ | 4 | 4 | 0 |

After Round 1, the remaining queue lengths are $(3,4,0)$\.

Round 2\:

| Queue | Quota | Packets served | Remaining |
|--------|--------|--------|--------|
| $Q_1$ | 2 | 2 | 1 |
| $Q_2$ | 3 | 3 | 1 |
| $Q_3$ | 4 | 0 | 0 |

After Round 2, the remaining queue lengths are $(1,1,0)$\.

Round 3\:

| Queue | Quota | Packets served | Remaining |
|--------|--------|--------|--------|
| $Q_1$ | 2 | 1 | 0 |
| $Q_2$ | 3 | 1 | 0 |
| $Q_3$ | 4 | 0 | 0 |

All packets have now been served.

The complete scheduling sequence, using the queue order $Q_1,Q_2,Q_3$, is\:

$$
\boxed{1,1,2,2,2,3,3,3,3,\;
1,1,2,2,2,\;1,2}
$$

The total number of packets transmitted is

$$
5+7+4=\boxed{16\text{ packets}}
$$

**Important\:** A queue's weight defines its service quota, not the number of packets it must transmit if fewer packets are available. Empty queues are skipped.

## 4\. WRR with variable-size packets

When packet sizes differ, serving the same number of packets does not necessarily allocate the same amount of bandwidth.

For example, serving two packets of 1,500 bytes gives 3,000 bytes of service, whereas serving two packets of 50 bytes gives only 100 bytes.

Therefore, ordinary packet-count WRR can produce unfair byte-level bandwidth allocation when packet sizes differ substantially.

### Numerical example

Suppose the mean packet sizes and desired weights are\:

| Queue | Mean packet size | Weight |
|--------|--------|--------|
| $Q_1$ | 50 bytes | 0.5 |
| $Q_2$ | 500 bytes | 0.75 |
| $Q_3$ | 1,500 bytes | 1 |

To approximate the desired bandwidth proportions using packet-count quotas, we can make the number of packets served inversely proportional to the mean packet size.

### Step 1: Divide each weight by its mean packet size

$$
\frac{0.5}{50}=0.01
$$

$$
\frac{0.75}{500}=0.0015
$$

$$
\frac{1}{1500}\approx0.000667
$$

Thus, the approximate packet-count ratio is

$$
0.01:0.0015:0.000667
$$

### Step 2: Convert to integer quotas

Multiplying these ratios by 6,000 gives

$$
\boxed{60:9:4}
$$

So the approximate WRR quotas are 60 packets from $Q_1$, 9 from $Q_2$, and 4 from $Q_3$ per cycle.

Check the approximate bytes served per cycle\:

| Queue | Packets | Mean size | Approximate bytes |
|--------|--------|--------|--------|
| $Q_1$ | 60 | 50 | 3,000 |
| $Q_2$ | 9 | 500 | 4,500 |
| $Q_3$ | 4 | 1,500 | 6,000 |

The byte ratio is

$$
3000:4500:6000=2:3:4
$$

This matches the original weight ratio $0.5:0.75:1=2:3:4$\.

**Limitation\:** This is an approximation based on mean packet sizes. Actual packet sizes vary, so the actual byte allocation may differ from the target. In practice, keeping accurate packet-size estimates can also be difficult.

## 5\. Advantages and disadvantages of WRR

| Advantages | Disadvantages |
|--------|--------|
| Simple and efficient to implement | Packet-count fairness may not imply byte-level fairness |
| Supports weighted service | Large weights may lead to long service bursts |
| Gives higher-weight queues more service opportunities | Mean packet sizes may be difficult to estimate |
| Skips empty queues and can use the link efficiently | Does not automatically guarantee strict delay or loss bounds |

## 6\. Exam-style questions

---

## Next topic: Deficit Round Robin (DRR)

DRR improves on basic Round Robin scheduling by tracking a **deficit counter** for each queue. It handles variable-size packets more effectively without requiring accurate mean packet-size estimates.

We will study the deficit counter, quantum, packet eligibility, and a step-by-step numerical example.

## Response:
10/25/2026, 4:44:40 PM · gpt-6

> **Demonstrating DRR scheduling**
>
> I’ll explain the visit-and-deficit process with three queues sharing equal quanta, showing packets sent, leftover deficit, and why oversized packets wait. I’ll also note fairness, variable packet sizes, and practical limitations.
>
> **Demonstrated DRR scheduling**
>
> 
>
> Worked for 18s

# Packet Scheduling — Topic 8: Deficit Round Robin (DRR)

## 1\. What is Deficit Round Robin?

**Deficit Round Robin (DRR)** is a packet scheduling algorithm designed to provide approximately fair bandwidth sharing among multiple flows, even when their packets have different sizes.

It improves on basic Round Robin and Weighted Round Robin by tracking how many bytes a queue is still entitled to transmit.

The key idea is simple\:

- Each queue receives a **quantum** of bytes during a visit.
- Each queue maintains a **deficit counter (DC)**\.
- A packet is transmitted only if its size is no greater than the queue's current deficit.
- If a packet is too large to transmit, the remaining deficit is carried over to the queue's next visit.

This carry-over mechanism allows a queue to transmit large packets without needing to know the average packet size in advance.

## 2\. Important terms

| Term | Meaning |
|--------|--------|
| Quantum ($Q_i$\) | Number of bytes added to a queue's deficit counter during a round |
| Deficit counter ($DC_i$\) | Number of bytes the queue is currently permitted to transmit |
| Head-of-line packet | First packet waiting in a queue |
| Active queue | A queue containing packets waiting to be transmitted |
| Round | A cycle in which the scheduler visits eligible queues |

The basic update is

$$
DC_i \leftarrow DC_i+Q_i
$$

When the queue transmits a packet of size $L$,

$$
DC_i \leftarrow DC_i-L
$$

The scheduler continues transmitting from that queue while its head-of-line packet fits within the remaining deficit.

If the next packet is too large, the scheduler stops serving that queue for the current visit and carries the remaining deficit forward.

## 3\. DRR algorithm

For each queue $i$, maintain its quantum $Q_i$ and deficit counter $DC_i$\.

### Steps of DRR

1. Visit the next queue in the scheduling cycle.
2. If the queue is nonempty, add its quantum to its deficit counter.
3. Check the head-of-line packet. If its size is less than or equal to the deficit counter, transmit it and subtract its size from the counter.
4. Repeat Step 3 while the next packet fits within the remaining deficit.
5. If the next packet does not fit, move to the next queue while retaining the unused deficit.
6. If the queue becomes empty, reset its deficit counter to zero in the standard DRR algorithm.

The essential difference from WRR is that **DRR tracks bytes, not just packet counts**\.

## 4\. Numerical example: DRR step by step

**Question\:** Three queues use DRR with a quantum of 500 bytes each. Their packet sizes, in queue order, are\:

- $Q_1:\ 200,\ 400,\ 300$ bytes
- $Q_2:\ 700,\ 200$ bytes
- $Q_3:\ 300,\ 300,\ 300$ bytes

Assume all queues are initially nonempty and all deficit counters start at zero. Determine how the packets are transmitted.

### Round 1

Initially\:

$$
DC_1=DC_2=DC_3=0
$$

**Queue $Q_1$**

Add its quantum\:

$$
DC_1=0+500=500
$$

- Transmit 200 bytes: $DC_1=500-200=300$\.
- The next packet is 400 bytes, which exceeds the remaining deficit of 300 bytes.

So $Q_1$ stops with $DC_1=300$\.

**Queue $Q_2$**

Add its quantum\:

$$
DC_2=0+500=500
$$

Its first packet is 700 bytes, which exceeds the deficit. No packet is transmitted, and $DC_2=500$ is carried forward.

**Queue $Q_3$**

Add its quantum\:

$$
DC_3=0+500=500
$$

- Transmit 300 bytes: $DC_3=200$\.
- The next packet is 300 bytes, which exceeds the remaining deficit.

So $Q_3$ stops with $DC_3=200$\.

Round 1 summary\:

| Queue | Bytes transmitted | Remaining deficit |
|--------|--------|--------|
| $Q_1$ | 200 | 300 |
| $Q_2$ | 0 | 500 |
| $Q_3$ | 300 | 200 |

### Round 2

Each nonempty queue receives another quantum.

**Queue $Q_1$**

$$
DC_1=300+500=800
$$

- Transmit 400 bytes: $DC_1=400$\.
- Transmit 300 bytes: $DC_1=100$\.

The queue is now empty, so its deficit counter is reset to zero.

**Queue $Q_2$**

$$
DC_2=500+500=1000
$$

- Transmit 700 bytes: $DC_2=300$\.
- Transmit 200 bytes: $DC_2=100$\.

The queue is now empty, so its deficit counter is reset to zero.

**Queue $Q_3$**

$$
DC_3=200+500=700
$$

- Transmit 300 bytes: $DC_3=400$\.
- Transmit another 300 bytes: $DC_3=100$\.
- The final 300-byte packet does not fit within the remaining deficit.

Round 2 summary\:

| Queue | Bytes transmitted in round | Remaining packets | Deficit |
|--------|--------|--------|--------|
| $Q_1$ | 700 | None | 0 (reset) |
| $Q_2$ | 900 | None | 0 (reset) |
| $Q_3$ | 600 | One 300-byte packet | 100 |

### Round 3

Only $Q_3$ remains nonempty.

$$
DC_3=100+500=600
$$

Its final 300-byte packet is transmitted\:

$$
DC_3=600-300=300
$$

The queue becomes empty, so the counter resets to zero.

### Final answer

The transmission order is\:

$$
\boxed{
Q_1(200),\ Q_3(300),\
Q_1(400),\ Q_1(300),\
Q_2(700),\ Q_2(200),\
Q_3(300),\ Q_3(300),\
Q_3(300)
}
$$

Total bytes transmitted\:

$$
900+900+900=\boxed{2700\text{ bytes}}
$$

The important observation is that $Q_2$ could not transmit its 700-byte packet during Round 1, but its unused deficit was retained. In Round 2, its accumulated deficit was sufficient to transmit both packets.

## 5\. Why does DRR handle variable-size packets better?

Consider two queues\:

- $Q_1$ contains packets of 100 bytes.
- $Q_2$ contains packets of 1,000 bytes.

Suppose both queues receive a quantum of 1,000 bytes per round.

With packet-count WRR, serving one packet from each queue transmits 100 bytes from $Q_1$ and 1,000 bytes from $Q_2$\. The service is very different in terms of bytes.

With DRR, each queue receives a byte budget. Over time, the scheduler can allocate approximately equal byte service to equally weighted queues, even when packet sizes differ.

DRR does not need to know the average packet size in advance.

## 6\. Advantages and limitations

| Advantages | Limitations |
|--------|--------|
| Handles variable-size packets effectively | Service is not perfectly fluid like GPS |
| Does not require mean packet-size estimates | A large quantum can create longer bursts |
| Efficient to implement | Small quanta may require multiple rounds before a large packet is eligible |
| Supports weighted service by assigning different quanta | Does not automatically guarantee strict delay bounds |
| Can provide approximately fair byte-level service | Performance depends on quantum selection and traffic conditions |

For weighted DRR, a common approach is to select quanta proportional to the desired weights\:

$$
Q_i=k\,w_i
$$

where $k$ is a scaling factor and $w_i$ is the flow's weight.

For example, weights $1:2:3$ could use quanta $500:1000:1500$ bytes.

## 7\. WRR vs. DRR vs. GPS

| Feature | WRR | DRR | GPS |
|--------|--------|--------|--------|
| Basic approach | Cyclic packet quotas | Cyclic byte budgets | Continuous weighted service |
| Variable-size packets | Can be unfair in byte terms | Handles them effectively | Idealized fluid sharing |
| Tracks deficit | No | Yes | No packet-level deficit |
| Directly implementable | Yes | Yes | No |
| Role | Practical scheduler | Practical scheduler | Theoretical benchmark |

## 8\. Exam practice

---

## Next topic: Weighted Fair Queuing (WFQ)

WFQ is a practical packet-scheduling algorithm designed to approximate the service provided by GPS. We will study how it calculates packet finish times, orders packets for transmission, and supports weighted service.

This is an important topic because **WFQ connects the ideal GPS model to real packet-based scheduling**\.

## Response:
10/25/2026, 4:45:32 PM · gpt-6

# Packet Scheduling — Topic 9: Weighted Fair Queuing (WFQ)

## 1\. What is WFQ?

**Weighted Fair Queuing (WFQ)** is a packet-scheduling algorithm designed to approximate the service provided by Generalized Processor Sharing (GPS), while transmitting complete packets.

It assigns a weight to each flow and uses packet finish-time calculations to decide which packet should be transmitted next.

The main idea is\:

- Each flow has its own queue.
- Each flow is assigned a weight.
- Higher-weight flows receive a larger share of link capacity when they are continuously backlogged.
- The scheduler calculates virtual finish times for packets.
- Among eligible packets, the scheduler transmits the one with the smallest virtual finish time.

Unlike GPS, WFQ works with real, indivisible packets.

## 2\. Why do we need WFQ?

Recall the limitation of GPS: it assumes that data is infinitely divisible, which is not possible in conventional packet transmission.

WFQ addresses this limitation by calculating when each packet *would finish* under an idealized fluid-sharing model, then using those calculations to schedule real packets.

**Packets arrive in multiple flow queues**

**WFQ calculates virtual finish times**

Based on packet size, flow weight and virtual time

**Transmit the eligible packet with the smallest finish time**

## 3\. Virtual finish time

For a packet $k$ belonging to flow $i$, a common WFQ formulation is

$$
\boxed{
F_i^k=\max(F_i^{k-1},V(a_i^k))
+\frac{L_i^k}{w_i}
}
$$

where\:

- $F_i^k$ \= virtual finish time of packet $k$ in flow $i$
- $F_i^{k-1}$ \= virtual finish time of the preceding packet in the same flow
- $V(a_i^k)$ \= system virtual time when the packet arrives
- $L_i^k$ \= packet length in bits or bytes
- $w_i$ \= weight assigned to flow $i$

The units must be consistent. For example, if packet lengths are in bytes, finish times are in virtual-byte units unless the model defines a corresponding scaling factor.

### Understanding the formula

The formula has two parts.

**Part 1: Determine the starting virtual time**

$$
\max(F_i^{k-1},V(a_i^k))
$$

The packet cannot begin its virtual service before the preceding packet in its own flow finishes, or before the virtual time at which the packet arrives, whichever is later.

**Part 2: Add the packet's weighted service requirement**

$$
\frac{L_i^k}{w_i}
$$

A larger packet requires more virtual service. A higher weight reduces the packet's virtual service increment, giving that flow a greater share of service.

For the first packet of a flow, its previous finish time is commonly initialized to zero, so its virtual finish time is calculated from the arrival virtual time and its packet size.

## 4\. Numerical example: Calculate WFQ finish times

**Question\:** Three flows have the following packet sizes and weights. Assume all packets arrive at the same virtual time, $V(a)=0$, and the previous finish time for each flow is initialized to zero. Calculate their virtual finish times and determine the transmission order.

| Flow | Weight | Packet sizes |
|--------|--------|--------|
| $F_1$ | 1 | 500, 300 bytes |
| $F_2$ | 2 | 800, 400 bytes |
| $F_3$ | 1 | 200, 600 bytes |

### Step 1: Calculate finish times for $F_1$

First packet\:

$$
F_1^1=\max(0,0)+\frac{500}{1}
=\boxed{500}
$$

Second packet\:

$$
F_1^2=\max(500,0)+\frac{300}{1}
=\boxed{800}
$$

### Step 2: Calculate finish times for $F_2$

First packet\:

$$
F_2^1=\max(0,0)+\frac{800}{2}
=\boxed{400}
$$

Second packet\:

$$
F_2^2=\max(400,0)+\frac{400}{2}
=\boxed{600}
$$

Notice that $F_2$ has a higher weight, so its packets accumulate virtual finish time more slowly relative to their sizes.

### Step 3: Calculate finish times for $F_3$

First packet\:

$$
F_3^1=\max(0,0)+\frac{200}{1}
=\boxed{200}
$$

Second packet\:

$$
F_3^2=\max(200,0)+\frac{600}{1}
=\boxed{800}
$$

### Step 4: Schedule packets by finish time

The calculated tags are\:

| Packet | Virtual finish time |
|--------|--------|
| $F_3^1$ | 200 |
| $F_2^1$ | 400 |
| $F_1^1$ | 500 |
| $F_2^2$ | 600 |
| $F_1^2$ | 800 |
| $F_3^2$ | 800 |

The scheduler considers the head-of-line packet of each nonempty flow and transmits the eligible packet with the smallest finish-time tag.

The transmission order is therefore

$$
\boxed{
F_3^1\rightarrow F_2^1\rightarrow
F_1^1\rightarrow F_2^2
}
$$

The last two packets, $F_1^2$ and $F_3^2$, have equal finish-time tags of 800, so either may be selected first under a tie-breaking rule.

**Important\:** These finish-time tags are virtual scheduling values, not actual transmission completion times in milliseconds. In a full WFQ implementation, virtual time is updated as packets arrive and the server progresses, so later arrivals require the appropriate virtual arrival time.

## 5\. Advantages and limitations of WFQ

| Advantages | Limitations |
|--------|--------|
| Approximates GPS service | More complex than FIFO and basic WRR |
| Supports weighted bandwidth sharing | Requires virtual-time and finish-time calculations |
| Accounts for packet sizes | Scheduling overhead can increase with many flows |
| Can provide useful service and delay guarantees under appropriate assumptions | Does not automatically guarantee every QoS metric without additional traffic and system constraints |

## 6\. FIFO vs. WRR vs. DRR vs. WFQ

| Feature | FIFO | WRR | DRR | WFQ |
|--------|--------|--------|--------|--------|
| Scheduling basis | Arrival order | Weighted packet quotas | Weighted byte budgets | Virtual finish times |
| Multiple flow queues | Not required | Yes | Yes | Yes |
| Handles variable packet sizes fairly | Not inherently | Not necessarily | Yes, approximately | Yes, by approximating GPS |
| Uses weights | Not inherently | Yes | Yes, through quanta | Yes |
| Main advantage | Simplicity | Easy weighted scheduling | Efficient byte-level fairness | GPS-based service approximation |

## 7\. Exam-style practice

---

## What comes next?

We have covered the major scheduling disciplines from your notes: FIFO, GPS, fairness bounds, WRR, DRR and WFQ.

The next useful step is to consolidate them with a **packet scheduling comparison and exam-style problems**—particularly WRR scheduling tables, DRR deficit-counter tables, and WFQ finish-time calculations. These are the areas where step-by-step numerical practice is most valuable.

## Response:
10/25/2026, 4:46:47 PM · gpt-6

# Packet Scheduling — Consolidated Revision and Exam Practice

We have covered the major scheduling disciplines: FIFO, GPS, fairness bounds, WRR, DRR, and WFQ. Now let’s consolidate them and practise the three most important numerical question types: **WRR scheduling tables, DRR deficit-counter tables, and WFQ finish-time calculations.**

## 1\. Quick comparison of scheduling disciplines

| Discipline | Main idea | Handles variable packet sizes? | Key limitation |
|--------|--------|--------|--------|
| FIFO | Serve packets in arrival order | Yes, but no byte fairness | No per-flow fairness or priority |
| GPS | Share capacity among active flows according to weights | Ideal fluid model | Cannot directly transmit indivisible packets |
| WRR | Give each queue a fixed packet quota per round | Basic WRR may be unfair in bytes | Packet sizes can differ |
| DRR | Use a deficit counter and quantum measured in bytes | Yes | Service is packetized, not perfectly fluid |
| WFQ | Send the eligible packet with the smallest virtual finish-time tag | Yes | More computation and bookkeeping |

**Important distinctions for exams**

- **WRR\:** Think in terms of packets per round.
- **DRR\:** Think in terms of bytes, quantum, and deficit counter.
- **WFQ\:** Think in terms of virtual finish-time tags.
- **GPS\:** Think of an ideal fluid-sharing benchmark.
- **FIFO\:** Think of arrival order.

## 2\. Numerical 1 — Weighted Round Robin (WRR)

**Question\:** Three queues have packet weights $2:3:1$\. Their initial packet counts are\:

- $Q_1=5$
- $Q_2=4$
- $Q_3=3$

Assume equal-sized packets. Find the scheduling order and the remaining queue lengths after each round. A queue that becomes empty is skipped.

### Solution

The quotas per round are $2,3,1$\.

### Round-by-round table

| **Round** | **Q1 served** | **Q2 served** | **Q3 served** | **Remaining (Q1,Q2,Q3)** |
|--------|--------|--------|--------|--------|
| Initial | — | — | — | $5, 4, 3) |
| 1 | 2 | 3 | 1 | \(3, 1, 2) |
| 2 | 2 | 1 | 1 | \(1, 0, 1) |
| 3 | 1 | 0 | 1 | \(0, 0, 0) |

The scheduling order, using queue labels, is\:

$$
Q_1,Q_1,\ Q_2,Q_2,Q_2,\ Q_3,
$$

$$
Q_1,Q_1,\ Q_2,\ Q_3,\ Q_1,\ Q_3
$$

All 12 packets are served.

**Exam tip\:** A queue with fewer packets than its quota serves only the packets it has. Unused quota does not automatically mean the queue can transmit extra packets beyond its own quota.

## 3\. Numerical 2 — Deficit Round Robin (DRR)

**Question\:** Three queues use a quantum of 400 bytes each. Their packet sizes, in transmission order, are\:

- \(Q_1: 200, 500, 300$
- $Q_2: 600, 200$
- $Q_3: 300, 300, 200$

Initially, all deficit counters (DCs) are zero. Determine the transmission order and the deficit counters after each visit.

### Rules to apply

For every nonempty queue\:

1. Add its quantum to its deficit counter.
2. While the head packet fits within the counter, transmit it and subtract its size.
3. If the next packet does not fit, retain the remaining deficit for the next visit.
4. When a queue becomes empty, reset its deficit counter to zero.

### Solution

### DRR calculation table

| Round | Queue | DC after adding quantum | Packets transmitted | DC after visit |
|--------|--------|--------|--------|--------|
| 1 | Q1 | 400 | 200 | 200 |
| 1 | Q2 | 400 | None (600 won't fit) | 400 |
| 1 | Q3 | 400 | 300 | 100 |
| 2 | Q1 | 600 | 500 | 100 |
| 2 | Q2 | 800 | 600, 200 | 0 (queue empty) |
| 2 | Q3 | 500 | 300, 200 | 0 (queue empty) |
| 3 | Q1 | 500 | 300 | 0 (queue empty) |

### Final answer

- Transmission order: $Q_1(200), Q_3(300), Q_1(500), Q_2(600), Q_2(200), Q_3(300), Q_3(200), Q_1(300)$\.
- Total bytes transmitted: $1000+800+800=2600$ bytes.
- All queues become empty, and their counters reset to zero.

**Common mistake\:** Do not discard the deficit when a queue is still nonempty. For example, $Q_2$ carries its 400-byte deficit from Round 1 into Round 2.

---

## 4\. Numerical 3 — Weighted Fair Queuing (WFQ)

**Question\:** Three flows have the following packets. All packets arrive at virtual time $V=0$, and the previous finish-time tag for each flow is initially zero.

| Flow | Weight | Packet sizes |
|--------|--------|--------|
| $F_1$ | 1 | 300, 200 |
| $F_2$ | 2 | 800, 400 |
| $F_3$ | 1 | 100, 500 |

Calculate the virtual finish-time tags and determine the transmission order by selecting the smallest eligible head-packet tag.

### Formula

$$
F_i^k=\max(F_i^{k-1},V(a_i^k))+\frac{L_i^k}{w_i}
$$

Here $L_i^k$ is the packet size, $w_i$ is the flow weight, and $F_i^{k-1}$ is the previous packet's finish tag.

### Step 1: Calculate the tags

For $F_1$, weight $1$\:

$$
F_1^1=0+\frac{300}{1}=300
$$

$$
F_1^2=300+\frac{200}{1}=500
$$

For $F_2$, weight $2$\:

$$
F_2^1=0+\frac{800}{2}=400
$$

$$
F_2^2=400+\frac{400}{2}=600
$$

For $F_3$, weight $1$\:

$$
F_3^1=0+\frac{100}{1}=100
$$

$$
F_3^2=100+\frac{500}{1}=600
$$

### Step 2: Build the finish-time table

| Packet | Virtual finish tag |
|--------|--------|
| $F_1^1$ | 300 |
| $F_1^2$ | 500 |
| $F_2^1$ | 400 |
| $F_2^2$ | 600 |
| $F_3^1$ | 100 |
| $F_3^2$ | 600 |

### Step 3: Determine the scheduling order

Only the first waiting packet of each flow is initially eligible. After transmitting a packet, its next packet becomes eligible.

1. $F_3^1$, tag $100$
2. $F_1^1$, tag $300$
3. $F_2^1$, tag $400$
4. $F_1^2$, tag $500$
5. $F_2^2$, tag $600$
6. $F_3^2$, tag $600$

The final two packets have equal tags, so either order is valid under a tie-breaking rule that permits either choice.

**Important\:** These tags are virtual finish-time values, not actual clock times or physical packet transmission completion times.

---

## 5\. Exam practice — try these yourself

[Reset practice]

*Correction note for Question 2\:* The correct option is **A**\: after adding the quantum, the deficit is $200+500=700$ bytes; transmit the 600-byte packet and retain 100 bytes.

## 6\. Final revision checklist

Before considering Packet Scheduling complete, make sure you can\:

- Explain guaranteed service versus best effort.
- Calculate unweighted and weighted max-min fairness.
- Distinguish bandwidth, loss, delay, and jitter.
- Explain FIFO, GPS, and fairness bounds.
- Fill WRR round-by-round queue tables.
- Track DRR deficit counters correctly.
- Calculate WFQ virtual finish-time tags and select eligible packets.

That completes the core Packet Scheduling revision based on the topics you shared. The next step is to move to the next topic in your professor’s syllabus or PPT.



---
Powered by [ChatGPT Exporter](https://www.chatgptexporter.com)