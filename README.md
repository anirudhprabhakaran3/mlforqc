# Quantum Circuit Mapping

The quantum circuit mapping problem is a key challenge in quantum computing, particularly in the context of quantum compilation. When designing a quantum algorithm or circuit, it's often necessary to map it onto a physical quantum device, which can involve some significant challenges due to the physical constraints of the hardware. Quantum devices typically have a limited number of qubits, which can be arranged in a variety of different topologies (such as a linear array, a 2D grid, or a tree structure). Additionally, the qubits may be subject to different types of errors, such as decoherence, crosstalk, and other forms of noise.

The goal of quantum circuit mapping is to find a way to implement a given quantum circuit on a specific quantum device, subject to these constraints. Every quantum circuit contains qubits that are used for various computations. These logical qubits have to be mapped to physical qubits on an actual quantum computer. However, there are some conditions for this – for any gate to be applicable, the qubits have to be physically adjacent to each other. This operation is done using SWAP gates, that are made of 3 CNOT gates. However, this is a time consuming and noisy operation.

The circuit mapping problem has been proved to be NP complete, and as a consequence, the computation of its exact solution could be not suitable to deal with for future generation of quantum processors characterized by thousands or millions of qubits. Thus, there is a strong need of approximate algorithms capable of efficiently computing sub-optimal solutions for this problem and pave the way towards the next generation of compilers for quantum computers. In our project, we implement the Neural Layout model, a neural network based machine learning approach to efficiently address the circuit mapping problem based on data from real IBM Q processors.

-----------------------------------------

## Nerual Layout approach

We approach the circuit mapping problem as a classification task, which is implemented by a function Φ that uses an input vector x, containing a set of features characterizing both the circuit c and the processor P, which estimates an array y composed of n elements corresponding to the n circuit qubit mappings, where each element belongs to the mapping label set Y. The aim is to be able to provide mapping capabilities similar to the best algorithms currently used in quantum circuit mapping, but with a lower computational complexity.


The aforementioned input vector x is composed of a set of features characterizing both the circuit c and the processor P.

In particular, with respect to the circuit c, the following information has been considered:
* an integer value representing the number of qubits composing the circuit.
* an integer value representing the total number of CNOT gates in the circuit c.
* a matrix of integer values where the item [i, j] contains the number of CNOT gates between the control qubits qi and the target qubit qj of the circuit c. To make the network work with any circuit width less than or equal to the number of processor qubits, this matrix of integers is set of size n X n.

At the same time, with respect to the processor P, the following information has been taken into account:
* an array of real values where each value represents the error rate of a CNOT using qi as control qubit and qj as target qubit for each q1, qj ∈ processor qubits.
* an array of real values where each value represents the execution time of a CNOT using qi as control qubit and qj as target qubit for each q1, qj ∈ processor qubits.
* an array of real values where each value represents the transverse relaxation time (T2) characterizing a qubit qi of the processor P.
* an array of real values where each value represents the longitudinal relaxation time (T1) characterizing a qubit qi of the processor P.
* an array of real values where each value represents the readout error characterizing a qubit qi of the processor P.

We will be using a dataset corresponding to the IBM Q Burlington processor, where n=5. This gives us 78 input features.

Nerual Layout has the following architecture:
1. An input layer with 78 neurons.
2. A dense layer with 256 neurons.
3. A dense layer with 1024 neurons.
4. A dropout layer with 1024 neurons.
5. Five slots numbered 0 to 4, giving 25 outputs.
6. A repair operator giving 5 outputs as the output vector y.

The five slots have 3 layers each:
1. A dense layer with 256 neurons.
2. A dense layer with 128 neurons.
3. A dense layer with 5 neurons.

The 5 neurons corresponding to each slot represents a probability array (activation func: softmax), where the ith element signifies the probability that the given circuit qubit is mapped to the ith processor qubit.
	The slots are an essential part since without it, the 5 outputs of the neural network could map 2 or more circuit qubits to the same processor qubit, which is not a possible solution. Hence we introduce slots and probability arrays, and avoid such solutions using a repair operator.

The repair operator takes in 25 inputs corresponding to 5 probability arrays, and outputs five integers corresponding to the mapped processor qubit that forms a feasible solution. It works by applying the following algorithm 5 times for 5 different outputs.

Repeat for 5 iterations:
** Find the maximum probability among the arrays of the remaining circuit qubits corresponding to the processor qubits that havn't yet been mapped.
** Assign that processor qubit to the corresponding circuit qubit.

The 5 output qubits correspond to the processor qubits that have been mapped to the ith circuit qubit.
