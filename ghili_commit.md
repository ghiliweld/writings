# ghili polynomial commitment scheme
a simple & elegant polynomial scheme with 
- O(1) commitment and proof sizes
- no trusted setups or groups of unknown order involved.

my beef with polynomial commitment schemes (transparent and trusted) was that commitments aren't made to polynomials, bur rather hidden evaluations of a polynomial.
this is the reason trusted setups exist; to protect that hidden evaluation.
given two polynomials, *p* and *p'* (both of degree d), *p* and *p'* will intersect at at most *d* points. this makes it unlikely that you can cheat your way to a valid evaluation by using a different polynomial than what you commit to. an evaluation at a hidden point *𝜏* will be almost impossible to cheat, which is why keeping our trusted setup is important. if our point *𝜏* gets exposed, it could undermine the security of the commitment scheme.

> we can conclude that evaluation6 of any polynomial at an arbitrary point is akin to the
representation of its unique identity - from [this paper](https://arxiv.org/pdf/1906.07221.pdf)

i'd rather a polynomials commitment scheme that makes commitments to the polynomial coefficients explicitly, and then allow you to prove you know the result of their blind evaluation. the issue with commiting to coefficients is that their orderinng must be made explicit in the commitment. how to do this while maintaining a constant size commitment is tricky.

## the naive approach
<img src="https://render.githubusercontent.com/render/math?math=c = (g^{a_0}, ..., g^{a_n})"> with a blind evaluation to compute  <img src="https://render.githubusercontent.com/render/math?math=g^{p(x)}">

this works fine, except the commitment size and computation time is now O(n), where n = deg(p)

we can do betteer

## close but no dice :/
the ghili polynomial commitment scheme, like most polynomial commitment schemes, is comprised of a triple of algorithms
- **commit**: takes a polynomial *p* (represented as a list of coefficients) and returns the commitment *c*
- **open**: takes a commitment *c*, an integer *x*, and an integer *y* and returns the proof *π*
- **open**: takes a commitment *c*, an integer *x*, an integer *y*, and a proof *π* and returns either 0 or 1

it must be 
- **binding**: two polynomials *p* and *p'* cannot map to the same commitment *c*
- **hiding**: a polynomial *p* cannot be uncovered from a commitment *c*
- **efficient**: verifying an evaluation to a commmitment must be more efficient than evaluating the polynomial itself, so should be done in sublinear time

### commit
> note: this isn't binding, i.e. multiple different polynomials could map to the same commitment. if we want to commit to the coefficients we need a way to asign an order to em as well. how can we make it binding?

let g be a generator point in a group G

<img src="https://render.githubusercontent.com/render/math?math=%24c%20%3D%20%5Csum_%7Bi%3D0%7D%5E%7Bn%7D%20g%5E%7Ba_i%7D%24"> where a_i is the i-th coefficient in a polynomial *p*

## progress?
this is the situation so far:
- commiting to a coefficient a_i is easy, it's just an integer so raise g to the a_i
- commiting to an ordered sequence (coefficient list of a polynomial) isn't feasible cause of how elliptic curve groups are commutative
  i.e. we could commit to *p* and *p'* with the same commitment *c*
- so commiting to a polynomial by explicitly commiting to it's coefficients won't be straightforward

point #1 provides a hint as how to go about this. we can commit to the coefficients individually, but making those the commitment gives us size O(n).
is there way to aggregate these coeffs injectively to get size O(1)?

i think the [PointProofs](https://eprint.iacr.org/2020/419.pdf) offers some guidance here. the paper introduces cross-commitment aggregation. commitments use a trusted setup with secret trapdoor *𝜏* **but** that trapdoor is not used for the a_0 coeff since it it's multiplied by x^0 which is 1. therefore, i'm hoping we can commit to polynomials p_i = a_i the same way we would commit to anythinng using PointProofs then we aggregate them.

tl;dr onn the plan:
- commit to coefficients
- aggregate them somehow with PointProof techniques

## is this even possible?
let e be a pairing bilinear map -- e: G x G -> H

let e' be a self-bilinear map -- G x G -> G

self-bilinear are proven to not be secure, but if were to combine self-bilinear and regular bilinear maps to create a secure multilinear map it could be a way of commiting to coeffs a_0 through a_n. so self-bilinear map through n-1 operations and then a regular bilinear map at the last step to ensure security.

ex: e'(g^a_0, g^a_1) ->, g^a_2) -> ... e(->, g^a_n) = h^p
