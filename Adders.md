# Adders in Quantum Circuits

## Initializations

```
from qiskit import QuantumRegister, ClassicalRegister, QuantumCircuit
from qiskit import IBMQ, Aer, execute
from qiskit.visualization import plot_bloch_multivector

# If you run this code outside IBM Quantum Experience,
# run the following commands to store your API token locally.
# Please refer https://qiskit.org/documentation/install.html#access-ibm-quantum-systems
# IBMQ.save_account('MY_API_TOKEN')

# Loading your IBM Q account(s)
IBMQ.load_account()
```

Quantum circuits are models for quantum computation in which a computation is a sequence of quantum gates. 

## Quantum Gates

### X Gate

An X gate equates to a rotation around the X-axis of the Bloch sphere by 𝜋 radians. It maps |0⟩ to |1⟩ and |1⟩ to |0⟩. It is the quantum equivalent of the NOT gate for classical computers and is sometimes called a bit-flip.

𝑋=[01 10]

```
# Let's do an X-gate on a |0> qubit
q = QuantumRegister(1)
qc = QuantumCircuit(q)
qc.x(q[0])
qc.draw(output='mpl')

# Let's see the result
backend = Aer.get_backend('statevector_simulator')
result = execute(qc, backend).result().get_statevector(qc, decimals=3)
plot_bloch_multivector(result)
```

### H Gate

A Hadamard gate represents a rotation of 𝜋 about the axis that is in the middle of the 𝑋-axis and 𝑍-axis. It maps the basis state |0⟩ to |0⟩+|1⟩2√, which means that a measurement will have equal probabilities of being 1 or 0, creating a 'superposition' of states. This state is also written as |+⟩.

𝐻=1/√2[11 1−1]

```
# Let's do an H-gate on a |0> qubit
q = QuantumRegister(1)
qc = QuantumCircuit(q)
qc.h(q[0])
qc.draw(output='mpl')

# Let's see the result
backend = Aer.get_backend('statevector_simulator')
result = execute(qc, backend).result().get_statevector(qc, decimals=3)
plot_bloch_multivector(result)
```

### Z Gate

The Z gate represents a rotation around the Z-axis of the Bloch sphere by 𝜋 radians. It is sometimes called a 'phase shift gate'.

𝑍=[10 0−1]

```
# Let's do an Z-gate on |+>
q = QuantumRegister(1)
qc = QuantumCircuit(q)
qc.h(q[0])
qc.z(q[0])
qc.draw(output='mpl')

# Let's see the result
backend = Aer.get_backend('statevector_simulator')
result = execute(qc, backend).result().get_statevector(qc, decimals=3)
plot_bloch_multivector(result)
```

## CX Gate (CNOT Gate)

The controlled NOT (or CNOT or CX) gate acts on two qubits. It performs the NOT operation (equivalent to applying an X gate) on the second qubit only when the first qubit is |1⟩ and otherwise leaves it unchanged. Note: Qiskit numbers the bits in a string from right to left.

```
# Let's do an CX-gate on |00>
q = QuantumRegister(2)
qc = QuantumCircuit(q)
qc.cx(q[0],q[1])
qc.draw(output='mpl')
```

### CZ Gate

The CZ gate acts on two qubits, called a 'control bit' and a 'target bit'. It flips the sign (equivalent to applying the phase shift Z gate) of the target qubit if and only if the control qubit is |1⟩.

```
# Let's do an CZ-gate on |00>
q = QuantumRegister(2)
qc = QuantumCircuit(q)
qc.cz(q[0],q[1])
qc.draw(output='mpl')
```

Note: A CZ gate can also be constructed from a CX gate and an H gate.

```
# Let's make CZ-gate with CX-gate and H-gate
q = QuantumRegister(2)
qc = QuantumCircuit(q)
qc.h(q[1])
qc.cx(q[0],q[1])
qc.h(q[1])
qc.draw(output='mpl')
```

### CCX Gate

The CCX gate is also called a Toffoli gate. The CCX gate is a three-bit gate, with two controls and one target as their input and output. If the first two bits are in the state |1⟩, it applies a Pauli-X (or NOT) on the third bit. Otherwise, it does nothing. Note: Qiskit numbers the bits in a string from right to left.

```
# Let's do an CCX-gate on |00>
q = QuantumRegister(3)
qc = QuantumCircuit(q)
qc.ccx(q[0],q[1],q[2])
qc.draw(output='mpl')
```

## Creating logical gates with quantum gates

Now let's start creating a classic logic gate using quantum gates. Each gate and their truth tables will be shown. Here we denote quantum registers as 'q' and classical registers as 'c' where we encode the output of the measurement.
