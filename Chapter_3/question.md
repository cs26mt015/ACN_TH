Absolutely. For **Queuing Theory**, especially your professor's PPT, I would prepare questions in **five categories**: theoretical, conceptual/tricky, numerical, scenario-based, and derivation/formula-based.

Below is a comprehensive exam-oriented question bank. I’m **not giving answers yet**, so you can attempt them first.

# CS 606T — Queuing Theory Question Bank

## A. Random Variables — Basic & Conceptual

### Q1.

Define a random variable. Explain why a random variable is actually a deterministic function.

### Q2.

Differentiate between:

* Sample space
* Outcome
* Random variable
* Realization of a random variable

### Q3.

A coin is tossed three times. Define a random variable \(X\) representing the number of heads. List the possible values of \(X\).

### Q4.

Classify the following as discrete or continuous:

a. Number of packets arriving at a router in one second
b. Time required to transmit a packet
c. Number of customers in a queue
d. Packet size
e. Waiting time of a packet

### Q5.

Can a continuous random variable have a probability \(P(X=x)>0\) for some particular value \(x\)? Explain.

### Q6.

Explain the difference between PMF and PDF.

### Q7.

Why is the probability at a single point generally zero for a continuous random variable?

### Q8.

A random variable \(X\) has:

$$
P(X=0)=0.2,\quad P(X=1)=0.5,\quad P(X=2)=0.3
$$

Is this a valid PMF? Explain.

### Q9.

If a PDF is given by \(f_X(x)\), how do you calculate:

$$
P(a<X<b)?
$$

### Q10.

Explain why the **area under a PDF**, rather than the height of the PDF, represents probability.

---

# B. Bernoulli, Binomial & Poisson

### Q11.

What is a Bernoulli random variable? State its PMF.

### Q12.

A packet transmission succeeds with probability 0.95. Define a Bernoulli random variable for the transmission.

### Q13.

What is the difference between a Bernoulli and Binomial random variable?

### Q14.

A router sends 10 packets. Each packet independently has probability 0.9 of successful delivery.

Find the probability that exactly 8 packets are successfully delivered.

### Q15.

For the same experiment, calculate the probability that at least 8 packets are successfully delivered.

### Q16.

A server receives a request successfully with probability 0.8. Five independent requests are made.

Find the probability that none of them succeeds.

### Q17.

State the assumptions required for the Binomial distribution.

### Q18.

Explain why the Poisson distribution is useful for modeling packet arrivals.

### Q19.

A router receives packets at an average rate of 5 packets/ms. Find the probability that exactly 3 packets arrive during the next millisecond.

### Q20.

Packets arrive at a router according to a Poisson process with rate 100 packets/sec.

Find the probability that no packet arrives during 10 ms.

### Q21.

For a Poisson random variable with parameter \(\lambda\), derive:

$$
P(X=0)
$$

### Q22.

A network link receives 600 packets per minute according to a Poisson process.

Find the probability that exactly 5 packets arrive during the next second.

### Q23.

What is the relationship between the Poisson distribution and the exponential distribution?

---

# C. Exponential Distribution

### Q24.

State the PDF and CDF of an exponential random variable with parameter \(\mu\).

### Q25.

What is the mean of an exponential random variable?

### Q26.

A packet arrival rate is 20 packets/sec. What is the expected inter-arrival time?

### Q27.

Packets arrive according to an exponential inter-arrival distribution with rate 10 packets/sec.

Find:

$$
P(T>0.2)
$$

### Q28.

A customer has already waited 15 minutes for a taxi. The average waiting time is 10 minutes.

Does the probability of waiting another 5 minutes depend on the fact that the customer has already waited 15 minutes?

Explain mathematically.

### Q29.

Prove the memoryless property of the exponential distribution:

$$
P(X>s+t\mid X>s)=P(X>t)
$$

### Q30.

Explain the difference between:

$$
P(X<t)
$$

and

$$
P(X>t)
$$

for an exponential random variable.

### Q31.

A network packet's inter-arrival time is exponentially distributed with rate \(50\) packets/sec.

Calculate the probability that the next packet arrives within 20 ms.

### Q32.

A server's service time is exponentially distributed with mean 4 ms.

Calculate:

a. Service rate
b. Probability service takes more than 10 ms
c. Probability service finishes within 5 ms

---

# D. Stochastic Processes

### Q33.

Define a stochastic process.

### Q34.

Explain the difference between a random variable and a stochastic process.

### Q35.

Why can a stochastic process be described as a "movie" whereas a random variable is a "snapshot"?

### Q36.

Consider:

$$
X(t)=\text{number of packets in a router at time }t
$$

Explain:

a. What is the random variable?
b. What is the parameter space?
c. What is the state space?

### Q37.

Give an example of:

a. Discrete-time stochastic process
b. Continuous-time stochastic process

### Q38.

What does it mean for a stochastic process to be stationary?

### Q39.

Explain the following equation:

$$
F(x_1,\ldots,x_n;t_1+h,\ldots,t_n+h)
=
F(x_1,\ldots,x_n;t_1,\ldots,t_n)
$$

### Q40.

Why is stationarity useful when analyzing network traffic?

---

# E. Markov Processes & Markov Chains

### Q41.

Define the Markov property.

### Q42.

Why is the Markov property called the **memoryless property**?

### Q43.

Explain the difference between a Markov process and a Markov chain.

### Q44.

A queue currently contains 5 packets. Explain what the Markov property means in this situation.

### Q45.

Suppose:

$$
P(X_{n+1}=j\mid X_n=i,X_{n-1}=k)
$$

Explain how this expression simplifies under the Markov property.

### Q46.

What is a transition probability matrix?

### Q47.

What properties must every transition probability matrix satisfy?

### Q48.

Given:

$$
P=
\begin{bmatrix}
0.7&0.3\\
0.2&0.8
\end{bmatrix}
$$

Is this a valid transition matrix? Explain.

### Q49.

For the above matrix, find:

$$
P(X_1=1\mid X_0=0)
$$

### Q50.

Calculate \(P^2\) for:

$$
P=
\begin{bmatrix}
0.7&0.3\\
0.2&0.8
\end{bmatrix}
$$

and explain what its entries represent.

### Q51.

Explain the meaning of the \(n\)-step transition probability matrix.

### Q52.

Prove:

$$
P^{(n)}=P^n
$$

for a time-homogeneous Markov chain.

---

# F. Poisson Process

### Q53.

Define a Poisson process.

### Q54.

State all the important assumptions/properties of a Poisson process.

### Q55.

If customers arrive at a rate of 120 customers/hour, find \(\lambda\) in customers/minute.

### Q56.

Packets arrive at a router according to a Poisson process with rate 100 packets/sec.

Find the probability of exactly 3 arrivals in 50 ms.

### Q57.

A server receives 10 requests per second.

Find the probability that no request arrives during 100 ms.

### Q58.

For a Poisson process with rate \(\lambda\), derive:

$$
P(N(t)=k)
$$

### Q59.

Explain the relationship:

$$
\text{Poisson arrivals}
\Longleftrightarrow
\text{Exponential inter-arrival times}
$$

### Q60.

Explain the **merging property** of Poisson processes.

### Q61.

Two independent packet streams have rates:

$$
\lambda_1=100
$$

and

$$
\lambda_2=200
$$

packets/sec.

Find the rate of the combined stream.

### Q62.

Three independent Poisson processes have rates 10, 20 and 30 requests/sec.

Find the rate after merging them.

### Q63.

Explain the splitting property of a Poisson process.

### Q64.

A packet stream arrives at rate 1000 packets/sec. Each packet is independently sent to Queue A with probability 0.3 and Queue B with probability 0.7.

Find the arrival rates of the two queues.

### Q65.

Explain why the two streams generated by splitting a Poisson process are themselves Poisson processes.

---

# G. Little's Law

This section is **very important for numericals**.

### Q66.

State Little's Law.

### Q67.

Explain each parameter in:

$$
N=\lambda T
$$

### Q68.

A router receives 100 packets/sec and the average packet spends 20 ms in the system.

Find the average number of packets in the system.

### Q69.

A network contains an average of 50 packets and the arrival rate is 100 packets/sec.

Find the average time spent by a packet.

### Q70.

A queue contains an average of 10 customers. Customers arrive at 2 customers/minute.

Find the average time spent in the system.

### Q71.

Explain why Little's Law does **not require Poisson arrivals**.

### Q72.

A network has three independent traffic streams with:

$$
\lambda_1=10,\quad
\lambda_2=20,\quad
\lambda_3=30
$$

packets/sec.

Their average delays are 1 sec, 2 sec and 3 sec respectively.

Find the total average number of packets in the system.

### Q73.

A router has an average of 100 packets in the system and an average delay of 50 ms.

Find the throughput.

### Q74.

Differentiate between:

$$
N
$$

and

$$
N_q
$$

in queueing systems.

### Q75.

Derive the relationship:

$$
N=N_q+N_s
$$

---

# H. M/M/1 Queue — Extremely Important

### Q76.

What does each `M` and `1` represent in an M/M/1 queue?

### Q77.

State the assumptions of an M/M/1 queue.

### Q78.

Define traffic intensity/utilization:

$$
\rho=\frac{\lambda}{\mu}
$$

Explain its significance.

### Q79.

Why must:

$$
\rho<1
$$

for a stable M/M/1 queue?

### Q80.

Derive:

$$
P(n)=(1-\rho)\rho^n
$$

### Q81.

Derive the probability that the system is empty:

$$
P(0)=1-\rho
$$

### Q82.

For an M/M/1 queue with:

$$
\lambda=5
$$

customers/sec and

$$
\mu=10
$$

customers/sec, calculate:

a. \(\rho\)
b. \(P(0)\)
c. \(P(3)\)

### Q83.

For the same queue, calculate the probability that at least 5 customers are in the system.

### Q84.

Derive the average number of customers in the system:

$$
\boxed{N=\frac{\rho}{1-\rho}}
$$

### Q85.

Using Little's Law, derive:

$$
\boxed{T=\frac{1}{\mu-\lambda}}
$$

### Q86.

Derive the average waiting time in the queue:

$$
\boxed{W=\frac{\lambda}{\mu(\mu-\lambda)}}
$$

### Q87.

Explain the difference between:

* \(T\): time in system
* \(W\): waiting time in queue
* Service time

### Q88.

Show that:

$$
T=W+\frac1\mu
$$

### Q89.

For an M/M/1 queue:

$$
\lambda=100\text{ packets/sec}
$$

and

$$
\mu=125\text{ packets/sec}
$$

calculate:

a. Utilization
b. Average number in system
c. Average delay
d. Average queueing delay
e. Average number in queue

### Q90.

A router receives 900 packets/sec and serves 1000 packets/sec.

Calculate:

a. \(\rho\)
b. \(N\)
c. \(T\)
d. \(W\)
e. \(N_q\)

---

# I. M/M/1 — Challenging Numericals

### Q91.

An M/M/1 router operates at 80% utilization. Its service rate is 500 packets/sec.

Find:

a. Arrival rate
b. Average number of packets
c. Average delay
d. Average queueing delay.

### Q92.

An M/M/1 system has average delay of 10 ms and service rate of 200 packets/sec.

Find the arrival rate.

### Q93.

An M/M/1 queue has:

$$
\lambda=90
$$

packets/sec.

The required average delay must be less than 20 ms.

Find the minimum service rate required.

### Q94.

A router has service rate 1000 packets/sec. What arrival rate will produce utilization of 95%?

What happens to the average delay as utilization approaches 1?

### Q95.

Two routers have identical service rates.

Router A operates at:

$$
\rho=0.5
$$

Router B operates at:

$$
\rho=0.9
$$

Compare their average queue lengths mathematically.

---

# J. M/M/1 Buffer Overflow

### Q96.

An M/M/1 gateway has:

$$
\lambda=125
$$

pps and service rate:

$$
\mu=500
$$

pps.

Calculate:

a. Utilization
b. \(P(n)\)
c. Probability of having more than 13 packets.

### Q97.

For an M/M/1 queue with \(\rho=0.25\), calculate:

$$
P(N>13)
$$

### Q98.

A router needs packet loss probability below:

$$
10^{-6}
$$

Explain how you would determine the required buffer size.

### Q99.

Why does buffer overflow probability decrease exponentially with buffer size in an M/M/1 model?

---

# K. M/M/m Queues

### Q100.

What does M/M/m represent?

### Q101.

What is the difference between M/M/1 and M/M/m?

### Q102.

In an M/M/m system, why does the departure rate depend on the number of busy servers?

### Q103.

A system has 4 servers, each with service rate 10 customers/hour.

What is the maximum aggregate service capacity?

### Q104.

Customers arrive at 10/hour and there are 5 servers, each serving at 3/hour.

Calculate the traffic intensity.

### Q105.

Explain the meaning of:

$$
\rho=\frac{\lambda}{m\mu}
$$

### Q106.

Why must:

$$
\rho<1
$$

for an M/M/m queue?

### Q107.

Derive the probability of zero customers:

$$
P_0=
\left[
\sum_{n=0}^{m-1}\frac{(m\rho)^n}{n!}
+
\frac{(m\rho)^m}{m!(1-\rho)}
\right]^{-1}
$$

### Q108.

For an M/M/2 queue:

$$
\lambda=10,\quad\mu=8
$$

per server.

Determine whether the system is stable.

### Q109.

Explain the meaning of the Erlang-C probability:

> Probability that an arriving customer has to wait.

### Q110.

Why can M/M/m reduce waiting time compared with M/M/1?

---

# L. M/M/m Numerical — Exam Level

### Q111.

Students arrive at a computer center at 10 students/hour. There are 5 terminals. Each terminal serves a student at an average rate of 3 students/hour.

Calculate:

a. Traffic intensity
b. Probability that all terminals are idle
c. Probability that all terminals are busy
d. Average number waiting
e. Average number in system.

### Q112.

A network router has 4 parallel processors. Packets arrive at 100 packets/sec, and each processor can process 30 packets/sec.

Calculate the traffic intensity and determine whether the system is stable.

### Q113.

A call center receives 20 calls/minute. It has 4 operators, each capable of handling 6 calls/minute.

Calculate the utilization.

### Q114.

Compare an M/M/1 system with service rate \(2\mu\) against an M/M/2 system where each server has rate \(\mu\).

What factors should be considered?

---

# M. Scenario-Based Questions

### Q115.

A cloud server experiences highly unpredictable request arrivals. Requests arrive independently and service times vary randomly.

Which queueing model would be a reasonable first approximation and why?

### Q116.

A router receives packets at a rate almost equal to its service rate.

Explain what happens to:

* Queue length
* Delay
* Utilization
* Packet loss

as \(\lambda\rightarrow\mu\).

### Q117.

A network administrator wants to reduce packet delay. They can either:

1. Increase the processing speed of one server, or
2. Add another server.

Explain how M/M/1 and M/M/m models can help make this decision.

### Q118.

A supermarket has a single queue and five checkout counters.

What queueing model is conceptually closest to this system?

### Q119.

A supermarket instead has five independent queues, one per checkout counter.

How does this differ from a single-queue multi-server system?

### Q120.

Explain why a single shared queue can sometimes provide better load balancing than multiple independent queues.

---

# N. Switching Fabric + Queuing Theory

### Q121.

Explain why queues occur inside a switching fabric.

### Q122.

What is the relationship between:

**Input Queuing → HOL Blocking → Reduced Throughput?**

### Q123.

An \(N\times N\) crossbar has input queues. Two inputs request the same output in a time slot.

What happens to:

a. The winning packet
b. The losing packet
c. Packets behind the losing packet?

### Q124.

Why does Virtual Output Queuing reduce HOL blocking?

### Q125.

How many virtual output queues are required for an \(N\times N\) switch?

### Q126.

An \(8\times8\) crossbar uses VOQ.

How many queues are required?

### Q127.

A \(32\times32\) crossbar uses input FIFO queues.

Explain why its theoretical capacity may not be fully utilized.

### Q128.

Explain the relationship between queueing theory and switch-fabric scheduling.

---

# O. Input-Queued Switch Throughput

### Q129.

What assumptions are made in the classical throughput analysis of an input-queued switch?

### Q130.

Define:

$$
F_m
$$

and

$$
B_{i,m}
$$

### Q131.

Explain:

$$
F_m=N-\sum_{i=1}^{N}B_{i,m}
$$

### Q132.

Taking expectation, derive:

$$
E[F]=N(1-E[B_i])
$$

### Q133.

Given:

$$
E[F]=N\rho
$$

derive the relationship between \(\rho\) and \(E[B_i]\).

### Q134.

Explain why the classical HOL-blocking input-queued switch achieves approximately 58.6% throughput.

### Q135.

An \(N\times N\) input-queued switch has \(\rho=0.586\).

Derive its expected number of transmitted packets per slot.

### Q136.

An \(8\times8\) input-queued switch operates at the theoretical HOL limit.

Calculate its average throughput in packets/slot.

### Q137.

A \(16\times16\) input-queued switch operates at 58.6% throughput.

Calculate the average number of packets transmitted per slot.

### Q138.

Why does increasing \(N\) not eliminate HOL blocking?

---

# P. Very Tricky Conceptual Questions

### Q139.

True or False:

> If a queue has enough physical buffer space, packets can never be lost.

Justify.

### Q140.

True or False:

> In an M/M/1 queue, increasing utilization from 50% to 90% increases delay by only 40%.

Explain mathematically.

### Q141.

True or False:

> A Poisson process has exponentially distributed inter-arrival times.

### Q142.

True or False:

> The exponential distribution is memoryless, but the Poisson distribution itself is not a waiting-time distribution.

Explain.

### Q143.

True or False:

> Little's Law requires Poisson arrivals.

### Q144.

True or False:

> In an M/M/m system, increasing the number of servers always decreases utilization.

Explain carefully.

### Q145.

True or False:

> In an input-queued switch, a packet that is blocked at the HOL can prevent packets behind it from being transmitted even when their desired outputs are free.

### Q146.

True or False:

> Output queuing suffers from HOL blocking in the same way as input FIFO queuing.

---

# Q147 — Integrated Numerical

A router receives packets according to a Poisson process at:

$$
\lambda=800\text{ packets/sec}
$$

The router has an exponentially distributed service time with:

$$
\mu=1000\text{ packets/sec}
$$

Answer:

1. What queueing model applies?
2. Calculate utilization.
3. Calculate probability that the system is empty.
4. Calculate probability of exactly 3 packets.
5. Calculate average number of packets.
6. Calculate average delay.
7. Calculate average queueing delay.
8. Calculate average number waiting in queue.

---

# Q148 — Integrated Network Scenario

A network gateway receives:

$$
500\text{ packets/sec}
$$

and takes an average of:

$$
1.5\text{ ms}
$$

to process each packet.

Assume an M/M/1 model.

Find:

1. Service rate
2. Utilization
3. Probability gateway is empty
4. Average number of packets
5. Average delay
6. Average queueing delay
7. Average number waiting
8. Probability of more than 10 packets in the system.

---

# Q149 — Design Question

A router currently has one processor with:

$$
\mu=1000\text{ packets/sec}
$$

and receives:

$$
\lambda=900\text{ packets/sec}.
$$

The engineer has two options:

**Option A:** Replace it with one processor having service rate \(2000\) packets/sec.

**Option B:** Use two processors, each with service rate \(1000\) packets/sec, sharing a single queue.

Using queueing theory, calculate the relevant performance measures for both designs and explain the differences.

---

# Q150 — Full Switching-Fabric Question

An \(N\times N\) crossbar uses input FIFO queues and fixed-length packets. Destinations are uniformly distributed.

Answer:

1. Explain HOL blocking.
2. Explain why some output ports may remain idle.
3. Define \(F_m\).
4. Define \(B_{i,m}\).
5. Derive:

$$
E[F]=N(1-E[B_i])
$$

6. Explain the significance of:

$$
E[F]=N\rho
$$

7. State the classical throughput result.

---

## ⭐ Highest-Priority Questions for Your Exam

If your professor wants **conceptual + numerical + derivation questions**, I would especially practice:

**Q6, Q10, Q17, Q23, Q29, Q33, Q38, Q41, Q46, Q50, Q52, Q54, Q60, Q63, Q67, Q71, Q78, Q79, Q80, Q84, Q85, Q86, Q89, Q91, Q93, Q96, Q98, Q100, Q107, Q111, Q117, Q120, Q122, Q124, Q125, Q129, Q132, Q134, Q136, Q140, Q142, Q143, Q145, Q147, Q148, Q149, Q150.**

These cover the **definitions, derivations, calculations, tricky concepts, network scenarios, and switching-fabric application** portions of the PPT.
