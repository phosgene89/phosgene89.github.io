
# <center>Notes on An Introduction to Quantum Computing</center>

## Chapter 2: Linear Algebra and the Dirac Notation

 **Definition:** Consider a Hilbert space 
 $\mathcal{H}$
  of dimension 
$2^{n}$
. A set of 
$2^{n}$
 vectors 
$B=\{ |b_{m}\rangle \}\subseteq \mathcal{H}$
 is called an orthonormal basis for 
 $\mathcal{H}$
  if

 $$\langle b_{n}|b_{m}\rangle = \delta_{n,m}, \:\:\:\:\:\:\:\forall b_{m}, b_{n} \in B$$

 and every 
 $|\Psi\rangle \in \mathcal{H}$
  can be written as

 $$|\Psi\rangle =\sum_{b\in B}\Psi_{n}|b_{n}\rangle,\:\:\:\:\:\:\:\Psi_{n}\in\mathbb{C}$$

 The values of 
 $\Psi_{n}$
  satisfy 
$\Psi_{n}=\langle b_{n}|\Psi \rangle$
, and are called the coefficients of 
$| \Psi \rangle$
 with respect to basis 
 $\{ | b_{n} \rangle \}$
 .

 * The set 
 $\{ \langle b_{n}| \}$
  is an orthonormal basis for 
$\mathcal{H}^{*}$
 called the dual basis.

 **Definition:** A linear operator on a vector space $\mathcal{H}$ is a linear transformation $T:\mathcal{H}\to\mathcal{H}$ of the vector space to itself.

* The outer product, 
$|\Psi\rangle\langle\varphi|$
, has the following property

    $$(|\Psi\rangle\langle\varphi|)|\gamma\rangle=|\Psi\rangle(\langle\varphi|\gamma\rangle)$$

    $$|\Psi\rangle(\langle\varphi|\gamma\rangle)=(\langle\varphi|\gamma\rangle)|\Psi\rangle$$

 * Linear operators can be written in terms of linear combinations of outer products of orthonobal bases.

  * The identity operator can be written as 
$\textbf{1}=\sum_{b_{n}\in B} |b_{n} \rangle\langle b_{n}|$

 * For any linear operator $T$, 
 $T^{\dagger}=T^{*}$

  * $T$ is unitary if 
$T^{\dagger}=T^{-1}$

 * $T$ is Hermitian if 
 $T^{\dagger}=T$

 * The eigenvalues of Hermitian operators are real.

 * $Tr(A) = \sum_{b_{n}}\langle b_{n}|A|b_{n}\rangle$
 where 
 $\{ |b_{n}\rangle \}$ 
is any orthonogmal basis for 
$\mathcal{H}$
.

**Definition:** A normal operator $A$ is an operator that satisfies

$$AA^{\dagger}=A^{\dagger}A$$

**Spectral theorem:** For every normal operator $T$ acting on a finite-dimensional Hilbert space $\mathcal{H}$, there is an orthonormal basis of 
$\mathcal{H}$ 
consisting of eigenvectors 
$|T_{i}\rangle$
 of $T$.

**Alternate formulation of the Spectral theorem:** For every finite-dimensional normal matrix 
$T$
, there is a unitary matrix 
$P$ 
such that 
$T = P\Lambda P^{\dagger}$
, where 
$\Lambda$
 is a diagonal matrix containing the eigenvalues of 
 $T$
 . The columns of 
 $P$
  contain eigenvectors of 
$T$
.

$\mathcal{H}_{A}$ 

$\mathcal{H}_{B}$



**Schmidt decomposition theorem:** If 
$|\Psi\rangle$
 is a vector in a tensor product space 
$\mathcal{H}_{A}$ 
$\otimes$ 
$\mathcal{H}_{B}$
 , then there exists an orthonormal basis 
$\{ | \varphi^{B}_{i} \}$
 for 
$\mathcal{H}_{B}$
 , and non-negative real numbers
$\{ p_{i} \}$
  so that

$$|\Psi\rangle = \sum_{i}\sqrt{p_{i}}|\varphi^{A}_{i}\rangle |\varphi_{i}^{B} \rangle$$

### <a href="https://phosgene89.github.io/quantum_computing/notes_kaye_etal">Return to Chapter List</a>

## Further Reading