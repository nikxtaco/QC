# Grover's Algorithm

You may have heard that one of the advantages a quantum computer has over a classical computer is its superior speed searching databases.
Grover's algorithm demonstrates this capability. This algorithm can speed up an unstructured search problem quadratically (a classical computation requires on the order of 𝑁 steps to search 𝑁 entries problem, while a quantum computer requires just 𝑁⎯⎯⎯⎯√), but its uses extend beyond that; it can serve as a general trick or subroutine to obtain quadratic run time improvements for a variety of other algorithms. This is called the amplitude amplification trick.

## Unstructured search

Suppose you are given a large list of 𝑁 items. Among these items there is one item with a unique property that we wish to locate; we will call this one the winner, 𝑤. Think of each item in the list as a box of a particular item. Say all items in the list have grey colored items except the winning item.

To find the winning item or the marked item, using classical computation, one would have to check on average 𝑁/2 of these boxes, and in the worst case, all 𝑁 of them. On a quantum computer, however, we can find the marked item in roughly 𝑁⎯⎯⎯⎯√ steps with Grover's amplitude amplification trick. A quadratic speedup is indeed a substantial time-saver for finding marked items in long lists. Additionally, the algorithm does not use the list's internal structure, which makes it generic; this is why it immediately provides a quadratic quantum speed-up for many classical problems.

## Creating an oracle that marks the winning item

How will the list items be provided to the quantum computer? A common way to encode such a list is in terms of a function 𝑓 that returns 𝑓(𝑥)=0 for all unmarked items 𝑥, and 𝑓(𝑤)=1 for the winner. To use a quantum computer for this problem, we must provide the items in superposition to this function, so we encode the function into a unitary matrix called an oracle. First, we choose a binary encoding of the items 𝑥,𝑤∈{0,1}𝑛 so that 𝑁=2𝑛. This way, we can represent it in terms of qubits on a quantum computer. We then define the oracle matrix 𝑈𝑤 to act on any of the simple, standard basis states |𝑥⟩ by 𝑈𝑤|𝑥⟩=(−1)𝑓(𝑥)|𝑥⟩
We see that if 𝑥 is an unmarked item, the oracle does nothing to the state. However, when we apply the oracle to the basis state |𝑤⟩, it maps 𝑈𝑤|𝑤⟩=−|𝑤⟩. Geometrically, this unitary matrix corresponds to a reflection about the origin for the marked item in an 𝑁=2𝑛-dimensional vector space.

## Amplitude amplification

So how does the algorithm work? Before looking at the list of items, we have no idea where the marked item is. Therefore, any guess of its location is as good as any other. You may have heard the term superposition. This can be expressed in terms of a uniform superposition:

If at this point we were to measure in the standard basis |𝑥⟩, this superposition would collapse to any one of the basis states with the same probability. Hence, on average we would need to try about 𝑁=2^𝑛 times to guess the correct item.

Now, let's enter the procedure called amplitude amplification, which is how a quantum computer significantly enhances the probability of finding the correct item. This procedure stretches out (amplifies) the amplitude of the marked item, which shrinks the other items' amplitudes, so that measuring the final state will return the right item with near-certainty.

This algorithm has a nice geometrical interpretation in terms of two reflections, which generate a rotation in a two-dimensional plane. The only two special states we need to consider are the winner |𝑤⟩ and the uniform superposition |𝑠⟩ . These two vectors span a two-dimensional plane in the vector space ℂ𝑁 . They are not quite perpendicular because |𝑤⟩ occurs in the superposition with amplitude 𝑁−1/2 as well.

We can, however, introduce an additional state |𝑠′⟩ that is in the span of these two vectors, is perpendicular to |𝑤⟩, and is obtained from |𝑠⟩ by removing |𝑤⟩ and rescaling.

Step 0 : The amplitude amplification procedure starts out in the uniform superposition |𝑠⟩ . The uniform superposition is easily constructed from |𝑠⟩=𝐻⊗𝑛|0⟩𝑛. At 𝑡=0, the initial state is |𝜓0⟩=|𝑠⟩.



```
from qiskit import QuantumRegister, ClassicalRegister, QuantumCircuit
from qiskit import IBMQ, Aer, execute
from qiskit.visualization import plot_bloch_multivector
