# ACN-TH

**User:** Anonymous  
**Created:** 9/19/2026 11:22:08  
**Updated:** 10/25/2026 17:32:20  
**Exported:** 10/25/2026 17:46:17  
**Link:** [https://chatgpt.com/c/6aae2307-7b8c-83e9-87c1-09040396fbf2](https://chatgpt.com/c/6aae2307-7b8c-83e9-87c1-09040396fbf2)  

## Response:
10/25/2026, 5:24:34 PM · gpt-6

> **Reviewed file contents**
>
> 
>
> Worked for 6s

I checked your 36-page PDF, **IP Address Lookup**, and we’ll continue with this chapter in the same step-by-step, exam-oriented style we used for Packet Scheduling. I’ll follow your professor’s slides, including their diagrams, examples, algorithms, and complexity analysis.

The chapter covers these main topics\:

| Slide pages | Topics |
|--------|--------|
| 2–10 | Forwarding function, classful addressing, CIDR, and longest prefix matching |
| 11–18 | Binary tries, search and updates, path compression, and complexity |
| 19–30 | Multibit tries, prefix expansion, fixed strides, operations, and performance |
| 31–36 | Variable-stride multibit tries and Level Compressed (LC) tries |

The key theme is **how a router finds the correct forwarding-table entry quickly**, even when multiple prefixes match a destination address. We’ll start with the forwarding function and build toward the trie-based lookup algorithms.

## Topic 1 — Forwarding Function and Address Lookup

*Based on slides 2–3 of your PDF.*

### 1\. What is packet forwarding?

A router receives a packet on an incoming interface and must decide where to send it next so that it moves toward its final destination.

The forwarding function must determine two things\:

1. **Next-hop router\:** Which router should receive the packet next?
2. **Egress interface\:** Through which outgoing interface should the packet be transmitted?

The incoming interface is called the **ingress interface**, while the outgoing interface is called the **egress interface**\.

### 2\. What is a forwarding table?

A forwarding table stores information that a router uses to determine the next hop for a packet.

Your professor's slide on page 2 shows a table with three important fields\:

| Entry number | Prefix | Next hop / interface |
|--------|--------|--------|
| 1 | `98.1.1.1/24` | `eth3` |
| 2 | `171.1.0.0/16` | `so6` |
| 3 | `171.1.1.0/24` | `fe5` |

Here, the prefix identifies a destination address or address range, and the next-hop information identifies where matching packets should be forwarded.

*Note\:* The first row is reproduced as displayed in the slide. In a conventional network-prefix table, a `/24` network prefix is normally written with its host bits set to zero, such as `98.1.1.0/24`\.

### 3\. What is an address lookup?

**Address lookup** is the operation of consulting the forwarding table using a packet's destination IP address as the search key.

The basic process is\:

**1\. Packet arrives**

Router receives packet on ingress interface

**2\. Read destination IP**

Use destination address as the lookup key

**3\. Search forwarding table**

Find the matching prefix and next-hop information

**4\. Forward packet**

Transmit through the selected egress interface

### 4\. Worked example

Suppose a router has these forwarding entries\:

| Destination prefix | Egress interface |
|--------|--------|
| `10.1.0.0/16` | `eth0` |
| `10.2.0.0/16` | `eth1` |
| `192.168.1.0/24` | `eth2` |

Determine the outgoing interface for each destination\:

- Destination `10.1.5.8` matches `10.1.0.0/16`, so it uses `eth0`\.
- Destination `10.2.4.7` matches `10.2.0.0/16`, so it uses `eth1`\.
- Destination `192.168.1.20` matches `192.168.1.0/24`, so it uses `eth2`\.

The router does not need to know the entire path to the final destination at this stage. It uses its own forwarding table to select the next step.

### 5\. Why is IP address lookup a problem?

In a simple network, finding the matching entry may be straightforward. But modern forwarding tables contain many prefixes, and a destination may match multiple entries.

For example, a destination might match both `10.0.0.0/8` and `10.1.0.0/16`\. The router needs a rule to decide which entry to use.

This leads to the central problem of the chapter\:

**How can a router find the most appropriate matching prefix as quickly as possible?**

Your professor introduces the impact of addressing schemes and then longest prefix matching to answer this question.

## Exam questions — Topic 1

1. Define the forwarding function of a router.
2. Differentiate between ingress and egress interfaces.
3. What is a forwarding table? Explain its fields.
4. Define IP address lookup.
5. Explain the steps involved in forwarding a packet using a forwarding table.
6. What is the difference between routing and forwarding?
7. Why must address lookup be performed efficiently in a high-speed router?
8. Given a forwarding table and destination IP addresses, determine the corresponding egress interfaces.
9. Explain why multiple matching prefixes can make address lookup more complex.
10. Draw and explain the forwarding-function workflow.

Next, we’ll cover **classful addressing versus classless addressing (CIDR)**, followed by the longest prefix match rule, which is essential for understanding the trie algorithms later in the PDF.

## Response:
10/25/2026, 5:26:05 PM · gpt-6

# Topic 2 — Classful Addressing vs. CIDR

*Based on slides 4–9 of your professor’s IP Address Lookup PDF.*

This topic explains how IP addresses are divided into network and host portions, why classful addressing became inefficient, and how CIDR makes forwarding more flexible—but also makes address lookup more complex.

## 1\. Classful addressing

In a classful addressing scheme, IPv4 addresses are divided into predefined classes. The class determines how many bits belong to the network portion and how many belong to the host portion.

Your professor’s slide 4 illustrates the five classes: A, B, C, D, and E.

**A**

**Class A**

Network: 8 bits · Host: 24 bits

First-octet range: 1–126 in conventional unicast usage

---

**B**

**Class B**

Network: 16 bits · Host: 16 bits

First-octet range: 128–191

---

**C**

**Class C**

Network: 24 bits · Host: 8 bits

First-octet range: 192–223

---

**D**

**Class D**

Multicast addressing

First-octet range: 224–239

---

**E**

**Class E**

Reserved / experimental

First-octet range: 240–255

*Note\:* The slide gives the class ranges. The Class A range above describes conventional unicast use; the special-purpose addresses `0.x.x.x` and `127.x.x.x` have separate uses.

### Default masks

| Class | Default prefix | Default subnet mask |
|--------|--------|--------|
| A | `/8` | `255.0.0.0` |
| B | `/16` | `255.255.0.0` |
| C | `/24` | `255.255.255.0` |

Class D and Class E are not ordinary classful unicast network classes.

### How does classful forwarding work?

With classful addressing, the network portion is determined by the address class. A router can use that predefined network portion to select the forwarding entry for the destination network.

For example, `172.16.5.10` belongs to Class B under the historical classful scheme. Its default network prefix is `172.16.0.0/16`\.

The forwarding table can therefore store one network entry for the destination network instead of separate entries for every host attached to it.

## 2\. Limitations of classful addressing

The main problem was inefficient address allocation.

Consider an organization that needs 500 host addresses\:

- A Class C network provides only 8 host bits, or 256 total address combinations—insufficient for 500 hosts.
- A Class B network provides 16 host bits, or 65,536 total address combinations—far more than needed.

Under the default classful allocation, the organization might receive a much larger block than necessary.

This contributes to address-space wastage and helped motivate **Classless Inter-Domain Routing (CIDR)**\.

## 3\. Classless Inter-Domain Routing (CIDR)

Your professor introduces CIDR on slide 5 as a response to inefficient use and depletion of the IP address space.

CIDR removes the restriction that network prefixes must have fixed lengths of 8, 16, or 24 bits. Instead, the network prefix can have a flexible length.

An IPv4 address has 32 bits\:

$$
\underbrace{\text{Network prefix}}_{\text{first }p\text{ bits}}
\quad
\underbrace{\text{Host identifier}}_{\text{remaining }32-p\text{ bits}}
$$

The CIDR notation is\:

`IP address/prefix length`

For example, `198.51.100.0/24` means that the first 24 bits identify the network, leaving 8 bits for host addresses.

The number of addresses in a prefix is\:

$$
\boxed{2^{32-p}}
$$

where $p$ is the prefix length.

### Examples

| CIDR prefix | Network bits | Host bits | Total addresses |
|--------|--------|--------|--------|
| `10.0.0.0/8` | 8 | 24 | 16,777,216 |
| `172.16.0.0/16` | 16 | 16 | 65,536 |
| `192.168.1.0/24` | 24 | 8 | 256 |
| `192.168.1.0/26` | 26 | 6 | 64 |
| `192.168.1.0/28` | 28 | 4 | 16 |

These are total address counts, including network and broadcast addresses in ordinary IPv4 subnets. Usable host counts are usually two fewer for conventional subnets, with exceptions such as `/31` point-to-point links.

### Worked example

**Question\:** How many addresses are in the network `192.168.10.0/27`?

Step 1: Find the host bits.

$$
32-27=5
$$

Step 2: Calculate the total number of addresses.

$$
2^5=\boxed{32}
$$

So this prefix contains 32 addresses.

## 4\. Subnetting and prefix flexibility

Your professor's slide 5 illustrates how subnetting divides the host identifier into a subnet identifier and a smaller host identifier.

For example, starting with a `/24` network and extending the prefix to `/26` borrows two bits from the original host portion.

$$
2^2=4\text{ subnets}
$$

Each `/26` subnet contains\:

$$
2^{32-26}=64\text{ addresses}
$$

This allows an address block to be divided into smaller networks according to requirements.

## 5\. Why CIDR makes lookup more complex

Slides 7–9 provide the important forwarding-table example.

Suppose router R1 initially has this entry\:

| Prefix | Next hop |
|--------|--------|
| `192.2.0.0/22` | R3 |

Later, a smaller network, `192.2.2.0/24`, moves to a different path through R2. Instead of replacing the entire `/22` route, R1 can add a more specific entry\:

| Prefix | Next hop |
|--------|--------|
| `192.2.0.0/22` | R3 |
| `192.2.2.0/24` | R2 |

Now consider destination `192.2.2.10`\.

It matches both prefixes\:

- `192.2.0.0/22`
- `192.2.2.0/24`

Which next hop should R1 choose?

The answer is **Longest Prefix Match (LPM)**\.

## 6\. Longest Prefix Match (LPM)

Longest Prefix Match selects the matching forwarding-table entry with the greatest prefix length.

In other words, when multiple entries match a destination, choose the most specific matching prefix.

For destination `192.2.2.10`\:

| Matching prefix | Prefix length | Next hop |
|--------|--------|--------|
| `192.2.0.0/22` | 22 | R3 |
| `192.2.2.0/24` | 24 | R2 |

Since $24>22$, the router chooses\:

$$
\boxed{\text{Next hop}=R2}
$$

The `/24` entry wins because it is more specific than the `/22` entry.

### Interactive practice

**Forwarding-table lookup**

Choose a destination address to see which route is selected.

192.2.1.10

192.2.2.10

192.2.3.10

---

## 7\. Why do we need efficient lookup algorithms?

Your professor emphasizes on slide 10 that longest prefix matching must be performed very quickly because routers process packets under strict time constraints.

A straightforward scan of every forwarding-table entry can be expensive when the table is large. The next slides introduce data structures that make this operation more efficient\:

- Binary tries
- Path-compressed tries
- Multibit tries
- Level Compressed (LC) tries

These structures organize prefixes to accelerate the search.

## Exam questions for Topic 2

1. Explain classful addressing and describe Classes A, B, C, D, and E.
2. Compare classful addressing with CIDR.
3. Why was CIDR introduced?
4. Explain CIDR notation using an example.
5. Calculate the number of addresses in a `/20`, `/26`, and `/29` IPv4 prefix.
6. Explain how subnetting divides an IP address into network, subnet, and host portions.
7. Why does CIDR make forwarding-table lookup more complex?
8. Define Longest Prefix Match.
9. Given overlapping prefixes and a destination IP address, determine the correct next hop.
10. Explain the forwarding-table update example on slides 7–9.
11. Why is a simple exact-match search insufficient when forwarding entries overlap?
12. Why must IP address lookup be optimized for high-speed routers?

**Next topic\:** Binary tries — how prefixes are represented as paths of 0s and 1s, how longest-prefix searches work, and how insertion and deletion are performed.

## Response:
10/25/2026, 5:27:16 PM · gpt-6

> **Reviewed file content and analyzed relevant sections**
>
> 
>
> Worked for 11s

# Topic 3 — Binary Tries for IP Address Lookup

*Based on your professor’s PDF, starting from slide 11 (“Tries”).*

The previous topic introduced **Longest Prefix Match (LPM)**\. Now we’ll study a data structure that helps routers find matching prefixes efficiently: the **binary trie**\.

## 1\. What is a binary trie?

A binary trie is a tree structure used to organize IP prefixes. Each branch represents one bit of a prefix\:

- Left branch: `0`
- Right branch: `1`
- Each internal node can have zero, one, or two children.
- A path from the root represents a sequence of bits.

Instead of comparing an entire IP address against every forwarding-table entry, a trie follows the destination address one bit at a time.

### Example

Suppose our forwarding table contains these prefixes\:

| Prefix | Next hop |
|--------|--------|
| `0.0.0.0/0` | R0 |
| `128.0.0.0/1` | R1 |
| `192.0.0.0/2` | R2 |
| `224.0.0.0/3` | R3 |

For illustration, we’ll use only the first few bits of IPv4 addresses. Each prefix corresponds to a path from the root.

### Binary trie structure

Root

0

`0`

Prefix `0/1`

1

`1`

Prefix `1/1`

Each additional bit moves one level deeper into the tree.

The diagram illustrates the basic branching rule. In an actual trie, longer prefixes are represented by paths with more levels, and forwarding information is stored at nodes corresponding to valid prefixes.

## 2\. How does a binary trie represent prefixes?

According to slide 12, nodes highlighted in gray represent actual prefixes. These nodes contain next-hop information or a pointer to it.

Consider these simplified prefixes\:

- `0/1` → R1
- `10/2` → R2
- `101/3` → R3

Their trie looks like this\:

Root

├── 0

**`0` → R1**

└── 1

`1`

└── 0

**`10` → R2**

└── 1

**`101` → R3**

Simplified prefix paths; labels indicate the stored forwarding entries.

The key idea is that **not every node represents a valid forwarding prefix**\. Some nodes exist only to connect paths, while other nodes store valid prefix information.

## 3\. Searching a binary trie — Longest Prefix Match

This is the most important operation in slides 13–15.

The search procedure follows the destination address bit by bit, starting at the root.

### Algorithm

1. Start at the root node.
2. Initialize the best match to the default route, if one exists.
3. Inspect the next destination-address bit.
4. Follow the `0` branch for bit `0` or the `1` branch for bit `1`\.
5. Whenever a node contains valid prefix information, remember it as the current best match.
6. Continue until the next branch does not exist or the address bits are exhausted.
7. Return the last remembered matching prefix.

Why remember previous matches? Because the search may reach a longer path that does not correspond to a valid prefix.

### Worked example

Use the simplified trie from the previous section\:

| Prefix | Next hop |
|--------|--------|
| `0/1` | R1 |
| `10/2` | R2 |
| `101/3` | R3 |

Suppose the destination begins with `1011`\.

- Read the first bit `1`\: follow the right branch. No valid prefix is stored there.
- Read the second bit `0`\: reach prefix `10`, so remember R2.
- Read the third bit `1`\: reach prefix `101`, so update the best match to R3.
- Continue with the fourth bit `1`\. If no branch exists, terminate.

The longest matching prefix is `101/3`, so the lookup returns **R3**\.

## 4\. Insertion and deletion

Slides 14–15 cover updates to the trie.

### Insertion

To insert a new prefix\:

1. Start at the root.
2. Follow the prefix bits one by one.
3. If the required branch does not exist, create the missing nodes.
4. Mark the final prefix node as valid and store its next-hop information.

If the path already exists, the router may only need to mark the final node as a valid prefix or update its forwarding information.

### Deletion

To delete a prefix\:

1. Follow the prefix bits to its corresponding node.
2. Remove its valid-prefix marker and forwarding information.
3. Remove unnecessary nodes that no longer represent a prefix and have no remaining descendants.
4. Preserve nodes that are still needed by other prefixes.

For example, deleting prefix `10/2` must not remove the path to `101/3`, because that longer prefix still exists.

## 5\. Complexity analysis

Your professor gives the following worst-case bounds on slide 15.

Let\:

- $N$ \= number of prefixes
- $W$ \= maximum prefix length in bits

For IPv4, $W=32$; for IPv6, $W=128$\.

| Operation | Worst-case time | Worst-case space |
|--------|--------|--------|
| Search | $O(W)$ | $O(NW)$ for the trie structure |
| Insert | $O(W)$ | Up to $O(W)$ additional nodes |
| Delete | $O(W)$ | May reclaim nodes that are no longer needed |

The $O(NW)$ space bound is an upper bound: prefixes often share paths, so actual memory use can be lower.

**Important exam point\:** Binary-trie search is $O(W)$, rather than $O(N)$ for a straightforward linear scan through $N$ forwarding entries. Since $W$ is fixed for a given IP address family, this gives a useful bound on lookup work.

## 6\. Path compression

Slides 16–17 introduce path compression to reduce memory use and unnecessary traversal.

In an ordinary binary trie, a chain of nodes with only one child can require examining several bits one at a time. Path compression skips such single-child portions of a path.

### Before compression

Root

↓ 1

Node 1

↓ 0

Node 2

↓ 1

Node 3

Several single-child nodes

---

### After compression

Root

↓ examine bits at stored positions

**Compressed path**

Skip intermediate single-child nodes

The compressed trie stores additional information identifying the next bit position to inspect. That lets the search skip unnecessary intermediate nodes.

**Complexity note\:** The professor states that worst-case search in a path-compressed trie remains $O(W)$\. Path compression can reduce the number of nodes visited and memory used, but it does not change this worst-case asymptotic bound.

## 7\. Questions for this topic

1. Define a binary trie and explain its node structure.
2. How are IP prefixes represented in a binary trie?
3. Explain the purpose of valid-prefix nodes.
4. Describe the longest-prefix-match search procedure in a binary trie.
5. Why must a trie search remember the last valid prefix encountered?
6. Construct a binary trie for the prefixes `0/1`, `10/2`, `110/3`, and `111/3`\.
7. Given a trie and a destination bit string, identify the longest matching prefix.
8. Explain the insertion and deletion operations in a binary trie.
9. Why can deletion not simply remove every node on the deleted prefix's path?
10. Derive the worst-case search, insertion, and deletion complexities.
11. Explain the $O(NW)$ space bound and why it may not be tight.
12. What is path compression, and why is it used?
13. What additional information is required in a path-compressed trie?
14. Does path compression change the worst-case search complexity? Explain.
15. Compare an ordinary binary trie and a path-compressed trie in terms of traversal and memory.

**Next topic in your PDF\:** Multibit tries, including fixed-stride lookup, prefix expansion, and the trade-off between trie depth and memory consumption.

## Response:
10/25/2026, 5:28:32 PM · gpt-6

> **Analyzed file lines 150–260 and completed search complexity analysis**
>
> 
>
> Worked for 19s

# Topic 4: Multibit Tries

Advanced Computer Networks — IP Address Lookup &#124; Based on slides 18–29

## 1\. Why do we need multibit tries?

In a **binary trie**, the router examines one bit of the destination IP address at each level. For a 32-bit IPv4 address, a lookup can require up to 32 memory accesses in the worst case.

Your professor's slides illustrate the performance problem\:

- Assume one memory access takes 10 nanoseconds.
- Worst-case lookup time: $32 \times 10 = 320$ nanoseconds.
- Maximum forwarding rate under this assumption: approximately $3.125$ million packets per second.
- At 40 bytes per packet, this corresponds to roughly 1 Gbps of aggregate link speed, using the slide's simplified calculation.
- Modern networks may need to support aggregate link speeds as high as 40 Gbps.

**Main idea\:** Instead of examining one bit per step, examine several bits at once to reduce the number of memory accesses.

## 2\. What is a multibit trie?

A **multibit trie** is a trie in which multiple bits of the destination address are examined at each step.

The number of bits examined in one step is called the **stride**\.

If the stride is $k$, each node can have up to\:

$$
2^k \text{ children}
$$

For example\:

| Stride $k$ | Bits examined per step | Maximum children per node |
|--------|--------|--------|
| 1 | 0 or 1 | 2 |
| 2 | 2 bits at once | 4 |
| 3 | 3 bits at once | 8 |
| 4 | 4 bits at once | 16 |

For stride 3, the possible bit chunks are `000`, `001`, `010`, `011`, `100`, `101`, `110`, and `111`\.

The trade-off is important: **larger strides reduce trie depth but increase the number of child entries a node may need.**

## 3\. Prefix transformations

A multibit trie examines groups of bits, so prefix lengths may not align with the stride boundaries. The slides introduce two techniques for prefix transformation\:

1. Prefix expansion
2. Disjoint prefixes

The slides covered here explain prefix expansion in detail.

### Prefix expansion

A prefix is expanded when it is converted into several longer, more-specific prefixes that together cover the same range of addresses.

Consider the prefix\:

`0*`

The `*` means any sequence of remaining bits. The prefix covers all addresses whose first bit is `0`\.

It can be represented equivalently by two longer prefixes\:

- `00*`
- `01*`

Or, by four prefixes of length 3\:

- `000*`
- `001*`
- `010*`
- `011*`

Example: expanding `0*` to length 3

**`0*`**

Original prefix

`000*`

`001*`

`010*`

`011*`

All four expanded prefixes collectively cover exactly the addresses covered by `0*`\.

### Why must we expand prefixes?

Suppose a trie uses a stride of 3 bits at its first level. A prefix of length 1 does not identify a complete 3-bit chunk. Expansion makes it possible to represent that prefix at the appropriate level.

**Important\:** Prefix expansion must preserve longest-prefix-match behavior. If an expanded prefix overlaps with an existing, more-specific prefix, the more-specific prefix must retain precedence.

### Disjoint prefixes

The slides list disjoint prefixes as another prefix-transformation technique, but the material covered here does not give its detailed definition or procedure. We will not assume additional rules beyond the provided slides.

## 4\. Fixed-stride multibit tries

A **fixed-stride multibit trie** uses the same stride size for every node at a given level.

The example in your professor's slides uses\:

- Level 1: stride of 3 bits.
- Level 2: stride of 2 bits.

Therefore, the first level examines bits 1–3, and the second level examines bits 4–5.

Prefixes whose lengths do not match these boundaries must be transformed into suitable prefixes.

### Example from the slides: overlapping prefixes

Suppose prefix $P_1$ is expanded into four 3-bit prefixes\:

`000*`, `001*`, `010*`, `011*`

Now suppose an existing prefix $P_3$ is already equal to `001*`\.

Both represent the same address range, but the original $P_3$ is a more-specific prefix than $P_1$\. Therefore, according to longest prefix match, **$P_3$ must take precedence over the expanded entry for $P_1$**\.

This is an important exam concept: expansion must not change the forwarding decision produced by longest prefix match.

## 5\. Searching a fixed-stride multibit trie

The search algorithm works as follows\:

1. Divide the destination address into chunks corresponding to the stride at each level.
2. Use each chunk to select the next child node.
3. Whenever a node contains valid forwarding information, remember it as the best matching prefix so far.
4. Continue until there are no more branches to follow or the address bits are exhausted.
5. Return the last remembered best match.

### Worked example: searching `100111`

For illustration, assume a fixed stride of 3 bits at each level.

Split the address\:

$$
100111 \rightarrow 100 \mid 111
$$

The search process is\:

| Step | Bit chunk | Action |
|--------|--------|--------|
| 1 | `100` | Follow the root's `100` child |
| 2 | `111` | Follow the `111` child of the selected node |
| 3 | — | Keep the most-specific valid prefix encountered |
| 4 | — | Return the best match when traversal ends |

The exact next hop depends on the forwarding entries stored in the trie. The text extracted from the slide does not expose the complete diagram's entries, so a specific next hop cannot be determined from the address alone.

### Search complexity

If $W$ is the address width and $k$ is a fixed stride, the approximate number of levels traversed is\:

$$
\boxed{O(W/k)}
$$

For example, with a 32-bit IPv4 address and stride 4\:

$$
32/4=8 \text{ levels}
$$

This is a reduction from the worst-case 32 levels of a binary trie, assuming each level requires one memory access.

## 6\. Inserting prefixes

The slides demonstrate insertion of `1100*` into a trie whose strides are 3 bits at level 1 and 2 bits at level 2.

### Step-by-step insertion of `1100*`

**Step 1: Divide into chunks.**

The first three bits form `110`, leaving `0*`\.

$$
1100^* \rightarrow 110 \mid 0^*
$$

**Step 2: Find the first-level node.**

Follow the root's `110` entry. The slide identifies the resulting node as $P_4$\.

**Step 3: Expand the incomplete second-level chunk.**

The second-level stride is 2, but the remaining prefix has only one fixed bit. Expand `0*` into\:

- `00*`
- `01*`

**Step 4: Create the necessary children.**

If $P_4$ has no children, create the four entries required by the 2-bit stride: `00`, `01`, `10`, and `11`\.

**Step 5: Store forwarding information.**

Augment entries `00` and `01` with the forwarding information for `1100*`\. The other two entries do not receive this prefix's information.

The same general principle applies to insertion of other prefixes: locate the correct path, expand incomplete chunks where necessary, and update the relevant entries while preserving longest-prefix-match semantics.

## 7\. Search, update, and space complexity

Let\:

- $W$ \= address width.
- $k$ \= stride size.
- $N$ \= number of prefixes.

| Operation | Complexity from the slides |
|--------|--------|
| Search | $O(W/k)$ |
| Insertion | $O(W/k + 2^k)$ |
| Deletion | $O(W/k + 2^k)$ |
| Space | $O(2^k NW/k)$ |

The update complexity includes the search and the cost of modifying entries. The slides describe the maximum number of entries modified as $2^{k-1}$, then give the overall update bound as $O(W/k + 2^k)$\.

The space bound illustrates why choosing a very large stride is expensive: each node may require an array of $2^k$ entries, so memory consumption grows exponentially with the stride.

## 8\. Implementing a multibit trie

According to slide 29, each trie node is implemented using an array and pointers linking it to subtries.

For a node at a level with stride $k$\:

- The array has $2^k$ entries.
- Each entry has a `nhop` field that stores next-hop information.
- Each entry has a `ptr` field that stores a pointer to a subtrie, if one exists.

Nodes at different levels may have different array sizes because their strides may differ.

## 9\. Choosing the stride size

This is the central design trade-off in multibit tries.

| Larger strides | Smaller strides |
|--------|--------|
| Fewer trie levels | More trie levels |
| Fewer memory accesses | More memory accesses |
| Faster potential lookup | Slower potential lookup |
| Larger arrays and more memory | Smaller arrays and less memory |

The goal is to choose strides that provide the desired lookup speed without consuming excessive storage. The slides state that the designer chooses the number of levels based on the desired lookup speed, with an objective of at most three levels for a given prefix set while minimizing storage.

---

## 10\. Exam-oriented questions

Try these after writing the notes.

1. Why are multibit tries needed when binary tries already support longest prefix match?
2. Define stride. How many children can a node have when the stride is 4?
3. Explain prefix expansion with the prefix `0*`\.
4. What is a fixed-stride multibit trie?
5. Explain why prefix expansion must preserve longest-prefix-match behavior.
6. Describe the search algorithm for a multibit trie.
7. Search `100111` using a trie with a stride of 3 bits per level. Explain how the chunks are used.
8. Explain the insertion of `1100*` in a trie with strides 3 and 2.
9. Derive the search and update complexity of a fixed-stride multibit trie.
10. Explain the trade-off between lookup speed and memory consumption when selecting strides.
11. Describe the fields used in the array-based implementation of a multibit trie.
12. Compare binary tries and fixed-stride multibit tries.

### Quick revision

- **Binary trie\:** examines one bit per level.
- **Multibit trie\:** examines multiple bits per level.
- **Stride\:** number of bits examined at each step.
- **Prefix expansion\:** replaces a prefix with longer prefixes covering the same address range.
- **Fixed stride\:** all nodes at the same level use the same stride size.
- **Search\:** approximately $O(W/k)$ for fixed stride $k$\.
- **Trade-off\:** fewer levels and potentially faster search, but greater memory consumption.

The next section of your PDF is **Variable-stride Multibit Tries**, followed by **Level-Compressed (LC) Tries**, which combine path compression with level compression.

## Response:
10/25/2026, 5:29:38 PM · gpt-6

> **Reviewed file content lines 275–323**
>
> 
>
> Worked for 10s

# Topic 5: Variable-Stride Multibit Tries and Level-Compressed (LC) Tries

Advanced Computer Networks — IP Address Lookup &#124; Slides 30–36

This section builds on the multibit tries we just studied. The main goal is to **reduce lookup time without using excessive memory**\.

There are two important ideas\:

- **Variable-stride multibit tries\:** allow different stride sizes at the same level.
- **Level-compressed (LC) tries\:** combine path compression and level compression to reduce both trie height and redundant storage.

## 1\. Variable-stride multibit tries

In a fixed-stride trie, every node at the same level uses the same stride size. In a variable-stride trie, this restriction is removed.

**Definition\:** A variable-stride multibit trie is a multibit trie in which nodes at the same level can have different stride sizes.

For example, one node might inspect 2 bits while another node at the same level inspects 3 bits.

Conceptual comparison

Fixed stride

Root: 3 bits

All nodes: 2 bits

Variable stride

Root: 3 bits

2 bits

3 bits

Illustrative structures, not reproductions of the professor's slide diagrams.

The advantage of variable strides is flexibility: stride sizes can be chosen to suit the prefix distribution. The provided slide defines this structure but does not give a detailed algorithm for choosing the stride at each node.

## 2\. Compressing multibit tries

Prefix expansion reduces the number of levels in a trie, but it can create many entries with the same best-matching prefix. This increases memory consumption.

The slides identify two goals of compression\:

1. Remove redundant information to save memory.
2. Reduce trie height to make searching faster.

The slides also note that a compressed structure may fit entirely into the processor's L1 cache, potentially reducing memory-access time.

This motivates **Level-Compressed (LC) Tries**\.

## 3\. Level-Compressed (LC) tries

An LC trie combines two techniques\:

- **Path compression\:** compress sparse regions of a trie.
- **Level compression\:** use the multibit-trie idea of prefix expansion to reduce the number of levels.

How LC tries combine both techniques

**Original trie**

May contain long sparse paths and many levels

Step 1: Path compression

Skip unnecessary intermediate nodes

**Path-compressed trie**

Sparse paths are shortened

Step 2: Level compression

Compress densely populated subtries

**LC trie**

Designed to reduce both storage and lookup depth

### Why not use only one technique?

| Technique | Main benefit | Limitation |
|--------|--------|--------|
| Path compression | Reduces nodes along sparse paths | Does not necessarily reduce height in dense regions |
| Level compression | Reduces the number of levels | Prefix expansion can increase storage |
| LC trie | Combines both techniques | Requires managing compressed paths and expanded entries |

The central idea is to use path compression where the trie is sparse and level compression where it is dense.

## 4\. Step 1 — Path compression in an LC trie

When intermediate nodes are skipped, the lookup algorithm must know how many bits to skip.

The slides introduce a **skip value**, denoted by $s$\.

The skip value indicates how many bits must be skipped along a path.

For example, if a path contains intermediate nodes that have been removed, the compressed node stores the necessary skip information so that the search can move to the appropriate position in the destination address.

**Exam point\:** Path compression reduces the number of nodes traversed, but the algorithm must preserve enough information to interpret the skipped bits correctly.

## 5\. Step 2 — Level compression in an LC trie

After path compression, the next step is to compress densely populated regions.

According to slide 35\:

- Consider a node $n$ that is the root of a complete subtrie of maximum depth $d$\.
- Replace that complete subtrie with a corresponding one-level multibit subtrie.

The slide describes $d$ as the branching factor. Since the detailed diagram is not available in the extracted text, retain the slide's wording for your notes and follow the instructor's diagram for the exact construction.

The general objective is to represent a deeper region in fewer levels, reducing lookup depth.

## 6\. Comparison of all trie structures

| Feature | Binary trie | Fixed-stride multibit trie | Variable-stride multibit trie | LC trie |
|--------|--------|--------|--------|--------|
| Bits examined per step | One | Multiple | Multiple | Multiple, with compression |
| Stride selection | 1 bit | Same stride at a given level | May differ among nodes at the same level | Uses path and level compression |
| Main advantage | Simple structure | Fewer levels | Flexible stride selection | Balances reduced height and storage |
| Main concern | Many memory accesses | Larger arrays and storage | Stride-selection complexity | Managing compressed paths and levels |

## 7\. Exam questions with short answers

**Q1. Define a variable-stride multibit trie.**

A multibit trie in which nodes at the same level can have different stride sizes.

**Q2. What is the difference between path compression and level compression?**

Path compression removes unnecessary intermediate nodes in sparse regions. Level compression reduces the number of levels by using multibit-trie concepts in densely populated regions.

**Q3. What is the purpose of the skip value $s$?**

It indicates how many bits need to be skipped along a compressed path.

**Q4. Why can prefix expansion increase storage requirements?**

One prefix may be represented by multiple longer prefixes, creating more entries in the trie.

**Q5. What is the main idea of an LC trie?**

It combines path compression and level compression to reduce storage requirements while keeping the number of lookup levels small.

**Q6. Why is fitting a trie into L1 cache beneficial?**

L1 cache access is much faster than accessing main memory or other slower memory levels, so fitting the structure into cache can improve lookup performance.

**Q7. Compare the objectives of path compression and level compression.**

Path compression targets sparse portions of the trie; level compression targets dense portions.

---

## Chapter revision checklist

IP Address Lookup — final revision  of

That completes the concepts in your uploaded **IP Address Lookup** PDF, from forwarding functions and longest prefix match through binary tries, multibit tries, and LC tries. The last slide contains no additional substantive topic in the extracted text.



---
Powered by [ChatGPT Exporter](https://www.chatgptexporter.com)