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

An X gate equates to a rotation around the X-axis of the Bloch sphere by 𝜋 radians. It maps |0⟩ to |1⟩ and |1⟩ to |0⟩. It is the quantum equivalent of the NOT gate for classical computers and is sometimes called a bit-flip. If you are not familiar with linear algebra, you can learn it here: https://qiskit.org/textbook/ch-appendix/linear_algebra.html.

𝑋=(01 10)
