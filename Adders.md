# Adders in Quantum Circuits

Quantum circuits are models for quantum computation in which a computation is a sequence of quantum gates. 

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

### CX Gate (CNOT Gate)

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

### NOT Gate

As was mentioned before, an X gate can be considered a NOT gate.

```
# Create a Quantum Circuit with 1 quantum register and 1 classical register
q = QuantumRegister(1)
c = ClassicalRegister(1)
qc = QuantumCircuit(q,c)
qc.x(q[0])
qc.measure(q[0], c[0])    # Map the quantum measurement to the classical bits
qc.draw(output='mpl')
```

### AND Gate

With a CCX gate, the result of an AND gate for two control bits will be output to its target bit.

```
q = QuantumRegister(3)
c = ClassicalRegister(1)
qc = QuantumCircuit(q,c)
qc.ccx(q[0], q[1], q[2])
qc.measure(q[2], c[0])
qc.draw(output='mpl')
```

### NAND Gate

A NAND gate can be made by applying a NOT gate after applying an AND gate.

```
q = QuantumRegister(3)
c = ClassicalRegister(1)
qc = QuantumCircuit(q,c)
qc.ccx(q[0], q[1], q[2])
qc.x(q[2])
qc.measure(q[2], c[0])
qc.draw(output='mpl')
```

### OR Gate

```
q = QuantumRegister(3)
c = ClassicalRegister(1)
qc = QuantumCircuit(q,c)

qc.cx(q[1], q[2])
qc.cx(q[0], q[2])
qc.ccx(q[0], q[1], q[2])
qc.measure(q[2], c[0])
qc.draw(output='mpl')
```

### XOR Gate

```
q = QuantumRegister(3)
c = ClassicalRegister(1)
qc = QuantumCircuit(q,c)
qc.cx(q[1], q[2])
qc.cx(q[0], q[2])
qc.measure(q[2], c[0])
qc.draw(output='mpl')
```

### NOR Gate

```
q = QuantumRegister(3)
c = ClassicalRegister(1)
qc = QuantumCircuit(q,c)

qc.cx(q[1], q[2])
qc.cx(q[0], q[2])
qc.ccx(q[0], q[1], q[2])
qc.x(q[2])
qc.measure(q[2], c[0])
qc.draw(output='mpl')
```

## IMPORTANT: How to calculate Quantum Costs using an Unroller

There are several ways to evaluate an efficiency of a program (quantum circuit). Such as:

* Number of quantum bits
* Depth
* Program execution speed (Runtime)
* Number of instructions

These are all important factors that impact the results and throughput of quantum computation. In this particular challenge, we will use the number of instructions to evaluate the efficiency of our program. We will call the number of instructions "cost" throughout this challenge and will use the following formula to evaluate the cost of a circuit.

Cost = Single-qubit gates + CX gates×10

Any given quantum circuit can be decomposed into single-qubit gates (an instruction given to a single qubit) and two-qubit gates. With the current Noisy Intermediate-Scale Quantum (NISQ) devices, CX gate error rates are generally 10x higher than a single qubit gate. Therefore, we will weigh CX gates 10 times more than a single-qubit gate for cost evaluation.

You can evaluate gate costs by yourself by using a program called "Unroller". To elaborate on this, let's take a look at the example below.

### Example 1
```
import numpy as np
from qiskit import QuantumCircuit, ClassicalRegister, QuantumRegister
from qiskit import BasicAer, execute
from qiskit.quantum_info import Pauli, state_fidelity, process_fidelity

q = QuantumRegister(4, 'q0')
c = ClassicalRegister(1, 'c0')
qc = QuantumCircuit(q, c)
qc.ccx(q[0], q[1], q[2])
qc.cx(q[3], q[1])
qc.h(q[3])
qc.ccx(q[3], q[2], q[1])
qc.measure(q[3],c[0])
qc.draw(output='mpl')
```
Count the number of operations:
```
qc.count_ops()
```
OrderedDict([('ccx', 2), ('cx', 1), ('h', 1), ('measure', 1)])

As you can see, this quantum circuit contains a Hadamard gate, a CX gate and CCX gates. By using qiskit.transpiler and importing PassManager, we can decompose this circuit into gates specified by the Unroller as shown below. In this case, into U3 gates and CX gates.

```
from qiskit.transpiler import PassManager
from qiskit.transpiler.passes import Unroller
pass_ = Unroller(['u3', 'cx'])
pm = PassManager(pass_)
new_circuit = pm.run(qc) 
new_circuit.draw(output='mpl')
```
Count the number of operations:
```
new_circuit.count_ops()
```
OrderedDict([('u3', 19), ('cx', 13), ('measure', 1)])

Thus, the cost of this circuit is 19+13×10=149.

You can easily check how any arbitrary gate can be decomposed by using the Unroller.

### Example 2

In the example below, we used the Unroller to decompose a CCX gate into U3 gates and CX gates.
```
q = QuantumRegister(3, 'q0')
c = ClassicalRegister(1, 'c0')
qc = QuantumCircuit(q, c)
qc.ccx(q[0], q[1], q[2])
qc.draw(output='mpl')
```
Using the Unroller:
```
pass_ = Unroller(['u3', 'cx'])
pm = PassManager(pass_)
new_circuit = pm.run(qc) 
new_circuit.draw(output='mpl')
```
Count the number of operations:
```
new_circuit.count_ops()
```
OrderedDict([('u3', 9), ('cx', 6)])

So, the total cost of a CCX gate can be calculated as 9+6×10=69.

## Adders

An adder is a digital logic circuit that performs addition of numbers.
In this example, we are going to take a look at the simplest adders, namely half adder and full adder.

### Half Adder

The half adder is used to add together the two least significant digits in a binary sum. It has two single binary inputs, called A and B, and two outputs C (carry out) and S (sum). The output C will be used as an input to the Full Adder, which will be explained later, for obtaining the value in the higher digit.

From the truth table, you should notice that the carry output, C, is a result of operating an AND gate against A and B, where the output S is a result of operating an XOR against A and B. As we have already created the AND and XOR gates, we can combine these gates and create a half adder as follows.

We denote our quantum register as 'q', classical registers as 'c', assign inputs A and B to q[0] and q[1], the sum output S and carry output C to q[2] and q[3].

```
#Define registers and a quantum circuit
q = QuantumRegister(4)
c = ClassicalRegister(2)
qc = QuantumCircuit(q,c)

#XOR
qc.cx(q[1], q[2])
qc.cx(q[0], q[2])
qc.barrier()

#AND
qc.ccx(q[0], q[1], q[3])
qc.barrier()

#Sum
qc.measure(q[2], c[0])
#Carry out
qc.measure(q[3], c[1])

backend = Aer.get_backend('qasm_simulator')
job = execute(qc, backend, shots=1000)
result = job.result()
count =result.get_counts()
print(count)
qc.draw(output='mpl')
```

### Full Adder

The full adder takes two binary numbers plus an overflow bit, which we will call X, as its input. Create a full adder with input data:
𝐴=1, 𝐵=0, 𝑋=1.
```
from qiskit import QuantumRegister, ClassicalRegister, QuantumCircuit
from qiskit import IBMQ, Aer, execute

##### build your quantum circuit here
#Define registers and a quantum circuit
q = QuantumRegister(10)
c = ClassicalRegister(2)
qc = QuantumCircuit(q,c)

# Assigning Input Data
qc.x(q[0])
qc.x(q[2])

#XOR first two inputs in q[5]
qc.cx(q[0], q[5])
qc.cx(q[1], q[5])
qc.barrier()

#XOR result and third input in q[3]
qc.cx(q[2], q[3])
qc.cx(q[5], q[3])
qc.barrier()

#AND every two set of inputs in q[6], q[7] & q[8]
qc.ccx(q[0], q[1], q[6])
qc.ccx(q[1], q[2], q[7])
qc.ccx(q[0], q[2], q[8])
qc.barrier()

#OR the first two AND results in q[9]
qc.cx(q[6], q[9])
qc.cx(q[7], q[9])
qc.ccx(q[6], q[7], q[9])

#OR the previous result & the third result in q[4]
qc.cx(q[9], q[4])
qc.cx(q[8], q[4])
qc.ccx(q[9], q[8], q[4])
qc.barrier()

#Sum
qc.measure(q[3], c[0])
#Carry out
qc.measure(q[4], c[1])

# execute the circuit by qasm_simulator
backend = Aer.get_backend('qasm_simulator')
job = execute(qc, backend, shots=1000)
result = job.result()
count =result.get_counts()
print(count)
qc.draw(output='mpl')
```

```
```

```
```
