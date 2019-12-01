
# <center>Notes on An Introduction to Quantum Computing</center>

## Chapter 1: Introduction and Background

 * Classical computers cannot efficiently simulate quantum processes. 

 * We can represent a system with 2 possible states as a 2-dimensional column vector. The elements of this vector are the probabilities of being in the 2nd state. We use this to represent bits.

 * The NOT operator is represented by the matrix

$$NOT \equiv \begin{pmatrix}
0&1\\
1&0\\
\end{pmatrix}$$

 * The tensor product of two vectors $A$ and $B$, with dimensions $n$ and $m$ respectively, is defined as

$$A \otimes B = \begin{pmatrix} A_{1}B_{1}\\ \vdots\\A_{n}B_{1}\\A_{1}B_{2}\\ \vdots\\A_{n}B_{2} \\\vdots \\A_{1}B_{m} \\\vdots \\A_{n}B_{m} \end{pmatrix}$$

 * The CNOT gate is defined as

$$CNOT \equiv \begin{pmatrix}
1&0&0&0\\
0&1&0&0\\
0&0&0&1\\
0&0&1&0
\end{pmatrix} $$

 * Quantum mechanics is based on unitary operators, which requires that operators are time-reversible. The AND operator is not reversible, as when it evaluates to false, we do not know whether one or both inputs were false. ie we must be able to tell exactly what the input was based on the output.

  * It is possible to replace all non-reversible operators with reversible ones. A trivial example is concatenating the input to the output.

  * Superposition and interference are important in differentiating quantum mechanics from classical mechanics. For more, see <a href="https://ocw.mit.edu/courses/physics/8-04-quantum-physics-i-spring-2016/lecture-notes/MIT8_04S16_LecNotes2.pdf">experiments with photons</a>.

### <a href="https://phosgene89.github.io/notes_kaye_etal">Return to Chapter List</a>

## Further Reading