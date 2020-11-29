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

𝑋=(01 10)

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

𝐻=1/√2(11 1−1)

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
