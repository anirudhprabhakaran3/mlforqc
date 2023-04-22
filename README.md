# Quantum Algorithms

## Quantum Teleportation

Quantum teleportation is a process of transmitting quantum information from one location to another without physically moving the information carrier. In this process, the quantum state of a particle is transferred from one location to another, typically over a distance that is much larger than the physical size of the particle.

The process of quantum teleportation involves the use of two entangled particles, referred to as the "entangled pair" or "Bell pair", which have a special correlation, or entanglement, that allows for the teleportation to occur.

Here are the basic steps of the quantum teleportation process:

1. Preparation: The sender (Alice) and receiver (Bob) each have one particle each from the entangled pair. A third particle, which carries the quantum information to be teleported, is also prepared. This particle is typically referred to as the "input" or "teleportee" particle.

2. Entanglement: Alice performs a joint measurement on the input particle and her particle from the entangled pair. This measurement results in the destruction of the original state of the input particle, but creates a new entangled pair between Alice's particle and the teleportee particle.

3. Communication: Alice communicates the results of her measurement to Bob over a classical communication channel. This information is used to perform a quantum operation on Bob's particle from the entangled pair, which creates a new state for the teleportee particle.

4. Measurement: Bob performs a measurement on the teleportee particle, which results in the teleportation of the original state to Bob's particle from the entangled pair.


The quantum teleportation protocol can be described mathematically using the following equations:

The state of the input particle to be teleported is represented as: Q: |ψ⟩ = α|0⟩ + β|1⟩,

where α and β are complex numbers that represent the probability amplitudes of the two basis states.

The initial state of the entangled pair is represented as: q1,q2: |Φ+⟩ = (|00⟩ + |11⟩)/√2,

where |0⟩ and |1⟩ are the two basis states of a qubit.

The combined state of the 3 qubits Q, q1, q2 is: |ψ⟩ ⊗|Φ+⟩ = (1/√2) [ α(|000⟩+|011⟩) + β(|110⟩+|101⟩) ]

We then apply the Hadamard gate H to Q to get the final amplitudes in a desirable form, to get the final combined state as:

Combined state : (1/2) [ |00⟩[ α|0⟩+β|1⟩ ] + |01⟩[ β|0⟩+α|1⟩ ] + |10⟩[ α|0⟩-β|1⟩ ] + |11⟩[ -β|0⟩+α|1⟩ ] ]

We notice that if we measure Q and q1, and let the combined state of the 3 qubits collapse, the resulting state of q2 can always be converted to the initial state of Q through basic quantum gates.

* We can leave q2 as it is if the measurement result of Q, q1 is |00⟩,
* We can apply an X gate when it is |01⟩
* We can apply a Z gate when it is |10⟩
* We can and apply gates X and Z consecutively when the measurement result is |11⟩.

Hence, by conveying the result of the measurement to Bob, appropriate gates can be applied to q2 which helps us teleport the original state of Q to Bob. The initial state of Q will been altered due to observations, which is consistent with the No Cloning Theorem.

-----------------------------------------

## Grover's Search Algorithm

Grover's algorithm is a quantum algorithm to search through an unstructured data structure in O(root(N)) time, as compared to the O(N) time of classical algorithms.

To do this, we use an oracle, a function which negates the phases of only those states which we desire to find through the algorithm. We want to create the phase Oracle in such a way that when we make a query, if the query input is the winner, then we will negate the amplitude of the winner in the superposition. If any of the remaining N-1 qubits are queried, the Oracle will not alter them in any way. This is represented in matrix form, by a diagonal matrix of all ones and a -1 for the winner column.

We design the Oracle based on the binary strings that we want to find. In this case, for three qubits, let's say that we want to find the string 101 in this case, we will design the Oracle accordingly.

In the beginning, we have a uniform superposition |s⟩, where the amplitudes of all the quantum states are the same. If there are N quantum states, then the amplitude of each quantum state it 1/√N.

If we were to observe all the qubits, there is (1/√N)^2  = 1/N chance that we will find the winner. Through the Grover's search algorithm,  we can increase the probability of finding the winner to almost 100% upon measurement.

Geometrically speaking, we can decompose the superposition into two components, the winner component |w⟩ and the component with other qubits |r⟩. We can imagine the initial state in a coordinate plane with |r⟩ and |w⟩ as the x- and y-axes.

After that we can follow the following steps to increase the probability of finding the winner:

* Apply query the phase Oracle, which reflects the current superposition state about |r⟩.
* We the apply a reflection gate to perform reflection about teh original state |s⟩. (The net effect of these steps is that the superposition state is rotated by an angle of 2θ towards |w⟩)
* We will repeat these steps t number of times, until we increase the amplitude of the winner state.

It turns out that mathematically t ≈ √N . So the average time complexity is √N.
