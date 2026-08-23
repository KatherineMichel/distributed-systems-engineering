# Martin Kleppmann Glossary (ChatGPT)

Absolutely. I’d structure it around the **actual terminology used in Kleppmann’s Cambridge Distributed Systems course**, rather than making a generic distributed-systems dictionary. The course’s eight distributed-systems lectures cover models/failures, time and clocks, broadcast, replication, consensus/Raft, consistency, and case studies such as Spanner. 

I’ve also emphasized distinctions that are easy to blur together—**latency vs. bandwidth, fault vs. failure, causality vs. ordering, linearizability vs. eventual consistency, and broadcast vs. consensus**.

# Distributed Systems Glossary

### Martin Kleppmann — Cambridge Distributed Systems Course

*A study glossary of the principal terminology, concepts, algorithms, and abstractions used in Martin Kleppmann’s Distributed Systems course.*

---

## 1. Foundations

### Distributed system

A system consisting of multiple **nodes** that communicate over a network and cooperate to accomplish a task. Unlike a single computer, the components communicate primarily through message passing and can fail independently.

### Node

A communicating computing device participating in a distributed system. A node could be a server, desktop, phone, sensor, vehicle, or other computer.

### Process

A running program or computational entity on a node. Distributed algorithms generally reason about processes communicating by sending messages.

### Message

A unit of information sent from one node or process to another.

### Message passing

A communication model in which processes exchange information by explicitly sending and receiving messages rather than accessing shared memory.

### Shared-memory concurrency

Concurrency between processes or threads that can access the same memory. Distributed systems generally lack this property because each node has its own address space.

### Network

The communication medium connecting nodes. In the distributed-systems model, its physical implementation is abstracted away.

### Distributed algorithm

An algorithm in which multiple nodes execute concurrently and coordinate through communication.

### Latency

The time between sending a message and receiving it.

**Key distinction:** latency measures *how long communication takes*.

### Bandwidth

The amount of data that can be transferred per unit of time.

**Key distinction:** bandwidth measures *how much data can be transferred per unit time*.

### Network delay

The time required for a message to travel through the communication system.

### Unbounded delay

The possibility that a message may take an arbitrarily long time to arrive. A message that has not arrived may therefore be delayed rather than lost.

### Nondeterminism

The fact that the exact ordering and timing of events cannot necessarily be predicted. Distributed systems contain substantial nondeterminism because nodes and networks operate independently.

---

# 2. Failures and System Models

### Fault

A defect or malfunction affecting part of a system.

### Failure

A fault becoming observable as incorrect or unavailable behavior.

### Partial failure

A failure in which some components of a distributed system fail while others continue operating.

**This is one of the fundamental difficulties of distributed systems.**

### Crash failure

A node stops executing and does not recover during the relevant period.

### Crash-stop model

A model in which a node can crash, but once crashed it never returns.

### Crash-recovery model

A model in which a node can crash and subsequently restart.

### Omission failure

A failure in which a process or communication channel fails to send or receive a message that should have been delivered.

### Timing failure

A failure involving an operation taking longer than its specified timing bound.

### Byzantine failure

A node behaves arbitrarily, potentially sending contradictory or malicious messages to different nodes.

### Byzantine fault tolerance

The ability of a distributed system to continue satisfying its guarantees despite Byzantine failures.

### Fault tolerance

The ability of a system to continue providing its intended service despite some components failing.

### High availability

The property that a service remains accessible and operational despite failures.

### Failure detector

A mechanism that attempts to determine whether another node has failed.

### False positive

A failure detector reports that a node has failed when it has not.

### False negative

A failure detector fails to report that a node has actually failed.

### Fault model

A set of assumptions describing what kinds of failures the system must tolerate.

### System model

A set of assumptions about how nodes, communication, timing, and failures behave.

### Synchronous system

A system in which known bounds exist on message-delivery time and process execution time.

### Asynchronous system

A system in which there are no known upper bounds on message-delivery or process-execution time.

### Partially synchronous system

A system in which timing guarantees may eventually hold, even though they are not necessarily guaranteed at all times.

---

# 3. The Two Generals and Byzantine Generals Problems

### Two Generals Problem

A thought experiment demonstrating the difficulty of achieving reliable agreement over an unreliable communication channel.

The key issue is that acknowledgements themselves require acknowledgement, producing an infinite regress.

### Common knowledge

Knowledge that all participants know, all know that everyone knows, and so on.

The Two Generals Problem illustrates why communication over an unreliable channel cannot easily establish common knowledge.

### Byzantine Generals Problem

The problem of achieving agreement among distributed participants when some participants may behave arbitrarily or maliciously.

### Byzantine agreement

An agreement protocol that tolerates Byzantine participants.

### Correct process

A process that follows the protocol and does not experience a failure covered by the system's fault model.

---

# 4. Remote Procedure Calls

### Remote Procedure Call (RPC)

A mechanism that makes a function call on another node appear similar to a local function call.

### RPC client

The node initiating the remote procedure call.

### RPC server

The node executing the requested operation.

### Stub

A local proxy for a remote function. The stub converts a local-looking function call into a network request.

### Marshalling

Encoding function arguments or data structures into a representation suitable for transmission.

### Unmarshalling

Decoding a received representation back into usable data structures.

### Middleware

Software providing abstractions and services between an application and the underlying communication mechanisms.

RPC frameworks are one example of middleware.

### Interface Definition Language (IDL)

A language used to formally specify the interface of a distributed service, including operations and their parameters.

### Location transparency

The abstraction that hides where a resource or service is physically located.

### At-most-once semantics

An RPC operation is executed no more than once, although it may not execute at all.

### At-least-once semantics

An RPC operation is guaranteed to be attempted, but it may execute multiple times.

### Exactly-once semantics

The conceptual guarantee that an operation takes effect exactly once.

In real distributed systems, exactly-once semantics are difficult to provide end-to-end.

### Retry

Sending an operation again after an unsuccessful or timed-out attempt.

### Idempotence

An operation is idempotent if performing it multiple times has the same externally visible effect as performing it once.

**Example:** setting `x = 5` is naturally idempotent; charging a credit card £100 is not.

### Timeout

A limit after which a process stops waiting for a response.

### Request-response protocol

A communication pattern in which a client sends a request and the server returns a response.

---

# 5. Time and Clocks

### Physical clock

A clock intended to represent real-world time.

### Clock skew

The difference between the readings of two physical clocks.

### Clock drift

The rate at which a physical clock gains or loses time relative to a reference clock.

### Clock synchronisation

The process of bringing clocks on different nodes into agreement.

### Monotonic clock

A clock whose value never moves backward.

Monotonic clocks are useful for measuring durations and timeouts.

### Wall-clock time

A representation of calendar time, such as UTC.

### UTC

Coordinated Universal Time, the global reference time used for civil timekeeping.

### NTP

The Network Time Protocol, used to synchronise computer clocks over a network.

### Leap second

An adjustment occasionally made to UTC to keep it approximately aligned with Earth's rotation.

**Distributed-systems significance:** physical time can jump or behave unexpectedly, making wall-clock time dangerous for measuring elapsed durations.

---

# 6. Ordering and Causality

### Event

An action occurring within a distributed system, such as sending or receiving a message.

### Happens-before relation

Lamport's relation describing causal ordering between events.

Written as:

`a → b`

when event `a` causally precedes event `b`.

### Causality

The relationship in which one event can be considered to have influenced another.

### Concurrent events

Events for which neither happens-before the other.

If:

`a ↛ b` and `b ↛ a`

then `a` and `b` are concurrent.

### Logical clock

A clock that represents the causal ordering of events rather than physical time.

### Lamport clock

A logical clock assigning monotonically increasing integer timestamps to events.

### Lamport timestamp

The integer assigned to an event by a Lamport logical clock.

A fundamental property is:

`a → b  ⇒  L(a) < L(b)`

However, the converse is not necessarily true.

### Total order

An ordering in which every pair of events is comparable.

### Partial order

An ordering in which some pairs of events may be incomparable.

### Total-order relation

A relation that orders every pair of elements.

### Causal order

An ordering that respects the happens-before relationship.

### Vector clock

A logical clock containing one counter per process. Vector clocks can represent causal relationships more precisely than Lamport clocks.

### Vector timestamp

The vector assigned to an event by a vector clock.

### Causal consistency

A consistency model in which causally related operations are observed in causal order.

---

# 7. Broadcast

### Broadcast

Sending a message to multiple nodes.

### Reliable broadcast

A broadcast abstraction providing guarantees about which messages are eventually delivered.

### FIFO broadcast

A broadcast abstraction preserving the sender's order of messages.

If a sender broadcasts `m1` before `m2`, recipients deliver `m1` before `m2`.

### Causal broadcast

A broadcast abstraction preserving causal ordering.

If broadcasting `m1` causally precedes broadcasting `m2`, recipients deliver `m1` before `m2`.

### Total-order broadcast

A broadcast abstraction in which all nodes deliver messages in the same order.

### FIFO-total-order broadcast

A total-order broadcast that additionally preserves the sender's FIFO ordering.

### Delivery order

The order in which messages are delivered to an application.

### Broadcast protocol

A distributed protocol implementing a particular set of broadcast guarantees.

### Causal delivery

Delivering messages in an order that respects their causal dependencies.

---

# 8. Replication

### Replication

Maintaining multiple copies of data or service state on different nodes.

### Replica

One copy of replicated data or state.

### Primary

A replica designated to coordinate writes or otherwise act as the authoritative replica.

### Follower

A replica that receives state updates from another replica, commonly a primary or leader.

### Leader

A node elected or designated to coordinate operations among replicas.

### Primary-backup replication

A replication architecture in which a primary handles operations and backups maintain copies of its state.

### State machine replication

Replicating a deterministic state machine across multiple nodes by ensuring that all replicas process the same operations in the same order.

### Quorum

A subset of replicas whose participation is sufficient for an operation to be considered successful.

### Read quorum

The number of replicas that must participate in a read.

### Write quorum

The number of replicas that must participate in a write.

### Quorum intersection

The requirement that read and write quorums overlap sufficiently to guarantee that relevant replicas share information.

### Replication factor

The number of replicas maintained for a piece of data.

### Failover

Switching service from a failed node to another node.

### Replica divergence

A situation in which replicas contain different states.

### Conflict

A situation in which different replicas have accepted incompatible updates.

---

# 9. Consensus

### Consensus

The problem of getting multiple distributed processes to agree on a single value despite failures.

A typical consensus abstraction has three important properties:

* **Agreement:** correct processes decide the same value.
* **Validity:** the chosen value satisfies the protocol's validity rule.
* **Termination:** correct processes eventually decide.

### Agreement

No two correct processes decide different values.

### Validity

The decided value must satisfy the algorithm's requirements regarding proposed values.

### Termination

Correct processes eventually reach a decision.

### Consensus algorithm

An algorithm allowing distributed nodes to reach agreement despite specified failures.

### Raft

A consensus algorithm designed to be understandable and practical. It uses a leader, replicated logs, elections, and terms.

### Raft leader

The Raft node responsible for accepting client commands and replicating log entries.

### Raft follower

A Raft node that receives replicated log entries and responds to the leader.

### Raft candidate

A Raft node attempting to become leader.

### Raft term

A monotonically increasing logical period in Raft. Terms help nodes identify stale leaders and messages.

### Leader election

The process by which nodes select a leader.

### Election timeout

A randomized timeout used by Raft followers to determine when they should begin an election.

### Raft log

The sequence of commands maintained by Raft replicas.

### Log entry

A command stored in the Raft replicated log.

### Log replication

The process of sending log entries from the leader to followers.

### Committed entry

A Raft log entry that is guaranteed to be durable and agreed upon according to Raft's commitment rules.

### State machine

A deterministic computation that changes state in response to commands.

### State machine replication

Running identical state machines on multiple nodes while ensuring they process the same commands in the same order.

---

# 10. Two-Phase Commit

### Distributed transaction

A transaction whose operations involve multiple nodes or services.

### Transaction

A group of operations treated as a single logical unit.

### Atomicity

The property that a transaction's effects occur entirely or not at all.

### Two-Phase Commit (2PC)

A distributed transaction protocol that coordinates participants so that they either all commit or all abort.

### Coordinator

The node coordinating a two-phase commit transaction.

### Participant

A node participating in a distributed transaction.

### Prepare phase

The first phase of 2PC. Participants determine whether they are able to commit and record the necessary state.

### Commit phase

The second phase of 2PC. The coordinator instructs participants to commit or abort.

### Prepared state

A participant state in which it has agreed to commit if instructed by the coordinator.

### Blocking

A situation in which a participant cannot safely make progress because it lacks information about the final transaction decision.

**Important:** 2PC provides atomic commitment, but it can block if the coordinator fails at an unfortunate time.

---

# 11. Consistency

### Replica consistency

The degree to which different replicas present compatible views of system state.

### Consistency model

A specification describing what values and operation orderings clients are allowed to observe.

### Linearizability

A consistency guarantee in which each operation appears to take effect instantaneously at some point between its invocation and response.

Linearizability preserves real-time ordering between non-overlapping operations.

### Linearization point

The conceptual instant at which a linearizable operation takes effect.

### Sequential consistency

A consistency model requiring operations to appear in some sequential order consistent with each individual process's program order.

Unlike linearizability, sequential consistency does not necessarily preserve real-time ordering across processes.

### Eventual consistency

A consistency model in which replicas may temporarily disagree, but if updates stop, replicas eventually converge to the same state.

### Strong consistency

An informal term generally referring to guarantees that make replicated data appear closely synchronized.

The exact meaning depends on the consistency model being discussed.

### Weak consistency

A broad term for models that provide fewer guarantees about what different clients may observe.

### Read-after-write consistency

A guarantee that after a client writes a value, subsequent reads by that client observe that write.

### Monotonic reads

A guarantee that once a client has observed a particular version of data, later reads do not return an older version.

### Monotonic writes

A guarantee that writes from a single client are applied in the order issued.

### Session consistency

A family of guarantees relating consistency to a particular client's session.

---

# 12. Collaboration and Conflict Resolution

### Concurrent update

An update that occurs without being causally ordered after another update.

### Conflict resolution

The process of determining a resulting state when replicas have accepted incompatible concurrent updates.

### Last-write-wins (LWW)

A conflict-resolution strategy that chooses one update based on a timestamp or ordering mechanism.

**Caution:** using physical timestamps can produce surprising results because clocks are imperfect.

### Multi-value register

A replicated data structure that retains multiple concurrent values rather than arbitrarily selecting one.

### Conflict-free Replicated Data Type (CRDT)

A replicated data structure designed so that independently performed updates can be merged automatically while preserving convergence properties.

### Convergent CRDT

A CRDT in which replicas converge by merging their states.

### Commutative operation

An operation where:

`a ∘ b = b ∘ a`

Commutativity is useful for distributed systems because independently reordered operations can produce the same result.

### Associative operation

An operation where:

`(a ∘ b) ∘ c = a ∘ (b ∘ c)`

Associativity makes repeated merging easier to reason about.

### Idempotent operation

An operation that can be applied repeatedly without changing the result after the first application.

---

# 13. Spanner and Physical Time

### Google Spanner

A globally distributed database system designed to provide strong consistency across geographically distributed data.

### TrueTime

Spanner's time API, which represents the current time as an interval rather than a single exact timestamp.

### Time interval

A representation of time using an earliest and latest possible value.

### External consistency

A property of Spanner in which transactions respect real-time ordering while maintaining distributed transactional consistency.

### Commit-wait

A technique in which Spanner waits until its TrueTime uncertainty interval has passed before considering a transaction committed with its chosen timestamp.

### Clock uncertainty

The uncertainty about the exact current physical time.

### Atomic clock

A highly precise clock based on atomic transitions.

### GPS time source

A source of precise time derived from GPS signals.

---

# 14. Important Conceptual Distinctions

### Fault vs. failure

A **fault** is the underlying problem; a **failure** is the externally observable consequence.

### Failure vs. partial failure

A traditional single computer often fails as a whole. A distributed system can experience **partial failure**, where one component fails while the rest continues operating.

### Latency vs. bandwidth

**Latency** is how long it takes to move data.

**Bandwidth** is how much data can be moved per unit time.

### Physical time vs. logical time

**Physical time** attempts to represent real-world time.

**Logical time** represents ordering and causality between events.

### Causality vs. total ordering

Causality only requires related events to be ordered. A total order additionally orders events that may be causally unrelated.

### Lamport clocks vs. vector clocks

Lamport clocks can tell us that:

`a → b  ⇒  L(a) < L(b)`

but they cannot reliably determine whether two events are concurrent.

Vector clocks can represent that distinction.

### FIFO order vs. causal order

FIFO ordering preserves the order from one sender.

Causal ordering can preserve dependencies across multiple senders.

### Total-order broadcast vs. consensus

Total-order broadcast ensures that nodes deliver messages in the same order.

Consensus solves the problem of getting nodes to agree on a value.

They are closely related abstractions and can be used to implement one another under appropriate assumptions.

### Replication vs. consensus

Replication creates multiple copies of state.

Consensus determines an agreed-upon sequence or decision despite failures.

A replicated state machine commonly uses consensus to decide the order of commands.

### Linearizability vs. eventual consistency

**Linearizability:** operations behave as though there were one up-to-date copy of the object.

**Eventual consistency:** replicas may temporarily disagree but converge if updates cease.

### RPC vs. local function call

An RPC may look like a local function call syntactically, but it has fundamentally different failure behavior because communication, the remote process, and the network can fail independently.

### 2PC vs. consensus

Two-phase commit coordinates a distributed transaction.

Consensus establishes agreement despite certain failures.

2PC can block when its coordinator fails; consensus algorithms such as Raft are designed specifically to make progress despite specified node failures.

---

# 15. The Core Mental Model

The most important chain of ideas in the course can be summarized as:

**Multiple nodes**
→ **message passing**
→ **independent execution**
→ **unpredictable communication**
→ **partial failures**
→ **no universally reliable notion of time**
→ **need to reason about event ordering**
→ **broadcast and replication**
→ **need for agreement**
→ **consensus**
→ **replicated state**
→ **consistency guarantees**

The central problem is not simply:

> "How do I get several computers to communicate?"

It is:

> **How can several independent computers coordinate correctly when communication is delayed, messages can be lost, nodes can fail, and there is no single universally shared clock?**

That question connects essentially every major concept in the course.

---

## High-Priority Terms to Memorize

If studying for the course, these are the terms I would know **cold**:

1. Distributed system
2. Node
3. Message passing
4. Latency
5. Bandwidth
6. Partial failure
7. Fault tolerance
8. Asynchronous system
9. Byzantine failure
10. RPC
11. Marshalling
12. Location transparency
13. Timeout
14. Idempotence
15. Physical clock
16. Clock skew
17. Clock drift
18. Monotonic clock
19. NTP
20. Happens-before
21. Causality
22. Concurrent events
23. Logical clock
24. Lamport clock
25. Vector clock
26. FIFO broadcast
27. Causal broadcast
28. Total-order broadcast
29. Replication
30. Replica
31. Quorum
32. Consensus
33. Agreement
34. Validity
35. Termination
36. Raft
37. Leader election
38. Raft term
39. Raft log
40. State machine replication
41. Two-phase commit
42. Coordinator
43. Participant
44. Linearizability
45. Sequential consistency
46. Eventual consistency
47. Read-after-write consistency
48. Conflict
49. CRDT
50. Spanner
51. TrueTime
52. External consistency
53. Commit-wait

This is based primarily on Kleppmann's Cambridge course notes and the current Cambridge course description; the published notes explicitly organize the material into the eight areas above, from distributed-system models through Spanner. 

If you're using this to study the course you're currently taking, **the most useful next version would be a "Kleppmann Distributed Systems Cheat Sheet" where each term has: definition → why it matters → example → common misconception → relationship to other terms.**
