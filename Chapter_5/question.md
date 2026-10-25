# ACN-TH

**User:** Anonymous  
**Created:** 9/19/2026 11:22:08  
**Updated:** 10/25/2026 17:32:20  
**Exported:** 10/25/2026 17:45:23  
**Link:** [https://chatgpt.com/c/6aae2307-7b8c-83e9-87c1-09040396fbf2](https://chatgpt.com/c/6aae2307-7b8c-83e9-87c1-09040396fbf2)  

## Response:
10/25/2026, 5:31:09 PM · gpt-6

# IP Address Lookup — Complete Question Bank

Advanced Computer Networks (CS 606T)

Based on your professor's uploaded **IP Address Lookup** slides, this question bank covers the full chapter: forwarding functions, classful addressing and CIDR, longest prefix match, binary tries, multibit tries, prefix expansion, variable-stride tries, and level-compressed tries.

I've organized the questions into theory, numerical problems, algorithms, and exam-style scenarios so you can prepare for both conceptual and problem-solving questions.

## Section 1: Basic concepts and forwarding functions

1. What is the forwarding function of a router?
2. What is IP address lookup, and why is it required?
3. What is a forwarding table?
4. Explain the role of the destination IP address during packet forwarding.
5. Differentiate between an ingress interface and an egress interface.
6. What information can a forwarding table contain?
7. Explain the steps performed by a router when a packet arrives.
8. Why is fast IP address lookup important in high-speed networks?
9. What is the relationship between address lookup and packet forwarding?
10. Explain the difference between routing and forwarding.
11. Why does a router need to select an outgoing interface?
12. What happens when multiple forwarding-table entries match a destination address?

## Section 2: Classful addressing and CIDR

13. Explain IPv4 classful addressing.
14. Describe IPv4 address Classes A, B, C, D, and E.
15. What are the default network masks for Classes A, B, and C?
16. Differentiate between network bits and host bits.
17. What are the limitations of classful addressing?
18. What is Classless Inter-Domain Routing (CIDR)?
19. Explain CIDR notation with an example.
20. How many host bits are present in an IPv4 prefix `/p`?
21. How many total addresses are represented by a `/24` prefix?
22. How many total addresses are represented by a `/26` prefix?
23. Explain why CIDR reduces address wastage compared with classful addressing.
24. Divide a `/24` network into four equal-sized subnets. Determine the prefix length and the number of addresses per subnet.
25. Explain the relationship between prefix length and address-block size.
26. Compare classful addressing and CIDR.
27. What is the significance of a `/16` prefix compared with a `/24` prefix?
28. Given an IPv4 address and a prefix length, determine the number of host bits and total addresses in the block.

## Section 3: Longest Prefix Match (LPM)

29. Define longest prefix match.
30. Why does a router use LPM instead of selecting the first matching entry?
31. How does prefix specificity affect the forwarding decision?
32. Explain how a `/24` route can take precedence over a matching `/22` route.
33. Can two forwarding-table entries match the same destination address? Explain.
34. What happens when a destination matches only a default route, if one is present?
35. Why is LPM important when using CIDR?
36. Explain the difference between a matching prefix and the longest matching prefix.
37. What factors affect the speed of an LPM lookup?
38. Explain LPM using a forwarding-table example.

### Numerical problem 1: Select the outgoing interface

A router has the following forwarding table\:

| Destination prefix | Next hop |
|--------|--------|
| `192.2.0.0/22` | R3 |
| `192.2.2.0/24` | R2 |
| `192.2.2.128/25` | R4 |
| `10.0.0.0/8` | R5 |

Determine the selected next hop for each destination\:

a. `192.2.1.10`

b. `192.2.2.20`

c. `192.2.2.200`

d. `10.1.2.3`

Explain each answer using LPM.

### Numerical problem 2: Forwarding-table construction

A router receives packets destined for the following addresses\:

- `172.16.1.10`
- `172.16.2.10`
- `172.16.3.10`

Its forwarding table contains `172.16.0.0/16`, `172.16.0.0/22`, and `172.16.2.0/24`\.

For each destination, identify every matching entry and determine which entry wins under LPM.

## Section 4: Binary tries

39. What is a binary trie?
40. How are the 0 and 1 bits represented as branches in a binary trie?
41. What information is stored at a valid prefix node?
42. Explain how a destination address is searched in a binary trie.
43. How does a binary trie find the longest matching prefix?
44. What happens when the search reaches a node without the required child?
45. Explain prefix insertion in a binary trie.
46. Explain prefix deletion in a binary trie.
47. Why should a node not be deleted if it still has descendants?
48. What is path compression?
49. Why is path compression useful in sparse binary tries?
50. Does path compression change the worst-case lookup complexity? Explain.
51. State the search, insertion, and deletion complexities of a binary trie.
52. Explain the space complexity $O(NW)$ of a binary trie.
53. Compare an ordinary binary trie with a path-compressed trie.
54. What is the role of intermediate nodes that do not store forwarding information?
55. Explain how IPv4 and IPv6 address widths affect trie lookup.
56. Describe the advantages and limitations of binary tries for IP lookup.

### Numerical problem 3: Construct a binary trie

Construct a binary trie for the following prefixes\:

- `0*` → R1
- `10*` → R2
- `101*` → R3
- `110*` → R4
- `111*` → R5

Then perform LPM searches for\:

a. `0011`

b. `1011`

c. `1001`

d. `1100`

e. `1110`

Show the path traversed and the best matching prefix at each step.

### Numerical problem 4: Trie updates

A binary trie contains the prefixes `10*`, `101*`, and `1011*`\.

1. Insert `100*`\.
2. Delete `10*`\.
3. Explain why deleting `10*` must not remove the nodes required for `101*` and `1011*`\.
4. State the worst-case time complexity of each operation.

## Section 5: Multibit tries

57. What is a multibit trie?
58. Why are multibit tries introduced?
59. Define stride in a multibit trie.
60. If the stride is $k$, how many children can a node have?
61. How does increasing the stride affect trie height?
62. Why can a larger stride increase memory consumption?
63. Explain how multibit tries reduce memory accesses.
64. Compare binary tries and multibit tries.
65. What is prefix transformation?
66. Why are prefix transformations needed in multibit tries?
67. What is prefix expansion?
68. Expand `0*` into prefixes of length 2 and length 3.
69. Explain why an expanded prefix must preserve LPM behavior.
70. What is a fixed-stride multibit trie?
71. What is the difference between fixed-stride and variable-stride multibit tries?
72. Explain how searching works in a fixed-stride multibit trie.
73. Explain how insertion works when a prefix ends partway through a stride.
74. Describe the array-based implementation of a multibit trie.
75. What are the `nhop` and `ptr` fields used for?
76. Explain the search-speed versus memory-consumption trade-off.
77. State the search complexity of a fixed-stride multibit trie.
78. State the update and space complexities given in the slides.
79. Why does the storage requirement increase exponentially with stride size?
80. What design objective does the professor's slide suggest for choosing the number of trie levels?

### Numerical problem 5: Stride and memory accesses

Assume IPv4 addresses are 32 bits long.

Calculate the number of levels required for fixed strides of\:

a. 1 bit

b. 2 bits

c. 4 bits

d. 8 bits

For each case, calculate the maximum number of children per node and explain the memory trade-off.

### Numerical problem 6: Prefix expansion

A multibit trie uses a stride of 3 bits at its first level. Consider the prefix `0*`\.

1. Expand the prefix to length 3.
2. List all expanded prefixes.
3. Explain why these prefixes cover the same address range as the original prefix.
4. Suppose one expanded prefix overlaps with an existing, more-specific prefix. Which prefix should determine forwarding?

### Numerical problem 7: Search by chunks

A fixed-stride multibit trie uses strides of 3 bits followed by 2 bits. Search for the destination bit string\:

`110011`

1. Divide the destination into chunks according to the strides.
2. Explain how each chunk determines the next branch.
3. Explain how the best matching prefix is maintained.
4. What happens if the next branch does not exist?

The final next hop depends on the forwarding entries in the trie, so you should use a supplied trie diagram to calculate it.

### Numerical problem 8: Insert `1100*`

A fixed-stride multibit trie uses strides of 3 bits and 2 bits.

Insert prefix `1100*` by answering the following\:

1. Divide the prefix according to the first-level stride.
2. Identify the remaining incomplete chunk.
3. Expand that chunk to the second-level stride.
4. Determine how many child entries the second-level node requires.
5. Identify which child entries receive the prefix's forwarding information.

## Section 6: Variable-stride tries and LC tries

81. Define a variable-stride multibit trie.
82. Why might variable strides be useful?
83. What is the problem with using prefix expansion without compression?
84. What is the main objective of compressing multibit tries?
85. Define a Level-Compressed (LC) trie.
86. Why do LC tries combine path compression and level compression?
87. Differentiate between path compression and level compression.
88. What is a skip value in an LC trie?
89. Why must an LC trie store information about skipped bits?
90. Explain the two steps involved in constructing an LC trie, as described in the slides.
91. How does level compression reduce trie height?
92. Why can prefix expansion increase storage requirements?
93. How can compression improve lookup performance?
94. Explain the potential benefit of fitting a trie into the L1 cache.
95. Compare binary tries, fixed-stride multibit tries, variable-stride tries, and LC tries.
96. What is the primary design trade-off when choosing the stride sizes and compression strategy?

## Section 7: Algorithm and complexity questions

97. Write pseudocode for searching a binary trie using longest prefix match.
98. Write pseudocode for inserting a prefix into a binary trie.
99. Write pseudocode for deleting a prefix without removing necessary descendants.
100. Explain why the worst-case binary-trie search complexity is $O(W)$\.
101. Derive the approximate search complexity $O(W/k)$ for a fixed-stride multibit trie.
102. Explain the update complexity $O(W/k+2^k)$ given in the slides.
103. Explain the space complexity $O(2^kNW/k)$ given in the slides.
104. Compare the memory-access requirements of binary and multibit tries.
105. Explain how path compression changes the number of nodes and why worst-case lookup can still be $O(W)$\.
106. Explain how stride size affects the number of levels, branching factor, and memory consumption.

## Section 8: Scenario-based questions

107. A router must perform fast IPv4 lookups at high aggregate link speeds. Explain why a binary trie might be insufficient and how a multibit trie can help.
108. A router's forwarding table contains overlapping prefixes. Explain how LPM determines the correct next hop.
109. A trie has many long paths with only one child per node. Which compression technique is suitable, and why?
110. A trie has dense regions containing many nodes at successive levels. Which compression technique is relevant?
111. A fixed-stride trie has a small height but consumes too much memory. Explain the cause and suggest a design direction based on the slides.
112. A prefix ends after only one bit, but the next trie level uses a stride of 3 bits. Explain how prefix expansion helps.
113. During deletion, a prefix node has descendants corresponding to more-specific prefixes. Explain how deletion should proceed.
114. A designer wants at most three levels while minimizing storage. Explain the competing factors involved in selecting strides.
115. A multibit trie uses arrays of different sizes at different levels. Explain why this is expected in a variable-stride design.
116. An LC trie skips intermediate nodes. Explain what information is needed to continue the search correctly.
117. Two forwarding entries match a destination, but one is more specific. Explain the forwarding decision.
118. A compressed trie uses less memory but still performs a long lookup. Explain why path compression does not necessarily eliminate the worst-case $O(W)$ bound.

## Section 9: Long-answer and comparison questions

119. Explain IP address lookup, from packet arrival to next-hop selection.
120. Explain classful addressing, CIDR, and the motivation for flexible prefix lengths.
121. Explain longest prefix match with a suitable forwarding-table example.
122. Describe the structure, search, insertion, deletion, and complexity of binary tries.
123. Explain path compression in binary tries, including its advantages and limitations.
124. Describe the structure and operation of fixed-stride multibit tries.
125. Explain prefix expansion and why forwarding information must preserve LPM.
126. Discuss the time and space trade-offs of multibit tries.
127. Compare fixed-stride and variable-stride multibit tries.
128. Explain the design and motivation of LC tries.
129. Discuss how path compression and level compression work together in LC tries.
130. Compare the main trie structures for IP address lookup and explain when their respective advantages matter.

---

## Suggested preparation order

If you're preparing for an exam, I recommend working through these in this order\:

1. **First priority\:** Questions 13–38 — CIDR and LPM numericals.
2. **Second priority\:** Questions 39–56 and 97–100 — binary-trie construction, search, insertion, deletion, and complexity.
3. **Third priority\:** Questions 57–80 and numerical problems 5–8 — multibit tries, strides, prefix expansion, and complexity.
4. **Fourth priority\:** Questions 81–96 and 107–118 — variable strides, LC tries, and scenario-based reasoning.
5. **Final revision\:** Questions 119–130 — full-length theory and comparison answers.

This gives you **130 questions** covering the chapter. The detailed numerical problems that require a particular trie diagram can be solved completely when that diagram or its forwarding entries are supplied.



---
Powered by [ChatGPT Exporter](https://www.chatgptexporter.com)