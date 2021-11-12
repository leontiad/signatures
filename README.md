## Signatures
Provide authenticity and non repudiation assuming a secure public key infrastructure:
Party A signs a message  m with a signature Sig using its secret key $\mathsf{sk_A}$. Party B verifies correctness of the signature using public key of party A $\mathsf{pk_A}$ and gets assurances that the message trully  came from party A, nobody tamper it and party A cannot deny that it signed message m using its secret key $\mathsf{sk_A}$ 
[Toc]

### Custodial signatures
Signatures are used in our everyday digital life to sign transactions, payments, contracts, documents, reports, etc. With the wide adoption of cryptocurrencies, the validity of a transactions lies on the signature of the issuer who holds its secret key to a wallet. Bitcoin,Ethereum use ECDSA digital signature algorithm.

## Crypto machinery

### Modular Arithmetics

### Shamir secret sharing

A dealer wants to secret share a secret $s \in \mathbb{Z}_q$ such that $t$ shares $\{y_i\}_{i=0}^{t-1}$  out of total $n$ suffice to reconstruct the secret $s$.  for a prime $q$. It chooses a polynomial $a=a_0+a_1x+a_2x^2+a_{t-1}x^{t-1}$ of degree $t-1$, sets $a_0=s$ and gives to each party $P_i$ the share $a(i) = y_i$, $i\in 1...n$. 

To reconstruct Lagrange interpolation is used. We are looking for ways to identify a unique polynomial $p_{t-1}(x)$, which satisfies $p(i)=y_i$ of degree $t-1$, from $t$ pairs $(x_i,y_i),  i\in 1...t$. That can be seen as solving the equation:
$$\mathbf{A}x=b$$, for a matrix $\mathbf{A}$ and vectors $x,b$ where $b_i=y_i,  i\in 1...t$ and $A_{i,j} = p_j(x_i)$, where $p_j(x) = x^j$.

Define the interpolation polynomial:
$$p_{t-1}(x) = \sum_{j=0}^{t}y_j\mathcal{L_{t-1,j}(x)}$$

where the Lagrange polynomials $\mathcal{L_{t-1,j}}(x)$ are defined as follows:

$$\mathcal{L_{t-1,j}}(x) = \prod_{k=0,k\ne j}^{t}\frac{x-x_k}{x_j-x_k}$$

Finally the polynomial $f(x)$ we are looking: $f(x)= p_{t-1}(x)$

#### Example
Find the 2 degree polynomial from the following pairs


| i | x | y |
| -------- | -------- | -------- |
| 0     | 2     | 3 |    
| 1     | 1    | 4 |     
| 2     | -5    | 8 |    

First construct the Lagrange polynomials

$\mathcal{L_{2,0}}(2)=\frac{(x-x_1)(x-x_2)}{(x_0-x_1)(x_0-x_2)}=\frac{(x-1)(x+5)}{(2-1)(2+5)}=\frac{x^2+5x-x-5}{7} =\frac{1}{7}(x^2+4x-5)$


$\mathcal{L_{2,1}}(1)= \frac{(x-x_0)(x-x_2)}{(x_1-x_0)(x_1-x_2)}=\frac{(x-2)(x+5)}{(1-2)(1+5)}=\frac{x^2+5x-2x-10}{-6}=\frac{-1}{6}(x^2+3x-10)$


$\mathcal{L_{2,2}}(-5)  = \frac{(x-x_0)(x-x_1)}{(x_2-x_0)(x_2-x_1)}=\frac{(x-2)(x-1)}{(-5-2)(-5-1)}=\frac{x^2-x-2x+2}{42}=\frac{1}{42}(x^2-3x+2)$

Compute the interpolating polynomial:

$p_{2}(x) = \sum_{j=0}^{2}y_j\mathcal{L}_{2,j}=y_0\mathcal{L}_{2,0}+y_1\mathcal{L}_{2,1}+y_2\mathcal{L}_{2,2}\\=3\frac{1}{7}(x^2+4x-5)+4\frac{-1}{6}(x^2+3x-10)+8\frac{1}{42}(x^2-3x+2)\\=\frac{3}{7}(x^2+4x-5)-\frac{2}{3}(x^2+3x-10)+\frac{4}{21}(x^2-3x+2)\\=\frac{1}{3}x^2-\frac{18}{21}x+\frac{103}{21} = f(x)$

That does not seem correct :) I am missing sth.




###  Feldman’s VSS

Verifiable secret sharing allows the receivers of the shares to verify whether the dealer was giving consistent shares to the participants.

Dealer gives to every party $P_i$ along with $a(i)$ commitments of the form $c_i=g^a_i$ for every coefficient of the polynomial $a$. Each party now can verify the correctness of its share $y_i=a(i)$, evaluating the polynomial in the exponent:
$$g^{y_i}=\prod{c_i}=g^{\sum{a_ix^j}}=g^{a(i)}$$

###  Paillier

###  Elgamal


## ECDSA 
* Setup(): Output an elliptic curv group $\mathbb{G}$ of order $q$ and an element G which generates  $\mathbb{G}$.
* KeyGen($1^{\lambda}$): Choose uniformly at random $x\gets \mathbb{Z}_q$. Set $\mathsf{sk} = x$ and publish the public key $\mathsf{pk} = G \cdot x$
* Sign(m,$\mathsf{sk} = x$)-> $\sigma$: 
    * Pick uniform at random $k\gets \mathbb{Z}_q^*$
    *  Compute $R = k \cdot G$
    *  Let $R=(r_x,r_y)$
    *   Set $r = r_x \mod q$
    *   s = $k^{-1}(H(m) + x\cdot r)\mod q$
    *    Output $\sigma = (r,s)$, if  $r=0$ or $s=0$ repeat the above steps.
* Verify($\sigma,\mathsf{pk}$):
    * Compute $a=H(m)/s \mod \mathbb{Z}_q$ and $b=r/s \mod \mathbb{Z}_q$
    * $u = G \cdot a + G \cdot b \in \mathbb{G}$
    * Let $u=(u_x,u_y)$
    * $r' = u_x\mod \mathbb{Z}_q$
    * if $r==r'$ accept, otherwise reject


### Threshold ECDSA
To avoid single point of failure: stealing/loosing the secret signing key which authorizes transfers of assets - threshold signatures are adopted by FinTech industry to distribute trust among multiple signers holding a share of the secret key but never the secret key at each entire form.
Threshold ECDSA aglorithms in general are executed in two phases between $n$ parties:
1. ThresholdKeyGen: Each party $P_j$ computes its  share of random secret share $x_i^j$
2. ThresholdSign: Each party computes its share of the signature $\sigma_i^j$

[TODO]:Add appropriate security definitions. Basically unforgeability: malicious parties cannot produce signatures for messages they haven't seen their signatures. Depends on number of corrupted parties: honest/dishonest majority and malicious/passive adversaries

### Multiplicative shares + Paillier
[MR04,GGN16,L17]: 2 parties setting

**ThresholdKeyGen**: 
- $P_1$ and $P_2$  hold $x_1$ and $x_2$ such that $x=x_1\cdot x_2$
- $P_1,P_2$ compute $a=x_1\cdot G,b=x_2\cdot G$, respectively  and send each other $a,b$
- $P_1,P_2$ compute $\mathsf{pk}=x_1 \cdot x_2\cdot G$

**ThresholdSign:**
- $P_1$ chooses $k_1$ uniformly at random and $P_2$ choses $k_2$ uniformly at random
- They both compute $R = k_1\cdot k_2 \cdot G$ (DH)
- $P_1$ computes $k_1^{-1}$ and $P_2$ computes $k_2^{-1}$
- Each compute $r_x = R \mod q$
- P1 computes Pailier public key $pk1$ and secret key $sk1$
- $P_1$ sends to $P_2$: $c_1=E_{pk1}(k_1^{-1}\cdot H(m))$ and $c_2=E_{pk1}(k_1^{-1}\cdot x_1\cdot r)$
- $P_2$ computes $\sigma_1 = c_2^{k_2^{-1}} = E_{pk1}(k_1^{-1}\cdot k_2^{-1}\cdot H(m))$ and $\sigma_2 = c_1^{k_2^{-1}\cdot x_2} = E_{pk1}(k_1^{-1}\cdot k_2^{-1}x_1\cdot x_2 \cdot r)$
- Finally $\sigma(m) = Dec_{sk1}(\sigma_1\cdot \sigma_2)\\ = Dec_{sk1}((E_{pk1}(k_1^{-1}\cdot k_2^{-1}\cdot H(m))\cdot E_{pk1}(k_1^{-1}\cdot k_2^{-1}x_1\cdot x_2 \cdot r))\\=Dec_{sk1}(E_{pk1}(k^{-1}(H(m) + x\cdot r)\mod q))= k^{-1}(H(m) + x\cdot r)\mod q$

[MR04]: That works well for honest but curious adversaries. For malicious adversaries expensive ZKP are needed.

[L17] approach removes ZKP with some more preparation:
- ThresholdKeyGen is as above but $P_1$ also sends to $P_2$,  $x_1$ encrypted which $P_1$ pailllier public key $pk1: E_{pk1}(x_1)$
- The first $4$ steps from ThresholdSign are executed as above, coupled with ZKP for DH exchanges: sender proves that it knows the secret exponent (fast and simple).
- Interestengly $P_2$ has almost an encrypted signature and it computes:
$\sigma'=E_{pk1}(k_2^{-1}\cdot H(m))\cdot E_{pk1}(k_2^{-1}x_1\cdot x_2 \cdot r)$. and sends $\sigma'$ to $P_1$.
- $P_1$ decrypts $\sigma'$ and computes $\sigma=\sigma'\cdot k_1^{-1}$
- $P_1$ verifies whether r,s is a valid ECDSA signature

Notice that if $P_1$ is malicious then its only input to the protocol is $k_1,x_1$ for the DH exchange and can be proven secure with ZKP. Another proof is needed to guarantee $P_1$ actually faithfully encrypted $E_{pk1}(x_1)$: Given $X_1=x_1\cdot G$ and $E_{pk1}(x_1)$ prove that there exists such $x_1$.

If $P_2$ wants to cheat by sending an invalid message $\sigma'$ then $P_1$ can recompute the signature  and check whether $P_2$ is cheating because $\sigma'$ is almost one step before being a valid signature: $\sigma'=\sigma\cdot k_1^{-1}$. As such, there is no need for expensive ZKP.

### Fully Threshold

- [GGN16]: Expensive key generation phase: distributed Paillier key generation. 
- [GG18]  Paillier
- [LNR18] Elgamal
- [DKLS19] OT based for multiplicative shareS: light computation, heavy bandwidth



## EdDSA
### Threshold EdDSA
## BLS
### Threshold BLS

## References
- [FROST] Chelsea Komlo, Ian Goldberg: FROST: Flexible Round-Optimized Schnorr Threshold Signatures. SAC 20
- [MR04] Philip D. MacKenzie, Michael K. Reiter: Two-party generation of DSA signatures. Int. J. Inf. Sec
- [GGN16]	Rosario Gennaro, Steven Goldfeder, Arvind Narayanan:Threshold-Optimal DSA/ECDSA Signatures and an Application to Bitcoin Wallet Security. ACNS 2016: 156-174
- [L17] Y Lindell:Fast secure two-party ECDSA signing Annual International Cryptology Conference, 2017
- [LNR18] Yehuda Lindell Ariel Nof Samuel Ranellucci: Fast Secure Multiparty ECDSA with Practical Distributed Key Generation and Applications to Cryptocurrency Custody
- [GG18] Rosario Gennaro and Steven Goldfeder: Fast Multiparty Threshold ECDSA with Fast Trustless Setup
- [Sepior] Ivan Damgard, Thomas Pelle Jakobsen,, Jesper Buus Nielsen, Jakob Illeborg, Pagter, and Michael Bæksvang Østergaard: Fast Threshold ECDSA with Honest Majority
- [GG20] Rosario Gennaro, Steven Goldfeder: One Round Threshold ECDSA with Identifiable Abort
- [Fireblocks] Ran Canetti, Rosario Gennaro, Steven Goldfeder, Nikolaos Makriyannis, Udi Peled: UC Non-Interactive, Proactive, Threshold ECDSA with Identifiable Aborts
- [DKLS18] Jack Doerner, Yashvanth Kondi, Eysa Lee, Abhi Shelat: Secure two-party threshold ECDSA from ECDSA assumptions
- [DKLS19] Jack Doerner, Yashvanth Kondi, Eysa Lee, Abhi Shelat: Threshold ECDSA from ECDSA Assumptions: The Multiparty 
