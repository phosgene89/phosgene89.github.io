
# <center>Notes on An Introduction to Quantum Computing</center>

## Chapter 2: Linear Algebra and the Dirac Notation

 * A state of $n$ qubits can be described with a vector of length $2^{n}$.

 * A bra, 
 $\langle\textbf{v}|$
 , is the complex conjugate of 
 $|\textbf{v}\rangle$, $\langle\textbf{v}| = |\textbf{v}\rangle^{*}$.

 * Therefore a bra is a row vector and a ket is a column vector.

 * Inner products, $\langle\textbf{v},\textbf{w}\rangle$, satisfy the following properties:

    1. $\langle\textbf{v},\sum_{i}\lambda_{i}\textbf{w}_{i}\rangle=\sum_{i}\lambda_{i}\langle\textbf{v},\textbf{w}_{i}\rangle$

    2. $\langle\textbf{v},\textbf{w}\rangle=\langle\textbf{w},\textbf{v}\rangle^{*}$

    3. $\langle\textbf{v},\textbf{v}\rangle\geq0$ and $\langle\textbf{v},\textbf{v}\rangle = 0 \Longleftrightarrow \textbf{v}=0$

 * $\parallel|\Psi\rangle\parallel=\sqrt{\langle\Psi|\Psi\rangle}$ is called the Euclidean norm.

 **Definition:** Consider a Hilbert space $\mathcal{H}$ of dimension $2^{n}$. A set of $2^{n}$ vectors $B=\{ |b_{m}\rangle \}\subseteq \mathcal{H}$ is called an orthonormal basis for $\mathcal{H}$ if

 $$\langle b_{n}|b_{m}\rangle = \delta_{n,m}, \:\:\:\:\:\:\:\forall b_{m}, b_{n} \in B$$

 and every $|\Psi\rangle \in \mathcal{H}$ can be written as

 $$|\Psi\rangle =\sum_{b\in B}\Psi_{n}|b_{n}\rangle,\:\:\:\:\:\:\:\Psi_{n}\in\mathbb{C}$$

 The values of $\Psi_{n}$ satisfy $\Psi_{n}=\langle b_{n}|\Psi \rangle$, and are called the coefficients of $| \Psi \rangle$ with respect to basis $\{ | b_{n} \rangle \}$.

### <a href="https://phosgene89.github.io/quantum_computing/notes_kaye_etal">Return to Chapter List</a>

## Further Reading