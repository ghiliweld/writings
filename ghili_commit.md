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

## the scheme
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

### open


### verify

