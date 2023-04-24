### Overview

We formally model and verify properties of the CSMA/CD (Carrier Sense Multiple
Access with Collision Detection) network protocol, using the Edinburgh
Concurrency Workbench (CWB) [1].

The implemented protocol is based on clause 4 of the IEEE 802.3 (Ethernet)
network standard. It involves the communication between two stations/parties,
MAC1 and MAC2, sending messages via a shared channel. In order to send a
message, a station must obtain exclusive use of the channel; it attempts to do
so by going through a *contention* period during which both stations can freely
attempt to transmit. A *collision* occurs when both parties attempt to transmit
during the *contention* period, at which point the *contention* period is
restarted and channel acquisition is retried. A station has acquired the channel
if it manages to transmit beyond the *contention* period without *collisions*.

### Formal protocol

The protocol is modeled using a process calculus, the Calculus of Communicating
Systems (CCS) [1], within CWB. It involves three parties (MAC1, MAC2, and the
shared medium). Each station is modeled as two CCS processes (a transmitter and a
receiver) executing concurrently. The full system is thus modelled as five
concurrent CCS processes. These exchang the following messages:

* `send`: submit a message to the channel;
* `begin`: begin exclusive use of the channel;
* `end`: end exclusive use of the channel;
* `bt` (begin transmission): channel begins message transmission;
* `et` (end transmission): channel ends message transmission;
* `br` (begin reception): receiver begins receiving message;
* `er` (end reception): receiver ends receiving message;
* `rec` (receive): message successfully and fully received;
* `c` (collision): collision occurred;
* `acq` (acquisition): successful acquisition of the channel;
* `bad`: message should be discarded due to collision having occurred;

More information about the way these messages are used to implement the protocol
can be found in CSMA.pdf. While the text is in italian, the figures and formulas
hopefully serve as sufficient explanation.

### Formal verification

The Edinburgh Concurrency Workbench (CWB) is an automated tool for analyzing
networks of finite-state processes expressed in Milner’s Calculus of
Communicating Systems (CCS), using a variety of different verification methods,
including equivalence checking, preorder checking, and model checking. In
particular, it allows to express specifications using *modal temporal logic*,
and then verify that the formalized processes meet those specifications.

The following properties of the modeled CSMA protocol are assessed and verified.

* W.r.t. a reference specification (half-duplex binary semaphore):

    * Strong and weak bisimilarity
    * Observational congruence
    * Weak trace equivalence

* As properties of the protocol itself

    * Deadlocks
	* Livelocks
	* Mutual exclusive access to the medium
	* Transmissions are always followed by receive

### References

[1] https://dl.acm.org/doi/pdf/10.1145/151646.151648

[2] https://en.wikipedia.org/wiki/Calculus_of_communicating_systems
