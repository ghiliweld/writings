# ghili polynomial commitment scheme
a simple & elegant polynomial scheme with 
- O(1) commitment and proof sizes
- no trusted setups or groups of unknown order involved.
---
my beef with polynomial commitment schemes (trannsparent and trusted) was that commitments aren't made to polynomials, bur rather hidden evaluations of a polynomial.
this is the reason trusted setups exist; to protect that hidden evaluation. i'm not quite sure how this helps, i still have to learn more about it.

i'd rather a polynomials commitment scheme that makes commitments to the polynomial coefficients explicitly, and then allow you to prove you know the result of their blind evaluation.

---
## the scheme
the ghili polynomial commitment scheme, like most polynomial commitment schemes, is comprised of a triple of algorithms
- **commit**: takes a polynomial *p* (represented as a list of coefficients) and returns the commitment *c*
- **open**: takes a commitment *c*, an integer *x*, and an integer *y* and returns the proof *π*
- **open**: takes a commitment *c*, an integer *x*, an integer *y*, and a proof *π* and returns either 0 or 1

### commit
note: this isn't binding, i.e. multiple different polynomials could map to the same commitment. how do make it binding?

let g be a generator point in a group G

<img src="https://render.githubusercontent.com/render/math?math=%24c%20%3D%20%5Csum_%7Bi%3D0%7D%5E%7Bn%7D%20g%5E%7Ba_i%7D%24"> where a_i is the i-th coefficient in a polynomial *p*

### open


### verify

