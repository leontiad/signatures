## Signatures
Provide authenticity and non repudiation assuming a secure public key infrastructure:
Party A signs a message  m with a signature Sig using its secret key $\mathsf{sk_A}$. Party B verifies correctness of the signature using public key of party A $\mathsf{pk_A}$ and gets assurances that the message trully  came from party A, nobody tamper it and party A cannot deny that it signed message m using its secret key $\mathsf{sk_A}$
[Toc]

### Custodial signatures
Signatures are used in our everyday digital life to sign transactions, payments, contracts, documents, reports, etc. With the wide adoption of cryptocurrencies, the validity of a transactions lies on the signature of the issuer who holds its secret key to a wallet. Bitcoin,Ethereum use ECDSA digital signature algorithm.

#### ECDSA 
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
#### EdDSA

