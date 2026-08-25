# Basic Quantum Circuit Simulator

This project models the time evolution of multi-qubit systems through pure linear algebra, bypassing black-box quantum frameworks to explicitly demonstrate the underlying computational physics.

## Core Quantum Concepts

* **Qubit & Superposition:** The fundamental unit of quantum information. Unlike classical bits, a qubit exists in a two-dimensional complex Hilbert space ($\mathbb{C}^2$) as a linear combination of basis states: $\vert{}\psi\rangle = \alpha\vert{}0\rangle + \beta\vert{}1\rangle$.
* **Unitary Evolution (Quantum Gates):** Quantum operations are represented by unitary matrices ($U^\dagger U = I$). These preserve the state vector's normalization (ensuring total probability remains 1) and guarantee that quantum operations are reversible.
* **Entanglement:** A physical phenomenon where multiple qubits become perfectly correlated. The state of the entire system cannot be factored into the individual states of the constituent qubits (e.g., measuring one qubit of a Bell state instantly determines the other).
* **Measurement & Collapse:** Observing a quantum state is a non-unitary process. It forces the superposition to irreversibly collapse into a classical computational basis state. The probability of measuring state $\vert{}i\rangle$ follows the Born rule: $P(i) = \vert{}\alpha_i\vert{}^2$.

## Mathematical Theory Behind the Code

This simulator explicitly translates quantum mechanics postulates into NumPy array operations:

* **State Vector Representation:** An $n$-qubit system is modeled as a $2^n$-dimensional complex vector. The `QuantumSimulator` initializes this as a 1D NumPy array representing the computational ground state $\vert{}00\dots0\rangle$.
* **Global Operators via Tensor Products:** Single-qubit gates (like $H$, $X$, $Y$, $Z$) are $2 \times 2$ matrices. To apply them to a target qubit in an $n$-qubit register, the simulator constructs a full $2^n \times 2^n$ global unitary matrix using the Kronecker product (`np.kron`). It applies the specific gate matrix to the target index and the $2 \times 2$ Identity matrix ($I$) to all other indices.
* **Controlled-NOT (CNOT) Logic:** The entangling CNOT gate is algorithmically constructed using projection operators. The global matrix is formed by summing two conditions: $P_0 \otimes I$ (if the control is $\vert{}0\rangle$, apply Identity to the target) and $P_1 \otimes X$ (if the control is $\vert{}1\rangle$, apply Pauli-X to the target).
* **Time Evolution:** The physical evolution of the system is executed via standard matrix-vector multiplication: $\vert{}\psi_{new}\rangle = U_{global} \vert{}\psi_{old}\rangle$.
* **Stochastic Measurement Engine:** The `measure()` method extracts the real probabilities from the complex vector ($\vert{}\psi\vert{}^2$), ensures numerical normalization, and uses `np.random.choice` to sample an outcome. Upon sampling, it physically collapses the simulated wave function by zeroing out the array and placing a `1.0` at the observed index.
