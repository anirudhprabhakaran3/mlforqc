# Machine Learning for Quantum Circuit Mapping

Quantum Computing is a newer form of computing that has been getting much more prevalent nowadays. Every quantum circuit contains qubits that are used for various computations. These logical qubits have to be mapped to physical qubits on an actual quantum computer. However, there are some conditions for this – for any gate to be applicable, the qubits have to be physically adjacent to each other. 

This operation is done using SWAP gates, that are made of 3 CNOT gates. However, this is a time consuming and noisy operation. The task aims to find an ideal mapping between logical and physical qubits given a quantum circuit, using machine learning methods. 
