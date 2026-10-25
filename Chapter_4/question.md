# Packet Scheduling — Question Bank (150 Questions)

This question bank covers the Packet Scheduling chapter for your Advanced Computer Networks course. It includes conceptual questions, short and long answers, proofs/derivations, numerical problems, scheduling tables, and scenario-based questions.

Coverage: QoS, scheduling requirements, max-min fairness, performance bounds, admission control, FIFO, GPS, fairness bounds, WRR, DRR, and WFQ.

As requested, these are questions only — no solutions or answers.

## Section A — Fundamentals and QoS (1–15)

1. What is packet scheduling in computer networks? Why is it required at an output interface?
2. Explain the role of a scheduler when multiple flows compete for the same outgoing link.
3. Differentiate between guaranteed-service and best-effort service.
4. What is Quality of Service (QoS)? Explain its importance in packet scheduling.
5. Define bandwidth, packet loss rate, delay, and jitter.
6. Why do real-time applications require different scheduling policies from file-transfer applications?
7. What are the essential requirements of an ideal packet scheduling discipline?
8. Explain why ease of implementation is an important property of a scheduling algorithm.
9. What is fairness in packet scheduling? Why is fairness difficult to achieve when flows have different demands?
10. Explain the relationship between packet scheduling and resource allocation.
11. What is admission control? Why is it required in a network offering guaranteed service?
12. Differentiate between traffic shaping, policing, admission control, and packet scheduling.
13. Explain the importance of performance bounds in designing scheduling algorithms.
14. Why might a FIFO scheduler fail to satisfy the QoS requirements of real-time applications?
15. A 12 Mbps outgoing link serves three flows demanding 3 Mbps, 5 Mbps, and 7 Mbps. Explain the resource-allocation problem and discuss how a scheduling policy could address it.

## Section B — Max-Min Fairness (16–30)

16. Define max-min fairness and explain its fundamental property.
17. Explain the water-filling approach used to calculate max-min fair allocations.
18. Differentiate between max-min fairness and equal bandwidth allocation.
19. Under what conditions does max-min fairness allocate equal rates to all flows?
20. Explain why a flow whose demand is fully satisfied should be removed from further water-filling calculations.
21. Calculate the max-min fair allocation for demands of 2, 4, 6, and 8 Mbps on a link with capacity 12 Mbps.
22. Four flows demand 1, 3, 5, and 9 Mbps. Calculate their max-min fair allocations when the total capacity is 10 Mbps.
23. Five flows demand 2, 4, 6, 8, and 10 Mbps. Find the max-min fair allocation for a link of capacity 20 Mbps.
24. Three flows have demands of 2, 7, and 10 Mbps. Calculate the max-min fair allocation when the link capacity is 12 Mbps.
25. Four flows have demands of 3, 3, 6, and 12 Mbps. Find their max-min fair allocation for a 15 Mbps link.
26. Explain weighted max-min fairness and how it differs from unweighted max-min fairness.
27. A link has 18 Mbps capacity. Three backlogged flows have weights 1, 2, and 3, with demands 10, 10, and 10 Mbps. Calculate the weighted max-min fair allocation.
28. Four flows have demands of 4, 6, 10, and 12 Mbps and weights 1, 2, 3, and 4. Find the weighted max-min fair allocation for a link with capacity 24 Mbps.
29. A 20 Mbps link serves three flows with weights 2, 3, and 5 and demands 3, 15, and 20 Mbps. Determine their weighted max-min fair allocations.
30. Prove or justify why max-min fair allocation cannot increase one flow’s rate without decreasing the rate of another flow that already has an equal or smaller allocation, under the standard max-min fairness definition.

## Section C — QoS Metrics, Performance Bounds, and Admission Control (31–45)

31. Define worst-case delay, average delay, and percentile delay. How do they differ?
32. What is the difference between a measured maximum delay and a guaranteed worst-case delay bound?
33. Define packet loss ratio and explain how it is calculated.
34. Explain jitter and describe two methods of measuring it.
35. Why are delay and jitter particularly important for voice and video traffic?
36. A flow transmits 5,000 packets, of which 75 are lost. Calculate the packet loss percentage.
37. A flow experiences packet delays of 10, 15, 20, 25, and 30 ms. Calculate the average delay and the delay range.
38. A network processes 10,000 packets, and 99% of them experience a delay of at most 40 ms. Explain what this percentile statement means.
39. A flow requires 4 Mbps of bandwidth, a maximum delay of 25 ms, and a loss rate below 0.1%. Explain how a network could assess whether to admit this flow.
40. A link has 30 Mbps capacity and currently guarantees 22 Mbps to admitted flows. A new flow requests 6 Mbps. Is sufficient bandwidth available? What other factors should admission control consider?
41. Explain how a scheduler can provide bandwidth guarantees without necessarily providing strict delay guarantees.
42. Differentiate between deterministic and statistical QoS guarantees.
43. Explain how traffic regulation affects the performance guarantees provided by a scheduler.
44. Why can a scheduler fail to provide its expected performance when traffic exceeds its declared traffic profile?
45. Design an admission-control procedure for a network that supports voice, video conferencing, and best-effort traffic.

## Section D — FIFO Scheduling (46–55)

46. Define First-In, First-Out (FIFO) scheduling and explain how it operates.
47. List the advantages and limitations of FIFO scheduling.
48. How does FIFO preserve packet order? Does it guarantee fairness between flows?
49. Explain the head-of-line delay problem in FIFO scheduling.
50. Three packets arrive at times 0, 2, and 3 ms, with transmission durations of 5, 2, and 4 ms, respectively. Determine their start times, completion times, and waiting times under FIFO.
51. Four packets arrive at times 0, 1, 4, and 5 ms, with transmission times of 3, 5, 2, and 1 ms. Calculate their waiting times and average waiting time under FIFO.
52. Two flows share a FIFO output queue. Flow A sends large packets continuously, while Flow B sends small latency-sensitive packets. Explain the impact on Flow B.
53. Can FIFO provide weighted bandwidth sharing between multiple flows? Justify your answer.
54. Compare FIFO scheduling with priority scheduling in terms of implementation, fairness, and delay.
55. A network administrator observes high voice-call latency even though the outgoing link is not overloaded on average. Explain how FIFO queueing and bursty traffic might contribute to this behaviour.

## Section E — Generalized Processor Sharing (GPS) (56–70)

56. Define Generalized Processor Sharing (GPS) and explain its ideal fluid-flow model.
57. Why is GPS considered an ideal benchmark for practical packet scheduling algorithms?
58. Explain how GPS allocates bandwidth among continuously backlogged flows.
59. Write the formula for the GPS service rate of a flow and explain every variable.
60. A 24 Mbps link serves four continuously backlogged flows with equal weights. Calculate the service rate of each flow.
61. A 30 Mbps link serves three continuously backlogged flows with weights 1, 2, and 3. Calculate their GPS service rates.
62. A 20 Mbps link serves four backlogged flows with weights 2, 3, 5, and 10. Determine the rate allocated to each flow under GPS.
63. A 12 Mbps link serves three flows with weights 1, 2, and 3. If the flow with weight 1 becomes idle, calculate the rates allocated to the remaining two backlogged flows.
64. Explain how GPS reallocates available capacity when a flow becomes idle.
65. Why is the ideal fluid model used in GPS difficult to implement with real network packets?
66. Compare GPS with FIFO scheduling.
67. Compare GPS with Weighted Round Robin (WRR).
68. Explain the relationship between GPS and Weighted Fair Queuing (WFQ).
69. A GPS system allocates service according to weights 2 and 4. During a period, both flows remain continuously backlogged. If the second flow receives 8 MB, how much data should the first flow receive over the same period?
70. Discuss the advantages and limitations of GPS as a theoretical scheduling discipline.

## Section F — Fairness Bounds (71–80)

71. What is a fairness bound in packet scheduling?
72. Differentiate between absolute fairness bounds and relative fairness bounds.
73. Explain how a practical packet scheduler can be evaluated against GPS.
74. Write a general expression for an absolute service bound and explain the meaning of its error term.
75. If GPS serves a flow with 20,000 bits during an interval and a scheduler has an absolute error bound of 2,000 bits, what minimum service does the bound guarantee?
76. Two equal-weight flows receive 12,000 and 10,500 bits during the same interval. Calculate their absolute service difference.
77. Two flows have weights 2 and 4 and receive 8,000 and 16,000 bits, respectively. Compare their weight-normalized service.
78. Explain why a small fairness-bound error can be important for short-lived flows.
79. Does satisfying a fairness bound automatically guarantee a maximum packet delay? Explain.
80. Discuss the factors that affect fairness-bound guarantees in practical packet scheduling algorithms.

## Section G — Weighted Round Robin (WRR) (81–95)

81. Define Weighted Round Robin (WRR) scheduling and explain its operation.
82. How are weights translated into packet quotas in WRR?
83. What is the difference between ordinary Round Robin and Weighted Round Robin?
84. Explain why packet-count-based WRR can be unfair when packet sizes differ.
85. Three queues have weights 1, 2, and 3 and contain 5, 7, and 4 packets, respectively. Determine the number of packets served from each queue in the first round and calculate the remaining queue lengths.
86. Three queues have weights 2, 3, and 1 and initial lengths of 7, 5, and 4 packets. Construct the WRR scheduling table until all queues become empty.
87. Four queues have weights 1, 2, 3, and 4. If all queues are continuously backlogged and packets have equal size, determine the proportion of packets allocated to each queue over a complete round.
88. A WRR scheduler has weights 2:3:4. Normalize the weights to integer packet quotas and explain the resulting service pattern.
89. Three flows have weights 0.5, 0.75, and 1. Their average packet sizes are 100, 500, and 1,000 bytes. Explain why packet quotas may need adjustment to achieve weighted byte service.
90. A WRR scheduler serves three queues with quotas 3, 2, and 1. Their packet sizes are 500, 1,000, and 1,500 bytes, respectively. Calculate the total bytes transmitted from each queue in one full round.
91. Explain the limitations of using average packet sizes to adjust WRR quotas.
92. Under what circumstances might WRR provide adequate fairness despite variable packet sizes?
93. Compare WRR and GPS in terms of implementation, packetization, and fairness.
94. Explain how increasing a queue's weight affects its long-term service share under WRR.
95. Design a WRR configuration for four traffic classes—voice, video, web traffic, and bulk transfers—given that voice should receive the highest relative service priority and bulk transfers the lowest. Justify your weight choices.

## Section H — Deficit Round Robin (DRR) (96–110)

96. Define Deficit Round Robin (DRR) and explain why it was developed.
97. Define the quantum and deficit counter in DRR.
98. Describe the operations performed when a DRR scheduler visits a nonempty queue.
99. Why does DRR retain unused deficit when a queue is nonempty?
100. Under what condition is a queue's deficit counter reset to zero?
101. A queue has a quantum of 500 bytes and an initial deficit counter of zero. Its first three packets have sizes 200, 400, and 300 bytes. Determine which packets are transmitted during the first visit and calculate the remaining deficit.
102. Three queues have a quantum of 400 bytes each. Their packet sizes are Q1: 100, 500, 200; Q2: 700, 100; and Q3: 300, 300. Construct a DRR table showing packet transmissions and deficit counters for each visit.
103. A DRR queue has a quantum of 600 bytes and a current deficit of 250 bytes. Its head packet is 800 bytes. Determine whether it can be transmitted during the current visit.
104. Two queues use a quantum of 500 bytes. Q1 contains packets of 300, 300, and 200 bytes, while Q2 contains packets of 700 and 400 bytes. Determine the transmission order over successive DRR rounds.
105. Explain how DRR supports fair service when packets have different sizes.
106. Compare DRR with WRR for a network carrying variable-sized packets.
107. Explain the trade-off between quantum size, scheduling overhead, and burstiness in DRR.
108. A weighted DRR scheduler has flow weights 1:2:4. If the base quantum is 250 bytes, propose suitable quanta and explain the allocation.
109. Discuss the advantages and limitations of DRR in high-speed routers.
110. A DRR queue becomes empty with a positive deficit counter. Explain how the counter should be handled and why.

## Section I — Weighted Fair Queuing (WFQ) (111–125)

111. Define Weighted Fair Queuing (WFQ) and explain its relationship to GPS.
112. What is a virtual finish-time tag in WFQ?
113. Write the packet finish-time formula used in WFQ and explain each term.
114. Why does WFQ account for packet sizes when selecting the next packet?
115. A packet of size 600 bytes belongs to a flow with weight 3. Its previous finish tag is 100 and its arrival virtual time is 150. Calculate its finish-time tag.
116. Flow A has weight 1 and packets of 400 and 300 bytes. Flow B has weight 2 and packets of 800 and 200 bytes. Assuming all packets arrive at virtual time zero, calculate all finish-time tags.
117. Three flows have weights 1, 2, and 4. Their first packets have sizes 300, 600, and 800 bytes. Assuming a virtual arrival time of zero, calculate their finish tags and determine the first packet selected.
118. Flow A has packets of 200 and 500 bytes with weight 2. Flow B has packets of 300 and 400 bytes with weight 1. Calculate their finish tags and derive a scheduling order, assuming all packets arrive at virtual time zero.
119. Explain why only the eligible head-of-line packet from each flow is considered when selecting the next WFQ packet.
120. What happens when two packets have identical virtual finish-time tags? Explain how a tie-breaking rule can be applied.
121. Explain the role of virtual time in WFQ when packets arrive at different times.
122. Discuss the computational overhead of WFQ compared with FIFO and WRR.
123. Compare WFQ and DRR in terms of fairness, complexity, and treatment of packet sizes.
124. Explain why WFQ is an approximation to GPS rather than an exact implementation of fluid-flow sharing.
125. Discuss the conditions under which WFQ can support bandwidth or delay guarantees.

## Section J — Comparative, Analytical, and Scenario-Based Questions (126–140)

126. Compare FIFO, WRR, DRR, and WFQ with respect to fairness, complexity, packet-size awareness, and QoS support.
127. Which scheduling discipline would you recommend for a router carrying packets of widely varying sizes? Justify your choice.
128. A router must provide differentiated bandwidth shares to multiple traffic classes while keeping implementation overhead low. Which scheduling discipline would you consider, and why?
129. A network carries voice, video, interactive gaming, and bulk file transfers. Propose a scheduling strategy and justify how it addresses the different traffic requirements.
130. Explain why no single scheduling algorithm is necessarily optimal for every networking environment.
131. A router uses FIFO, and large file-transfer packets cause delays for interactive traffic. Compare two alternative scheduling disciplines that could address this problem.
132. A service provider must allocate bandwidth proportionally to customer subscription weights. Explain how weighted scheduling could be implemented.
133. A router carries both small packets and large packets. Compare the fairness characteristics of WRR, DRR, and WFQ in this environment.
134. Explain the relationship between max-min fairness, GPS, and practical fair-queueing algorithms.
135. A network administrator observes that one flow receives more bytes than another despite both queues receiving the same number of transmission opportunities. Explain the possible causes.
136. A high-speed router has thousands of active flows. Compare the implementation challenges of WFQ and DRR.
137. A flow requires a strict delay bound, while another only requires a minimum bandwidth share. Explain how scheduling and admission control might jointly address these requirements.
138. A network has sufficient average bandwidth, but packets occasionally experience very high delays. Analyze possible causes involving queueing, bursty traffic, and scheduling discipline.
139. A router must support weighted fairness, variable packet sizes, and low scheduling overhead. Evaluate whether DRR is an appropriate choice and discuss its limitations.
140. A scheduling algorithm is fair over long time intervals but produces large short-term bursts. Explain why long-term fairness alone may not be sufficient for real-time applications.

## Section K — Advanced Numericals and Design Problems (141–150)

141. A 25 Mbps link serves five flows demanding 2, 4, 7, 8, and 12 Mbps. Calculate the unweighted max-min fair allocation and identify any unsatisfied demands.
142. A 30 Mbps link serves four flows with weights 1, 2, 3, and 4 and demands of 3, 8, 20, and 20 Mbps. Calculate the weighted max-min fair allocation using water-filling.
143. A FIFO queue contains four packets with arrival times 0, 2, 3, and 6 ms and transmission times 4, 3, 5, and 2 ms. Calculate start times, finish times, waiting times, and average waiting time.
144. A 36 Mbps link serves three continuously backlogged GPS flows with weights 2, 3, and 7. Calculate their service rates. If the flow with weight 7 becomes idle, recalculate the rates of the remaining flows.
145. Three WRR queues have weights 3:2:1 and initial lengths of 8, 5, and 4 packets. Construct a complete scheduling table showing every round until all queues become empty.
146. A DRR scheduler uses a quantum of 500 bytes for each queue. The packet sizes are Q1: 150, 450, 350; Q2: 700, 200, 300; and Q3: 250, 250, 500. Prepare a round-by-round deficit-counter table and determine the packet transmission order.
147. A weighted DRR system has weights 1:3:5 and uses a base quantum of 200 bytes. Calculate suitable quanta and explain how the weights influence service when all queues remain backlogged.
148. Three WFQ flows have weights 1, 2, and 3. Their packets, in arrival order within each flow, have sizes \\(400,200\\), \\(600,300\\), and \\(900,300\\) bytes, respectively. Assuming all packets arrive at virtual time zero, calculate their finish tags and determine the packet scheduling order.
149. A practical scheduler provides a flow with 18,000 bits of service, while GPS would provide 20,000 bits during the same interval. Calculate the absolute service difference. If the scheduler's maximum absolute error bound is 2,500 bits, determine whether the observed service satisfies the bound.
150. Design a packet scheduling system for a network with guaranteed-service traffic and best-effort traffic. Specify the scheduling discipline, queue organization, weight or quantum selection, admission-control policy, and QoS metrics you would monitor. Justify your design decisions.

Suggested exam-preparation order: First solve Questions 21–30 on max-min fairness, then Questions 50–51 on FIFO, Questions 60–69 on GPS, Questions 85–90 on WRR, Questions 101–110 on DRR, and Questions 115–120 on WFQ. These give you focused practice across the main numerical techniques before attempting the advanced problems.