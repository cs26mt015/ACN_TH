# CS 606T — Chapter 2: Router Architectures and Switching Fabrics

This answer sheet follows the numbering in `questions.md`. Use the detailed material for 5–10 mark answers; use the numbered answers for direct exam questions. For a good exam answer: begin with a definition, write 3–5 points, add a small diagram/table where relevant, and finish with the main advantage or limitation.

## Long-answer theory material

### Router: routing and forwarding

A router performs two fundamental functions:

1. **Routing (control plane):** Routing protocols discover topology, calculate paths, and build a routing table.
2. **Forwarding (data plane):** For every arriving packet, the router uses a forwarding table to select the next hop/output interface and transfers the packet onward.

`Routing algorithm → Routing table → Forwarding table → Fast packet forwarding`

Routing is comparatively infrequent and can be software/control-processor based. Forwarding happens for every packet and must run at line rate, so it is usually implemented in fast hardware such as ASICs/NPUs.

### Packet processing at a router

When an IP packet enters an ingress interface, the normal sequence is:

1. Receive the frame and validate the IP header.
2. Decrement **TTL**. TTL prevents a packet caught in a routing loop from circulating forever.
3. Update the IPv4 header checksum because changing TTL changes the header.
4. Perform destination/forwarding-table lookup, usually using longest-prefix matching.
5. Classify the packet, if policies use fields such as source/destination IP, ports, or protocol.
6. Apply services as needed: NAT, filtering, priority/QoS, or IPv4 fragmentation when packet size exceeds the outgoing MTU and fragmentation is allowed.
7. Select an output queue, schedule the packet, traverse the switching fabric, and transmit from the egress interface.

### Router architecture

```text
Ingress line card → switching fabric/backplane → egress line card
                          ↑
                   routing processor
```

- **Line cards:** interface with physical links; receive/transmit packets, perform fast-path processing, lookups, buffering, classification, and queueing.
- **Switching fabric:** internal path that moves packets from selected input ports to selected output ports.
- **Routing processor:** runs routing protocols, creates/updates routing and forwarding state, and handles management/control work.

Buffers are needed at inputs when arrivals exceed fabric capacity and at outputs when arrivals exceed the output-link rate.

### Switching fabrics and crossbars

A switching fabric/backplane is the router's internal interconnection system. Its important metrics are **throughput**, **latency**, and **path diversity**.

- A **shared backplane** has one shared communication path, so only limited transfers can use it at once; aggregate throughput is limited by backplane bandwidth.
- A **switched backplane** permits multiple independent input-to-output transfers simultaneously and therefore has higher aggregate throughput.
- An `N × N` crossbar has `N` inputs, `N` outputs, and `N²` crosspoints. A crosspoint is the controllable connection between one input and one output.
- During one timeslot, each input can send at most one packet and each output can receive at most one packet. The scheduler selects a conflict-free **matching**.

### Queueing, HOL blocking, output queueing, and VOQ

With one FIFO queue at each input, a packet at the head whose output is busy blocks later packets even if their outputs are free. This is **Head-of-Line (HOL) blocking**. It reduces utilization because usable output capacity can sit idle.

**Output queueing** places packets at their destination output queue. It avoids HOL blocking, but in the worst case an output may receive one packet from every input simultaneously. Therefore the fabric/output memory may need approximately `N ×` line rate in an `N × N` switch.

**Virtual Output Queueing (VOQ)** maintains a separate queue at each input for every output. Thus an `N × N` switch has `N²` VOQs. A blocked packet for one output does not block packets for another output. Its trade-off is more memory/queue state and a much more complex scheduler.

### PIM and fast/slow paths

**Parallel Iterative Matching (PIM)** is a scheduler for VOQ switches. Inputs request outputs, outputs grant one request, inputs accept one grant, and the process may repeat for several iterations. More iterations can find a larger conflict-free matching and improve utilization, but consume more scheduling time.

**Fast path:** high-speed per-packet operations—header validation, TTL/checksum work, forwarding lookup, classification, queue selection, and scheduling.

**Slow path:** less frequent control or exceptional work—routing-protocol processing, routing-table updates, ARP, ICMP error generation, and advanced/exception packet processing. “Slow” means outside the normal high-rate forwarding path, not necessarily a physically slow CPU.

## A. Core theory (Q1–Q10)

**Q1.** A router performs **routing** and **forwarding**. Routing uses algorithms/protocols to calculate end-to-end paths and populate tables; it is a control-plane function. Forwarding is the per-packet data-plane action of looking up the destination and sending the packet to the selected next hop/output interface.

**Q2.** A routing table contains routes learned from routing protocols, including destination prefixes, metrics, and path information. A forwarding table is an optimized data-plane table derived from it; it maps destination prefixes to the immediate forwarding action—next hop and output interface.

**Q3.** Forwarding is required for every arriving packet and must sustain very high packet rates with low delay. Therefore it uses specialized hardware and optimized lookup structures. Routing calculations and topology updates occur less frequently, are more complex, and are appropriate for software/control-plane processing.

**Q4.** On packet arrival, the ingress card: (1) validates header, (2) decrements TTL, (3) updates IPv4 header checksum, (4) performs forwarding lookup, (5) classifies/applies policies, (6) fragments if IPv4/MTU rules require it, (7) queues the packet, (8) sends it across the fabric, and (9) schedules/transmits it on the egress interface.

**Q5.** Header validation detects malformed or corrupted packets before they consume network resources. The router checks fields such as IP version, header length, and checksum; invalid packets are discarded.

**Q6.** TTL is decreased by one at every router hop. If it reaches zero, the router discards the packet and normally generates ICMP Time Exceeded. TTL prevents endless circulation caused by routing loops.

**Q7.** In IPv4, TTL is a header field included in the header checksum calculation. Decrementing TTL changes header bits, so the checksum must be recomputed/updated to keep the header valid.

**Q8.** Route/forwarding lookup compares the packet's destination IP address with forwarding-table prefixes, normally choosing the longest matching prefix. It obtains the next hop, output interface, and possibly associated forwarding/queueing action.

**Q9.** Fragmentation divides a large IPv4 packet into smaller fragments. It may be needed when packet size exceeds the outgoing link's MTU and fragmentation is permitted; otherwise the packet is dropped and appropriate feedback may be generated.

**Q10.** Packet classification identifies packets according to header fields so policies can be applied. Examples: source IP address, destination IP address, source/destination port number, protocol number, DSCP, or VLAN field. It supports QoS, filtering, security, and accounting.

## B. Routing and forwarding (Q11–Q15)

**Q11.** A routing algorithm uses topology and policy information to calculate paths. Its result is stored in the routing table. The router converts the usable routes into a compact optimized forwarding table, which the data plane uses to forward each packet quickly.

**Q12.** A routing table may contain protocol-specific information, multiple candidate routes, metrics, and control information. Searching it directly for every packet is too slow; a forwarding table contains only optimized next-hop/output actions for fast lookup.

**Q13.** After detecting failure, the routing protocol exchanges updates and recomputes routes. The routing table removes/changes affected paths; the forwarding table is then updated so new packets use an available alternate path or are dropped if no path exists.

**Q14.** Routing calculates paths and builds routing state. Forwarding transfers a received packet to its selected output. Classification identifies packet class from fields/policies. Scheduling decides which queued packet is transmitted next.

**Q15.** A routing update may happen after a topology/policy change, while forwarding happens once for every packet. Millions of packets per second can arrive, so forwarding must operate at line rate and much faster than control-plane processing.

## C. Router architecture (Q16–Q23)

**Q16.** The three major parts are: (1) **line cards** for network interfaces and fast ingress/egress processing, (2) **switching fabric** for internal input-to-output packet transfer, and (3) **routing processor** for control plane, routing protocols, management, and table updates.

**Q17.** A line card connects the router to a physical network link. It receives/transmits frames, performs fast forwarding operations, stores packets in buffers, and participates in classification, queueing, and scheduling.

**Q18.** The switching fabric is the internal interconnection mechanism that moves a packet from its ingress line card to its selected egress line card. It should provide high throughput, low latency, and multiple simultaneous transfers where possible.

**Q19.** The routing processor runs routing protocols, maintains routing/forwarding tables, handles control/management traffic, and coordinates the router's control plane.

**Q20.** Packet journey: the ingress line card receives and processes the packet, makes/uses forwarding decision and buffers it if necessary; the switching fabric transfers it to the selected egress card; the egress card queues, schedules, encapsulates as needed, and transmits it on the outgoing link.

**Q21.** Line-card memory stores forwarding/classification state and buffers packets during temporary rate mismatch, contention, or scheduling delay. Without it, bursts would cause immediate packet loss.

**Q22.** Packets accumulate in ingress buffers/queues. Queueing delay increases, and packets are dropped if the input buffer becomes full.

**Q23.** Input buffers absorb traffic that cannot immediately cross the fabric; output buffers absorb traffic that arrives faster than an egress link can transmit. Both are needed because contention and bursts can occur on either side.

## D–E. PC router and numericals (Q24–Q33)

**Q24.** A received packet moves NIC → PCI bus → main memory; the CPU reads/processes it in memory; then it is copied from memory → PCI bus → outgoing NIC. Thus the bus is used twice per forwarded packet, in addition to CPU/memory work.

**Q25.** Every forwarded packet crosses the PCI bus on ingress and egress. These two transfers share finite bus capacity, so the bus can saturate before the NICs or CPU reach their nominal rates.

**Q26.** Since each forwarded bit normally crosses the PCI bus twice, a bus of rate `B` can carry at most about `B/2` of one-direction forwarded payload, ignoring CPU and overhead. Hence throughput is approximately half the bus bandwidth.

**Q27.** Improve performance by using a faster bus/fabric, reducing packet copies (zero-copy/DMA), using faster NICs/memory, offloading work to hardware, batching packets, and parallelizing processing across CPU cores/queues.

**Q28.** Multiple cores can process different packets/queues in parallel, increasing packets per second when workload is parallelizable and memory/bus/fabric are not bottlenecks.

**Q29.** Copying consumes bus bandwidth, memory bandwidth, CPU cycles, and time. Fewer copies lower per-packet latency and free resources for more packets.

**Q30.** Header = `80 × 8 = 640 bits`.

- NIC→memory time = `640/(4×10^9) = 160 ns`.
- CPU time = `250/(2.5×10^9) = 100 ns` (one instruction per cycle assumption).
- Memory→NIC time = `160 ns`.
- Total = `160 + 100 + 160 = 420 ns`.
- Packet rate = `1/420 ns ≈ 2.381 million packets/s`; payload/header throughput = `640/420 ns ≈ 1.524 Gbps`.

**Q31.** `60 bytes = 480 bits`. Each PCI transfer = `480/(3×10^9) = 160 ns`. CPU time = `300/(3×10^9)=100 ns`. Total processing time = `160 + 100 + 160 = 420 ns`.

**Q32.** Processing time = `500 ns`.

1. Packets/s = `1/(500×10^-9) = 2,000,000 packets/s`.
2. At 100 bytes = 800 bits, throughput = `2,000,000 × 800 = 1.6×10^9 bps`.
3. Throughput = **1.6 Gbps**.

**Q33.** Packets/s = `1/(250×10^-9) = 4,000,000 packets/s`.

## F–I. Fabrics, crossbars, and numericals (Q34–Q59)

**Q34.** A switching fabric is the internal system that transfers packets between a router's input and output ports.

**Q35.** It is called a switching backplane because it is the internal interconnection/backplane through which line cards exchange packets.

**Q36.** A shared backplane is one common bus/path shared by all cards, usually allowing limited concurrent traffic. A switched backplane uses switching elements/paths so independent input-output pairs can transfer simultaneously.

**Q37.** All traffic competes for one shared bus. Its total traffic cannot exceed the bus capacity, so contention limits aggregate throughput.

**Q38.** Independent input-output pairs use separate internal paths/crosspoints at the same time. Aggregate throughput can therefore approach the sum of several line rates.

**Q39.** Three key metrics are **throughput** (transfer rate), **latency** (fabric traversal delay), and **path diversity** (ability to support independent simultaneous paths).

**Q40.** Throughput is packets/bits carried per unit time; latency is time from fabric input to output; path diversity is the number/flexibility of simultaneous non-conflicting input-output paths.

**Q41.** An `N × N` crossbar is a switch with `N` inputs and `N` outputs in which each input has a controllable crosspoint connection to every output.

**Q42.** There is one possible connection for every input-output pair. With `N` inputs and `N` outputs, the number is `N × N = N²`.

**Q43.** `4²=16`, `8²=64`, `16²=256`, and `32²=1024` crosspoints.

**Q44.** A crosspoint is the controlled connection at one input/output intersection. When enabled, it permits that input's packet to travel to that output for the scheduled interval.

**Q45.** The centralized scheduler examines requests and chooses a conflict-free matching. It ensures no input sends to two outputs and no output receives from two inputs in the same timeslot.

**Q46.** An output has one receiving/transmission resource for a timeslot. Two inputs requesting it would conflict, so only one can be selected unless special speedup/buffering is provided.

**Q47.** A crossbar can connect different inputs to different outputs at the same time. For example I1→O1 and I2→O2 do not conflict and may proceed simultaneously.

**Q48.** With fixed, predictable arrivals/requests, the scheduler may precompute or repeat a schedule. If traffic pattern remains stable, less real-time computation is required.

**Q49.** Dynamic scheduling is required when packet destinations, queue occupancy, or priorities vary from one timeslot to another.

**Q50.** A matching is a set of input-output connections in which no input and no output appears more than once.

**Q51.** A **maximal matching** cannot be enlarged by adding another edge without conflict. A **maximum matching** has the greatest possible number of edges among all matchings. Every maximum matching is maximal, but a maximal matching need not be maximum.

**Q52.** It maximizes the number of packets transferred in that timeslot, improving fabric utilization and reducing queues.

**Q53.** Exact maximum matching can be too slow for very short hardware timeslots. Approximate algorithms trade a small loss in matching quality for fast enough decisions at line rate.

**Q54.** A timeslot is a short scheduling interval during which the chosen set of crossbar connections is active and packets/cells are transferred.

**Q55.** A conflict-free schedule is required because one input cannot transmit two packets at once and one output cannot accept two selected inputs at once.

**Q56.** Maximum = `4 × 5 = 20` transfers. Utilization = `15/20 × 100 = 75%`.

**Q57.** Maximum transfers = `8 × 10 = 80`; actual = `52`; utilization = `52/80 × 100 = 65%`; unused opportunities = `80−52 = 28`.

**Q58.** Maximum = `16 × 100 = 1600`; at 75%, transferred = `0.75 × 1600 = 1200 packets`.

**Q59.** Crosspoints = `32² = 1024`. At one control bit each, memory = **1024 bits = 128 bytes** (ignoring implementation overhead).

## J–M. HOL, output queueing, VOQ, PIM (Q60–Q84)

**Q60.** Head-of-Line (HOL) blocking occurs in an input FIFO when the first packet cannot move to its desired output, thereby preventing packets behind it from moving even if their destinations are free.

**Q61.** FIFO serves only the queue head. If that head packet requests a busy output, later packets cannot bypass it, regardless of output availability.

**Q62.** Example:

```text
Input FIFO: [P1→O1] [P2→O2]
O1 busy; O2 free
```

P1 cannot go to O1. Under FIFO, P2 cannot overtake P1, so O2 is idle. This is HOL blocking.

**Q63.** P1 waits because Output 1 is unavailable. P2 also waits behind P1 even though Output 2 is free. Thus Output 2 may be wasted during that slot.

**Q64.** HOL can leave an output idle despite having a packet for it behind another input's blocked head packet. Fewer simultaneous transfers occur, reducing crossbar utilization.

**Q65.** No. The fabric may physically be able to connect an input to an idle output. The problem is queue order: the desired packet is trapped behind the blocked head packet.

**Q66.** In output queueing, packets cross the fabric promptly and are stored at the queue of their destination output. The egress scheduler selects packets for each outgoing link.

**Q67.** Packets are separated by output after crossing the fabric; a blocked packet for one output does not sit ahead of packets destined for another output at an input FIFO.

**Q68.** The major disadvantage is high speedup requirement. In the worst case, one output may receive packets from all `N` inputs in the same interval, requiring roughly `N ×` input line rate at the fabric/output memory.

**Q69.** If all `N` inputs simultaneously send packets to the same output, that output must accept `N` packets during one input packet time. Therefore it needs up to `N` times the single-input rate.

**Q70.** Worst case fabric rate = `4 × 1 Gbps = 4 Gbps` toward an output (and corresponding output-memory capability).

**Q71.** Virtual Output Queueing keeps a separate queue at each input for packets destined to each output.

**Q72.** VOQ was introduced to remove HOL blocking without requiring the extreme `N ×` speedup of pure output queueing.

**Q73.** Since packets for different outputs are in separate queues, a blocked queue for O1 does not prevent the scheduler from selecting a packet in the O2 queue.

**Q74.** There are `N` inputs, each with `N` output-specific queues: total `N²` VOQs.

**Q75.** `4²=16`, `8²=64`, `16²=256`, and `64²=4096` VOQs.

**Q76.** VOQ requires `N²` queue state/memory management and a complex high-speed scheduler.

**Q77.** The scheduler must examine requests from many VOQs and choose a conflict-free input-output matching every timeslot; it must also consider fairness, priority, and queue occupancy.

**Q78.** Ordinary input queueing has one FIFO per input, is simpler, but suffers HOL blocking. VOQ has one queue per input-output pair, eliminates HOL blocking, but needs `N²` queues and complex scheduling.

**Q79.** Parallel Iterative Matching (PIM) is an iterative request-grant-accept scheduling algorithm that finds a large conflict-free matching between VOQ inputs and outputs.

**Q80.** VOQ creates many eligible input-output requests. PIM quickly chooses non-conflicting transfers, allowing the fabric to use those VOQs efficiently.

**Q81.** One pass may create a small or poor matching. Repeating request/grant/accept steps can add/improve compatible matches and raise utilization.

**Q82.** A larger matching schedules more distinct input-output pairs in a timeslot, so fewer fabric opportunities are idle and queues drain faster.

**Q83.** More scheduling work/iterations usually gives a better/larger matching, but takes more time and hardware complexity. The scheduler must finish before the next timeslot.

**Q84.** Timeslots are very short at high line rates. An unlimited search for the perfect matching would finish too late, making the otherwise optimal decision useless for current traffic.

## N. Fast and slow paths (Q85–Q91)

**Q85.** The fast path is the high-speed data-plane path that performs routine per-packet processing and forwarding at or near line rate.

**Q86.** The slow path is the control/exception-processing path for less frequent, complex, or exceptional operations that are not normal line-rate forwarding.

**Q87.** Five fast-path operations: (1) IP header validation, (2) TTL decrement, (3) checksum update/validation, (4) forwarding lookup, (5) classification, (6) queue selection, and (7) packet scheduling. Any five may be written.

**Q88.** Five slow-path operations: (1) routing-protocol processing/keepalives, (2) routing-table updates, (3) ARP processing, (4) ICMP error generation, (5) advanced IP/options processing, and (6) fragmentation/reassembly as classified in the lecture. Any five may be written.

**Q89.** It handles every normal packet. If it cannot keep up with the arrival rate, buffers grow, delay rises, packets drop, and the router fails to meet line-rate throughput.

**Q90.** Updates require route computation, protocol messages, and changes only after topology/policy events; they are control-plane work. Normal forwarding simply uses the already prepared forwarding table for every packet.

**Q91.** Fast path is frequent, latency-sensitive, per-packet data-plane work, often hardware accelerated. Slow path is less frequent control/exception work, often software/control-processor based and allowed relatively more processing time.

## O. Scenarios (Q92–Q98)

**Q92.** The **fast path** handles these tasks because lookup, TTL decrement, classification, queue selection, and scheduling are routine operations performed for each of millions of packets.

**Q93.** The **slow path/control plane** is involved. Routing protocols detect/exchange failure information, recompute the route, update the routing table, and then install revised forwarding entries.

**Q94.** The second packet cannot leave under single FIFO input queueing even though Output 7 is free, because the first packet for busy Output 5 is at the head. This is **HOL blocking**. Use **VOQ** so packets for Output 5 and Output 7 occupy separate queues, then schedule the Output-7 packet.

**Q95.** The operator is solving HOL blocking and poor input-queue utilization. VOQ introduces `N²` queues and requires a complex, fast conflict-free scheduler such as PIM.

**Q96.** Total VOQs = `32 × 32 = 1024`.

**Q97.** Worst case = `8 × 10 Gbps = 80 Gbps` fabric/output acceptance rate toward one output.

**Q98.** Possible reasons: (1) HOL blocking from FIFO input queues, so eligible packets are trapped behind blocked heads; (2) poor/non-maximum scheduling or output conflicts, where many inputs request the same output. Other valid reasons include priority/fairness constraints or limited fabric speed.

## P. Design and analytical questions (Q99–Q115)

**Q99.**

| Architecture | Strength | Main limitation |
|---|---|---|
| Input queueing | Simple; normal fabric speed | HOL blocking lowers throughput |
| Output queueing | Avoids HOL; can approach full throughput | Needs about `N×` speedup in worst case |
| VOQ | Avoids HOL at normal fabric speed | `N²` queues and complex scheduling |

Choose based on port count, line rate, memory bandwidth, delay/QoS needs, acceptable scheduler complexity, traffic pattern, cost, and scalability.

**Q100.** Use an `N × N` switched crossbar because it supports multiple simultaneous transfers and low internal latency. Use VOQ at inputs to avoid HOL blocking, and a fast iterative scheduler such as PIM to make conflict-free high-utilization matches. Maintain priority-aware queues/scheduling for QoS, and use sufficient input/output buffering for bursts. This design trades hardware and scheduler complexity for high throughput and low latency.

**Q101.** All four cannot occur: I1 and I2 both request O1. A maximum matching has **3** transfers, for example `I1→O1`, `I3→O2`, and `I4→O3`.

**Q102.** Yes. I1→O2, I2→O3, I3→O1, and I4→O4 use each input once and each output once, so this is a valid matching of size 4.

**Q103.** At most **3** transfers: I1→O1, one of I2/I3→O2, and I4→O4. I2 and I3 conflict at O2.

**Q104.** Utilization = `5/8 × 100 = 62.5%`.

**Q105.** Utilization = `7/8 × 100 = 87.5%`.

**Q106.** Higher fabric bandwidth helps only if packets can be selected to use it. With one FIFO at each input, HOL blocking can trap packets for idle outputs behind blocked head packets. Output conflicts and poor scheduling can also leave capacity unused; VOQ plus effective scheduling is needed.

**Q107.** The mechanism is HOL blocking: FIFO permits only the head packet to request service. If its output is busy, a later packet for a free output cannot bypass it. Replace the single FIFO with VOQs and use a matching scheduler.

**Q108.** Output queueing can avoid HOL, but all `N` inputs may target one output at once. The fabric and output memory would need to accept about `N` packets per packet time (`N×` speedup), which becomes impractical at high port counts/rates due to memory bandwidth, power, and cost.

**Q109.** VOQ removes FIFO HOL blocking but still needs a scheduler to resolve simultaneous requests for the same output. It also requires `N²` queues, fairness/priority decisions, and a high-speed matching calculation, so contention and implementation complexity remain.

**Q110.** Physical crosspoints only make connections possible; they do not decide which requests may coexist. When requests conflict, the scheduler chooses one packet per input and one per output for the timeslot, preventing collisions and improving utilization.

**Q111.** With 64 inputs, ordinary input queueing has 64 FIFO queues but suffers HOL blocking. VOQ has `64² = 4096` queues, eliminating HOL but greatly increasing memory bookkeeping and scheduler request/matching complexity.

**Q112.** An `N × N` crossbar has `N²` crosspoints and VOQ has `N²` queues. As N grows, hardware/control state grows quadratically, while scheduling must process an increasingly large bipartite request graph within each timeslot. Therefore scalability is limited by area, power, memory/state, and scheduling speed, even though crossbars offer high parallelism.

**Q113.** Reordering increases receiver buffering and delay, complicates loss/retransmission inference, and may reduce TCP performance because TCP can mistake reordering for loss. Preserving order within a flow makes predictable high-performance transport behavior easier.

**Q114.** QoS requires the scheduler to consider not only maximum matching but also priority, delay deadlines, bandwidth guarantees, fairness, and isolation. These constraints can conflict and make match selection more complex than simple maximum-throughput scheduling.

**Q115.** A fabric may have enough raw bandwidth but less than 100% utilization because of output contention, HOL blocking, non-optimal scheduling, unmatched/empty queues, priority or fairness constraints, and queue/order limitations. Raw capacity alone does not ensure an eligible conflict-free matching each slot.
