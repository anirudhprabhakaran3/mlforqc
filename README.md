# Machine Learning for Quantum Circuit Mapping

Quantum Computing is a newer form of computing that has been getting much more prevalent nowadays. Every quantum circuit contains qubits that are used for various computations. These logical qubits have to be mapped to physical qubits on an actual quantum computer. This is called the quantum circuit mapping problem. However, there are some conditions for this – for any gate to be applicable, the qubits have to be physically adjacent to each other.

This operation is done using SWAP gates, that are made of 3 CNOT gates. However, this is a time consuming and noisy operation. Our aim is to find an ideal mapping between logical and physical qubits given a quantum circuit, using machine learning methods. This gives a heuristic search approach for suboptimal solutions making the approach scalable. 

-----------------------------------------
## Introduction

Quantum computing is an emerging field of computing that utilizes quantum-mechanical phenomena, such as superposition and entanglement, to perform computations. Unlike classical computers, which operate on bits that can only be in two states (0 or 1), quantum computers use qubits that can exist in a superposition of states. This means that a qubit can represent multiple values at the same time, which allows quantum computers to perform certain computations exponentially faster than classical computers.

One of the most famous algorithms in quantum computing is Shor's algorithm, which can factor large numbers exponentially faster than classical algorithms. This has important implications for cryptography, as many encryption algorithms rely on the difficulty of factoring large numbers. Another si Grover's search, which is a quantum algorithm that can search an unsorted database of N items in O(sqrt(N)) time, compared to O(N) time taken by the best classical algorithm.

However, quantum computing is still in its infancy, and many challenges remain before practical quantum computers can be built. These challenges include the fragile nature of qubits, which are susceptible to environmental noise and decoherence, and the difficulty of scaling up quantum systems to perform complex computations. Another is the quantum circuit-mapping problem, which involves determining the best way (using the least number of SWAP Gates) to map logical qubits to physical qubits and selecting a set of available gates that can be used to implement the desired quantum operation.

Despite these challenges, quantum computing holds great promise for solving important problems in various fields, from chemistry to finance to artificial intelligence. As the field continues to develop, it is likely that quantum computing will play an increasingly important role in shaping the future of technology.


-----------------------------------------


## Some basic concepts of Quantum Computing:

### Qubit
A qubit is the basic unit of quantum information. Unlike classical bits, which can only exist in two states (0 or 1), a qubit can exist in a superposition of both states. This means that a qubit can represent both 0 and 1 at the same time, and any linear combination of these states. Qubits are typically represented by vectors in a two-dimensional complex Hilbert space. However, when a qubit is measured, the quantum state of the qubit collapses into one of its basis states, with a probability given by the square of the amplitude of that basis state in the original superposition.


### Bloch Sphere Notation
The Bloch sphere is a geometric representation of the state of a qubit. It is a sphere with a diameter of 1, where the north and south poles represent the states |0⟩ and |1⟩, respectively. Any point on the surface of the sphere can represent a superposition of these two states, with the angle between the point and the north pole representing the probability of measuring the state |0⟩, and the angle between the point and the south pole representing the probability of measuring the state |1⟩. The Bloch sphere notation is a useful way to visualize the state of a qubit and to perform calculations involving qubits.


### Bra-ket Notation
Bra-ket notation is a common way to represent quantum states and operators. A quantum state is represented by a ket, which is a column vector of complex numbers, and is typically denoted as |ψ⟩. The conjugate transpose of a ket is called a bra, which is a row vector denoted as ⟨ψ|. The inner product of a bra and a ket is denoted as ⟨ψ|ϕ⟩, and represents the probability amplitude of measuring the state |ϕ⟩ given that the state |ψ⟩ was prepared. Bra-ket notation is a convenient way to write and manipulate quantum states and operators, and is commonly used in quantum computing.
For example, |ψ⟩ = 0.6|0⟩ + 0.8|1⟩ is a quantum with a probability of (0.6)^2 = 0.36 of collapsing to a |0⟩ state upon measurement, and (0.8)^2 = 0.64 probability of collapsing to a |1⟩ state.


### Tensor Product of qubits
The tensor product of two qubits is a mathematical operation that combines the two individual qubits into a single composite system. To perform the tensor product of two qubits, we write out the basis states of each qubit in bra-ket notation, and then take the tensor product of the two sets of basis states. For example, if we have qubit A in the state |0⟩ and qubit B in the state |1⟩, then the tensor product of these two qubits is written as:
|0⟩ ⊗ |1⟩ = |01⟩
This means that the composite system is in the state where qubit A is in the state |0⟩ and qubit B is in the state |1⟩.
Similarly, we can compute the tensor product of any two qubits by taking the tensor product of their individual basis states. The resulting composite system will have four basis states, which can be written in the form |00⟩, |01⟩, |10⟩, and |11⟩. These four basis states form the computational basis of the composite system, and any state of the composite system can be written as a linear combination of these four basis states.


### No-Cloning Theorem
The no-cloning theorem is a fundamental principle of quantum mechanics which states that it is impossible to create an identical copy of an arbitrary unknown quantum state. In other words, it is impossible to create a process or machine that can take an arbitrary quantum state and produce a second identical copy of that state, without disturbing or altering the original state. This is in contrast to classical information, where copying is trivial operation that can be performed without altering the original information.
	The no-cloning theorem has important implications for quantum algorithms, as it makes it impossible to copy or delete quantum information.

### Quantum Oracle
A quantum oracle is simply an oracle that operates on qubits, instead of classical bits. In other words, it's a black box that takes quantum states as input and returns quantum states as output.
Quantum oracles are a fundamental component in many quantum algorithms where they play a crucial role in solving problems more efficiently than classical computers. It is essentially a function which negates the phases of only those states which we desire to find through the algorithm. The complexity of a quantum algorithm can be measured by the number of uses/calls of the oracle function.

-----------------------------------------

## Quantum Gates

Quantum gates are mathematical operations that are performed on quantum bits (qubits) in a quantum computing system. These gates are the building blocks of quantum circuits, just like logic gates are the building blocks of classical digital circuits.

The Hadamard gate (H gate) is one of the most fundamental quantum gates. It is a single-qubit gate that acts on a qubit in superposition, transforming it into a new superposition state.
The H gate is represented by the following matrix:
H = 1/√2 * [1 1; 1 -1]

When applied to a qubit in the state |0⟩, the H gate transforms it into a superposition of |0⟩ and |1⟩:
H|0⟩ = 1/√2 * (|0⟩ + |1⟩)

Similarly, when applied to a qubit in the state |1⟩, the H gate transforms it into a superposition of |0⟩ and -|1⟩:
H|1⟩ = 1/√2 * (|0⟩ - |1⟩)


The X gate, also known as the NOT gate, is another fundamental single-qubit gate. It flips the state of a qubit from |0⟩ to |1⟩, or from |1⟩ to |0⟩.
The X gate is represented by the following matrix:
X = [0 1; 1 0]

When applied to a qubit in the state |0⟩, the X gate transforms it into the state |1⟩:
X|0⟩ = |1⟩

Similarly, when applied to a qubit in the state |1⟩, the X gate transforms it into the state |0⟩:
X|1⟩ = |0⟩
