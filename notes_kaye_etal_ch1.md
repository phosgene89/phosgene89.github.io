
# <center>Notes on An Introduction to Quantum Computing</center>

## Chapter 1: Introduction and Background

Classical computers cannot efficiently simulate quantum processes. 

We can represent a system with 2 possible states as a 2-dimensional column vector. The elements of this vector are the probabilities of being in the 2nd state. We use this to represent bits.

The NOT operator is represented by the matrix

$$\begin{pmatrix}
0&1\\
1&0\\
\end{pmatrix}$$

The tensor product of two vectors $A$ and $B$, with dimensions $n$ and $m$ respectively, is defined as

$$A \otimes B = \begin{pmatrix} A_{1}B_{1}\\ \vdots\\A_{n}B_{1}\\A_{1}B_{2}\\ \vdots\\A_{n}B_{2} \\\vdots \\A_{1}B_{m} \\\vdots \\A_{n}B_{m} \end{pmatrix}$$

The CNOT gate is defined as

$$CNOT \equiv \begin{pmatrix}
1&0&0&0\\
0&1&0&1\\
0&0&0&1\\
0&0&1&0
\end{pmatrix} $$

### <a href="https://phosgene89.github.io/notes_kaye_etal">Return to Chapter List</a>

## Further Reading