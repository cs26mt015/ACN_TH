# CS 606T — Chapter 3: Queuing Theory Exam Answers

This answer sheet follows `question.md` (Q1–Q150). For a 5–10 mark answer, write the definition, formula, assumptions, and 3–4 explanatory points. Rates and times must use matching units.

## Formula sheet to write before numericals

| Topic | Formula |
|---|---|
| Binomial | `P(X=k) = C(n,k)p^k(1-p)^(n-k)` |
| Poisson count in time `t` | `P(N(t)=k)=e^(-λt)(λt)^k/k!` |
| Exponential | `F(t)=1-e^(-μt)`, `P(T>t)=e^(-μt)`, `E[T]=1/μ` |
| Little's Law | `N=λT`, `Nq=λW`, `N= Nq+Ns` |
| M/M/1 utilization | `ρ=λ/μ`, with `ρ<1` |
| M/M/1 state probability | `P(n)=(1-ρ)ρ^n`; `P(N>k)=ρ^(k+1)` |
| M/M/1 averages | `N=ρ/(1-ρ)`, `T=1/(μ-λ)`, `Nq=ρ²/(1-ρ)`, `W=λ/[μ(μ-λ)]` |
| M/M/m utilization | `ρ=λ/(mμ)`, with `ρ<1` |
| Classical input-FIFO crossbar | `E[F]=Nρ`, and maximum `ρ≈0.586` |

## Detailed theory for long answers

### Poisson process, exponential time, and queues

A **Poisson process** models random independent arrivals at constant average rate `λ`. It has independent and stationary increments, `N(t)~Poisson(λt)`, and the probability of two or more arrivals in a very small interval is negligible. Its inter-arrival times are independent exponential random variables.

The **exponential distribution** is memoryless:

`P(T>s+t | T>s)=P(T>t)`.

Thus, the future waiting time does not depend on how long has already elapsed. Poisson arrivals plus exponential service time and one server give the **M/M/1** queue.

### M/M/1 answer structure

`M/M/1` means Markovian/Poisson arrivals, Markovian/exponential service times, and one server. It assumes infinite calling population and buffer, FCFS service (normally), independent arrivals and service, and stability `λ<μ`.

At equilibrium:

1. `ρ=λ/μ` is the fraction of time the server is busy.
2. `P(n)=(1-ρ)ρ^n`; probability system is empty is `P0=1-ρ`.
3. Increasing `ρ` near 1 makes queue length and delay grow nonlinearly and ultimately without bound.
4. `T` includes both waiting and service; `W` is waiting only; `T=W+1/μ`.

### Input-queued switch throughput

In an `N×N` crossbar with one FIFO at every input, only the head-of-line (HOL) packet can request a transfer. If that packet wants a busy output, packets behind it cannot move even if their outputs are idle. This is HOL blocking.

With fixed-size packets, slotted time, uniformly random destinations, and one FIFO at each input, classical analysis gives maximum per-port throughput about **0.586**. Therefore average transfers per slot are `E[F]=0.586N`, not `N`. VOQ removes this FIFO coupling by maintaining one input queue per output, but requires `N²` queues and a fast matching scheduler.

## A. Random variables (Q1–Q10)

**Q1.** A random variable is a function that assigns a real number to every outcome of a random experiment. The experiment outcome is random, but once an outcome is given, the function's output is fixed; therefore the mapping itself is deterministic.

**Q2.** The **sample space** is the set of all possible outcomes. An **outcome** is one element of that set. A **random variable** maps outcomes to numbers. A **realization** is the actual numerical value obtained when the experiment occurs.

**Q3.** Let `X = number of heads in three tosses`. Then `X∈{0,1,2,3}`.

**Q4.** (a) discrete, (b) continuous, (c) discrete, (d) generally discrete when measured in bytes (continuous if idealized as a real measurement), (e) continuous.

**Q5.** No. For a continuous RV, `P(X=x)=0` for every individual point; probabilities are assigned to intervals through area under the PDF.

**Q6.** A PMF gives `P(X=x)` for a discrete RV and its values sum to 1. A PDF is a nonnegative density for a continuous RV; probabilities are integrals of it, and the total area is 1.

**Q7.** A single point has zero width, so its integral/area under a finite PDF is zero. Only an interval with nonzero width has nonzero probability.

**Q8.** Yes. All probabilities are nonnegative and `0.2+0.5+0.3=1`, so it is a valid PMF.

**Q9.** `P(a<X<b)=∫_a^b f_X(x) dx`.

**Q10.** PDF height is density, not probability; it may even exceed 1. Probability must be dimensionless and is obtained by accumulating density over an interval—i.e., the area.

## B. Bernoulli, Binomial, Poisson (Q11–Q23)

**Q11.** A Bernoulli RV has one trial and two outcomes. If success probability is `p`, `P(X=1)=p`, `P(X=0)=1-p`; its PMF is `p^x(1-p)^(1-x)`, `x=0,1`.

**Q12.** Define `X=1` if transmission succeeds and `X=0` if it fails. Then `X~Bernoulli(0.95)`, with `P(X=1)=0.95`, `P(X=0)=0.05`.

**Q13.** Bernoulli describes one success/failure trial. Binomial counts successes in `n` independent Bernoulli trials with the same success probability `p`.

**Q14.** `P(X=8)=C(10,8)(0.9)^8(0.1)^2 = 0.1937` approximately.

**Q15.** `P(X≥8)=P(8)+P(9)+P(10)=0.1937+0.3874+0.3487=0.9298` approximately.

**Q16.** None succeeds: `(1-0.8)^5=(0.2)^5=0.00032`.

**Q17.** Binomial assumptions: fixed `n` trials; two outcomes per trial; same success probability `p`; trials are independent; RV counts successes.

**Q18.** Packet arrivals are counts over time. When arrivals are independent, occur at approximately constant average rate, and simultaneous arrivals are negligible, Poisson gives a useful count model.

**Q19.** Here mean count in 1 ms is `λ=5`. `P(X=3)=e^-5·5^3/3!≈0.1404`.

**Q20.** In 10 ms, mean = `100×0.01=1`. `P(0)=e^-1≈0.3679`.

**Q21.** Put `k=0` in Poisson PMF: `P(X=0)=e^-λ λ^0/0!=e^-λ`.

**Q22.** Rate = `600/60=10` packets/s. `P(X=5)=e^-10·10^5/5!≈0.0378`.

**Q23.** In a Poisson process, the number of arrivals in an interval is Poisson; the time between successive arrivals is exponential. Poisson counts and exponential inter-arrival times describe the same process from two views.

## C. Exponential distribution (Q24–Q32)

**Q24.** For rate `μ>0`, PDF: `f(t)=μe^(-μt), t≥0`; CDF: `F(t)=P(T≤t)=1-e^(-μt)`.

**Q25.** Mean exponential time = `E[T]=1/μ`.

**Q26.** Expected inter-arrival time = `1/20 s = 0.05 s = 50 ms`.

**Q27.** `P(T>0.2)=e^(-10×0.2)=e^-2≈0.1353`.

**Q28.** No; exponential waiting is memoryless. If mean is 10 min, `μ=0.1/min`; `P(T>20 | T>15)=P(T>5)=e^-0.5≈0.6065`.

**Q29.** `P(X>s+t|X>s)=P(X>s+t)/P(X>s)=e^-μ(s+t)/e^-μs=e^-μt=P(X>t)`.

**Q30.** `P(X<t)=1-e^-μt` is probability of completion/arrival before time `t`; `P(X>t)=e^-μt` is the survival probability after time `t`. They are complements for continuous `X`.

**Q31.** `20 ms=0.02 s`. `P(T≤0.02)=1-e^(-50×0.02)=1-e^-1≈0.6321`.

**Q32.** Mean `4 ms=0.004 s`, so `μ=250/s`. (a) 250 services/s. (b) `P(T>0.01)=e^(-250×.01)=e^-2.5≈0.0821`. (c) `P(T≤.005)=1-e^-1.25≈0.7135`.

## D. Stochastic processes (Q33–Q40)

**Q33.** A stochastic process is a collection of random variables indexed by time or another parameter, e.g. `{X(t):t≥0}`.

**Q34.** A random variable gives one uncertain value; a stochastic process gives an evolving collection of uncertain values over time.

**Q35.** A snapshot records one time/value, like a random variable. A movie records changing values over time, like a stochastic process.

**Q36.** For `X(t)=number of packets at time t`: (a) for each fixed `t`, `X(t)` is the RV; (b) parameter space is time, typically `t≥0`; (c) state space is `{0,1,2,...}`.

**Q37.** Discrete-time: `X_n`, number of packets at the end of slot `n`. Continuous-time: `X(t)`, number in a router at any real time `t`.

**Q38.** A stationary process has statistical properties that do not change when the time origin is shifted. In strict stationarity, all joint distributions are invariant under a common time shift.

**Q39.** It states that the joint distribution of values at `t1,...,tn` is the same at `t1+h,...,tn+h`; only time differences matter, not absolute clock time.

**Q40.** Stationarity permits estimates measured in one interval to represent other intervals, simplifying traffic modeling, average calculations, and capacity planning.

## E. Markov processes and chains (Q41–Q52)

**Q41.** Markov property: given the current state, the future is independent of the past: `P(X_{n+1}=j|X_n=i,X_{n-1},...)=P(X_{n+1}=j|X_n=i)`.

**Q42.** It is called memoryless because the present state contains all information needed for the next-state distribution; prior history adds no further predictive information.

**Q43.** A Markov process may have continuous time and/or state; a Markov chain commonly means a process with discrete states and discrete transition steps (though continuous-time Markov chains also exist).

**Q44.** If the current queue length is 5, the probabilities of its next length depend only on the current 5 packets, not on how the queue reached length 5.

**Q45.** It becomes `P(X_{n+1}=j|X_n=i)`, usually written `p_ij`.

**Q46.** A transition matrix `P=[p_ij]` lists one-step probabilities of moving from state `i` to state `j`.

**Q47.** Every entry satisfies `0≤p_ij≤1`; every row sums to 1 (for the row-vector convention); each row represents all possible next states from one current state.

**Q48.** Yes. Entries are nonnegative and rows sum to `0.7+0.3=1` and `0.2+0.8=1`.

**Q49.** `P(X1=1|X0=0)=p_01=0.3`.

**Q50.** `P²=[[0.55,0.45],[0.30,0.70]]`. Each entry `(i,j)` is the probability of reaching state `j` after two transitions starting in state `i`.

**Q51.** The `n`-step matrix `P^(n)` has entries `p_ij^(n)=P(X_n=j|X_0=i)`.

**Q52.** Chapman–Kolmogorov gives `P^(n+m)=P^(n)P^(m)`. Since `P^(1)=P`, repeated multiplication yields `P^(n)=P^n` for a time-homogeneous chain.

## F. Poisson process (Q53–Q65)

**Q53.** A Poisson process `{N(t)}` counts arrivals over time at constant rate `λ`, with `N(0)=0`, independent stationary increments, and Poisson-distributed counts `N(t)~Poisson(λt)`.

**Q54.** Important properties: constant rate; independent increments; stationary increments; probability of one arrival in small `h` is `λh+o(h)`; probability of two or more is `o(h)`; inter-arrival times are IID exponential.

**Q55.** `120/hour = 120/60 = 2 customers/minute`.

**Q56.** Mean in 50 ms = `100×.05=5`. `P(N=.3?)` For exactly 3: `e^-5·5³/3!≈0.1404`.

**Q57.** Mean in 100 ms = `10×.1=1`; `P(0)=e^-1≈0.3679`.

**Q58.** By the definition of a Poisson process, arrivals in interval length `t` have Poisson mean `λt`, hence `P(N(t)=k)=e^(-λt)(λt)^k/k!`.

**Q59.** A Poisson arrival process has IID exponential inter-arrival times. Conversely, IID exponential inter-arrival times of rate `λ` generate a Poisson counting process of rate `λ`.

**Q60.** The superposition/merging of independent Poisson processes of rates `λ1,λ2,...` is Poisson with rate `Σλi`.

**Q61.** Combined rate = `100+200=300 packets/s`.

**Q62.** Combined rate = `10+20+30=60 requests/s`.

**Q63.** Independently send each arrival of a Poisson process of rate `λ` to stream A with probability `p` and B with `1-p`. Then A and B are independent Poisson processes of rates `pλ` and `(1-p)λ`.

**Q64.** Queue A rate = `0.3×1000=300/s`; Queue B rate = `0.7×1000=700/s`.

**Q65.** Independent thinning preserves independent increments. In an interval, each original Poisson count is independently classified, producing Poisson counts with scaled means; therefore each split stream is Poisson.

## G. Little's Law (Q66–Q75)

**Q66.** Little's Law: `N=λT`, where average number in a stable system equals average throughput times average time in system.

**Q67.** `N` = average number in system; `λ` = long-run arrival/throughput rate; `T` = average time a customer/packet spends in the system, including service.

**Q68.** `N=100×0.020=2 packets`.

**Q69.** `T=N/λ=50/100=0.5 s`.

**Q70.** `T=N/λ=10/2=5 minutes`.

**Q71.** Little's Law is a long-run conservation/average relationship. It requires a stable system and well-defined averages, not Poisson arrivals, exponential service, or a particular queue discipline.

**Q72.** Total `N=ΣλiTi=10(1)+20(2)+30(3)=140 packets`.

**Q73.** `λ=N/T=100/0.05=2000 packets/s`.

**Q74.** `N` is average number in the complete system (waiting + in service). `Nq` is average number waiting in queue only.

**Q75.** Every customer is either waiting or receiving service. Taking averages gives `N=Nq+Ns`, where `Ns` is average number in service.

## H–J. M/M/1 and buffer overflow (Q76–Q99)

**Q76.** First `M`: Markovian/Poisson arrivals; second `M`: Markovian/exponential service; `1`: one server.

**Q77.** Assumptions: Poisson arrivals rate `λ`; IID exponential service rate `μ`; one server; FCFS typically; infinite buffer/population; independence; and `λ<μ` for steady state.

**Q78.** `ρ=λ/μ` is server utilization or offered load. It is the fraction of time the single server is busy and determines delay/queue size.

**Q79.** If `ρ≥1`, arrivals occur at least as fast as service on average. The queue has no finite steady-state average and grows without bound.

**Q80.** In equilibrium, birth-death balance gives `λP_n=μP_(n+1)`, so `P_(n+1)=ρP_n` and `P_n=ρ^nP_0`. Normalization `ΣP_n=1` gives `P0=1-ρ`, hence `P(n)=(1-ρ)ρ^n`.

**Q81.** From `Σ_n=0^∞ P0ρ^n=1`, `P0(1/(1-ρ))=1`; thus `P0=1-ρ`.

**Q82.** `ρ=5/10=.5`; `P0=.5`; `P3=(1-.5)(.5)^3=.0625`.

**Q83.** `P(N≥5)=ρ^5=(.5)^5=.03125`.

**Q84.** `N=Σn(1-ρ)ρ^n = ρ/(1-ρ)`, obtained by differentiating the geometric series or using its standard sum.

**Q85.** Little's Law gives `T=N/λ=[ρ/(1-ρ)]/λ`. Substitute `ρ=λ/μ` and simplify: `T=1/(μ-λ)`.

**Q86.** `W=T-1/μ = 1/(μ-λ)-1/μ = λ/[μ(μ-λ)]`.

**Q87.** `T` is total time in system = waiting + service. `W` is waiting before service begins. Mean service time is `1/μ`. Therefore `T=W+1/μ`.

**Q88.** Substitute results: `W+1/μ=λ/[μ(μ-λ)]+1/μ = [λ+μ-λ]/[μ(μ-λ)] =1/(μ-λ)=T`.

**Q89.** `λ=100`, `μ=125`. (a) `ρ=.8`. (b) `N=.8/.2=4`. (c) `T=1/25=.04s=40ms`. (d) `W=40−8=32ms`. (e) `Nq=λW=100(.032)=3.2`.

**Q90.** `λ=900`, `μ=1000`: (a) `ρ=.9`; (b) `N=.9/.1=9`; (c) `T=1/100=.01s=10ms`; (d) `W=10−1=9ms`; (e) `Nq=λW=900(.009)=8.1`.

**Q91.** `ρ=.8`, `μ=500`: (a) `λ=.8(500)=400/s`; (b) `N=.8/.2=4`; (c) `T=1/(500−400)=.01s=10ms`; (d) `W=10−2=8ms`.

**Q92.** `T=.01=1/(μ−λ)`, so `λ=μ−100=200−100=100/s`.

**Q93.** Need `1/(μ−90)<.02`, so `μ−90>50`; minimum service rate must be **greater than 140/s** (140/s gives exactly 20 ms).

**Q94.** `λ=.95(1000)=950/s`. As `ρ→1`, `T=1/(μ−λ)` and queue length grow toward infinity; delay becomes extremely large.

**Q95.** `N=ρ/(1-ρ)`. Router A: `.5/.5=1`; Router B: `.9/.1=9`. B has nine times A's average system queue length despite only 1.8 times utilization.

**Q96.** (a) `ρ=125/500=.25`. (b) `P(n)=(.75)(.25)^n`. (c) `P(N>13)=ρ^14=(.25)^14≈3.73×10^-9`.

**Q97.** `P(N>13)=ρ^14=(.25)^14≈3.73×10^-9`.

**Q98.** For finite buffer capacity `K` (maximum packets in system), use the M/M/1/K stationary distribution/loss probability. Increase `K` until the full-buffer probability `P_K` is below `10^-6`; for an infinite-tail approximation use `P(N≥K)≈ρ^K` and solve `ρ^K≤10^-6`, then round up. State the buffer convention clearly.

**Q99.** M/M/1 stationary probabilities are geometric: `P(n)=(1-ρ)ρ^n`. Since `0<ρ<1`, each additional buffer position multiplies the tail probability by `ρ`, causing exponential/geometric decay.

## K–L. M/M/m queues (Q100–Q114)

**Q100.** M/M/m has Poisson arrivals, exponential service times, and `m` identical parallel servers sharing the workload/queue.

**Q101.** M/M/1 has one server and service capacity `μ`. M/M/m has `m` servers and maximum capacity `mμ`; multiple customers can be served simultaneously.

**Q102.** If `n<m` customers are present, only `n` servers are busy, so total departure rate is `nμ`. If `n≥m`, all are busy and total departure rate is `mμ`.

**Q103.** Aggregate maximum capacity = `4×10=40 customers/hour`.

**Q104.** `ρ=λ/(mμ)=10/(5×3)=2/3≈0.667`.

**Q105.** It is the fraction of total available server capacity being offered by arrivals: arrival rate divided by `m` servers times per-server rate. It is also approximate average fraction of servers busy under stability.

**Q106.** If `λ≥mμ`, work arrives as fast as or faster than maximum combined service. The queue cannot have a finite steady state.

**Q107.** Let `a=λ/μ=mρ`. For `n<m`, `P_n=(a^n/n!)P0`; for `n≥m`, the birth-death tail is geometric with ratio `ρ`. Normalizing both parts gives `P0=[Σ_(n=0)^(m-1)a^n/n! + a^m/(m!(1-ρ))]^-1`.

**Q108.** `ρ=10/(2×8)=.625<1`, so it is stable.

**Q109.** Erlang-C is `P(N≥m)`: probability all servers are busy when an arrival occurs, so the arrival must wait in queue (assuming no loss and FCFS).

**Q110.** Parallel servers increase aggregate service capacity and allow several customers to be served at once. For the same load, this lowers probability of waiting and average queueing delay, though it adds cost/coordination.

**Q111.** `λ=10/h`, `m=5`, `μ=3/h`; `a=λ/μ=3.333`, `ρ=2/3`.

- (a) utilization = `2/3=0.6667`.
- (b) `P0=[Σ_(0)^4 a^n/n! + a^5/(5!(1-ρ))]^-1≈0.0318`.
- (c) all terminals busy/Erlang-C probability `≈0.3266`.
- (d) average waiting `Nq = Erlang-C×ρ/(1-ρ)≈0.653` students.
- (e) average in system `N=Nq+a≈0.653+3.333=3.986` students.

**Q112.** `ρ=100/(4×30)=100/120=.8333<1`; the system is stable.

**Q113.** `ρ=20/(4×6)=20/24=.8333`, or 83.33% utilization.

**Q114.** Both have total nominal capacity `2μ`, but one rate-`2μ` server completes any job faster once it begins, while two rate-`μ` servers serve two jobs concurrently. Compare service-time requirement, waiting probability/Erlang-C, reliability, cost, load balancing, parallelism, and whether jobs can be split. They are not generally identical in delay distribution.

## M–N. Scenarios and switching fabrics (Q115–Q128)

**Q115.** M/M/1 is a reasonable first approximation if requests arrive independently at roughly constant average rate, service times are approximately exponential, and one effective server processes them. Use M/M/m instead if several parallel workers serve a common queue.

**Q116.** As `λ→μ` in M/M/1, `ρ→1`; server utilization approaches 100%, queue length and delay rise without bound in the ideal model, and any finite real buffer increasingly overflows, causing packet loss.

**Q117.** M/M/1 predicts benefit of raising one server's `μ`: `T=1/(μ-λ)`. M/M/m models adding parallel servers: total capacity becomes `mμ` and Erlang-C gives waiting. Compare resulting delay, queue length, cost, reliability, and whether workload can be parallelized.

**Q118.** A single queue feeding five counters is conceptually an **M/M/5** system if arrivals/service are modeled as Poisson/exponential.

**Q119.** Five independent queues are five separate M/M/1-like systems, with customers choosing queues. A single-queue M/M/5 shares work dynamically across all free servers and generally balances load better.

**Q120.** In a shared queue, the next free server takes the next customer. This prevents one server's queue from being long while another server is idle, reducing imbalance and often reducing average waiting.

**Q121.** Queues arise when packets contend for the same output/crossbar resource or arrive faster than the fabric/output can serve them. Buffers temporarily hold excess packets.

**Q122.** One FIFO at input means a blocked HOL packet prevents following packets from reaching otherwise free outputs. Those unused output opportunities reduce number of transfers and hence throughput.

**Q123.** (a) Scheduler selects one winner and it crosses to the requested output. (b) Losing packet remains queued. (c) In FIFO, packets behind it may also be blocked if it remains at the head, even if their outputs are free.

**Q124.** VOQ separates packets by desired output at each input. A blocked queue for one output does not prevent the scheduler from choosing another queue at the same input whose output is free.

**Q125.** `N²` VOQs: `N` output-specific queues at each of `N` inputs.

**Q126.** `8²=64` queues.

**Q127.** A 32×32 FIFO input switch can suffer HOL blocking: blocked heads trap packets for free outputs, while output conflicts allow only one requester. Therefore its capacity is not fully schedulable; classical uniform traffic limit is about 58.6%.

**Q128.** Queueing theory models arrival, service, queue length, delay, and loss. Switch-fabric scheduling decides service opportunities/matchings. Together they explain how contention and scheduling policy determine queue delay and throughput.

## O. Input-queued throughput (Q129–Q138)

**Q129.** Classical assumptions: `N×N` crossbar; fixed-size packets and slots; one FIFO per input; only HOL packet considered; one packet per input/output per slot; uniformly independently distributed destinations; saturated/random offered traffic.

**Q130.** `F_m` is the number of packets successfully transferred in slot `m`. `B_(i,m)` is the number of packets destined for output `i` that are blocked/not transmitted in slot `m` under the analysis definition.

**Q131.** From the ideal `N` output opportunities in a slot, subtract blocked/unserved opportunities over all outputs: `F_m=N-Σ_i B_(i,m)`.

**Q132.** Take expectation: `E[F]=N-ΣE[B_i]`. Symmetry gives the same `E[B_i]` for each of N outputs, so `E[F]=N-NE[B_i]=N(1-E[B_i])`.

**Q133.** Equate `Nρ=N(1-E[B_i])`; cancel N: `ρ=1-E[B_i]`.

**Q134.** Uniform random destinations cause repeated output conflicts. FIFO prevents packets behind blocked HOL packets from using free outputs. Classical equilibrium analysis of these blocked requests gives `ρ≈0.586`, so only 58.6% of ideal capacity is used.

**Q135.** `E[F]=Nρ=0.586N` packets/slot.

**Q136.** `E[F]=8(.586)=4.688` packets/slot on average.

**Q137.** `E[F]=16(.586)=9.376` packets/slot on average.

**Q138.** More ports add both outputs and competing input requests. FIFO HOL coupling remains at every input, so increasing N does not let blocked packets bypass their heads; it does not remove the structural cause.

## P. True/false and integrated questions (Q139–Q150)

**Q139.** **False.** A finite buffer can fill during a burst or when arrival rate exceeds service temporarily; packets arriving to a full buffer are lost. Buffer space reduces loss but cannot guarantee zero loss.

**Q140.** **False.** `T=1/[μ(1-ρ)]`. At .5, `T=2/μ`; at .9, `T=10/μ`, a fivefold (400%) increase, not 40%.

**Q141.** **True.** Inter-arrival times of a Poisson process are exponential.

**Q142.** **True.** Exponential is a waiting-time distribution and is memoryless. Poisson is a discrete distribution for the number of events in an interval, not a waiting-time distribution.

**Q143.** **False.** Little's Law requires stable long-run averages, not Poisson arrivals.

**Q144.** **False.** With fixed `λ` and fixed per-server `μ`, adding servers reduces `ρ=λ/(mμ)`. But if workload/arrival rate changes or servers are not equally utilized, the statement needs those conditions; it is not an unconditional law.

**Q145.** **True.** This exactly describes HOL blocking in input FIFO queueing.

**Q146.** **False.** Output queueing places packets at their destination output; it does not have input-FIFO HOL blocking. Its issue is high speed/memory bandwidth requirement.

**Q147.** M/M/1 applies. `λ=800`, `μ=1000`, `ρ=.8`.

1. Model: M/M/1.
2. Utilization: `.8` (80%).
3. Empty probability: `P0=1-.8=.2`.
4. Exactly 3 packets: `P3=(.2)(.8)^3=.1024`.
5. Average number: `N=.8/.2=4`.
6. Average delay: `T=1/(1000-800)=.005s=5ms`.
7. Queueing delay: `W=5−1=4ms`.
8. Average waiting: `Nq=λW=800(.004)=3.2` packets.

**Q148.** Mean service time is 1.5 ms = .0015 s, so `μ=1/.0015=666.667/s`; `λ=500/s`; `ρ=.75`.

1. Service rate: `666.667 packets/s`.
2. Utilization: `.75`.
3. Empty probability: `.25`.
4. Average number: `N=.75/.25=3`.
5. Average delay: `T=1/(666.667-500)=.006s=6ms`.
6. Queueing delay: `W=6−1.5=4.5ms`.
7. Average waiting: `Nq=500(.0045)=2.25` packets.
8. `P(N>10)=ρ^11=.75^11≈0.0422`.

**Q149.** `λ=900/s`.

| Measure | Option A: M/M/1, `μ=2000` | Option B: M/M/2, each `μ=1000` |
|---|---:|---:|
| Utilization | `.45` | `.45` |
| Average time in system | `1/(2000-900)=0.909 ms` | `≈1.254 ms` |
| Average queueing time | `≈0.409 ms` | `≈0.254 ms` |
| Average in queue | `≈0.368` | `≈0.229` |
| Average in system | `≈0.818` | `≈1.129` |

For Option B, `P0≈0.3793` and Erlang-C waiting probability `≈0.2793`; `Wq=C/(2000-900)≈0.254 ms`. One faster server gives shorter service time and lower total delay here; two servers give lower waiting time and better resilience/parallelism. The appropriate choice also depends on cost and reliability.

**Q150.**

1. **HOL blocking:** a head packet waiting for a busy output blocks later FIFO packets for free outputs.
2. Outputs can remain idle because an eligible packet is trapped behind a blocked HOL packet; raw crossbar connectivity cannot bypass FIFO order.
3. `F_m` is the number of packets transferred successfully in slot `m`.
4. `B_(i,m)` is the number of packets/request opportunities for output `i` that are blocked/not transmitted in slot `m`.
5. `F_m=N-Σ_iB_(i,m)`. Taking expectations and using symmetry, `E[F]=N-N E[B_i]=N(1-E[B_i])`.
6. `E[F]=Nρ` means average successful transfers equal number of ports times per-port utilization/throughput `ρ`.
7. Under classical uniform random-destination FIFO-input assumptions, `ρ≈0.586`; maximum average switch throughput is about **58.6% of ideal**, or `0.586N` packets/slot.
