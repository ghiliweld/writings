# ghili signatures

based on [KZG commitments](https://alinush.github.io/2020/05/06/kzg-polynomial-commitments.html) and the [polynomial remainder theorem](https://en.wikipedia.org/wiki/Polynomial_remainder_theorem).

# key gen
```
sk := (x, y) <--- x,y in Z_p sampled randomly
pk := (g^x, g^y) in a pairing group G
```

# signing
on a set of messages **m**, generate a random polynomial *p* such that `deg(p) > |m|` and `p(x) = y`. a signature `s` on any message `m_i` is `s = p(m_i)`.
a signature.
essentially a signature is a tuple of `(openings, proofs)`(possibly batched?) under a KZG commitment scheme.

# verification
verification works like how one would verify an opening in a KZG scheme, except we don't have to reveal the value that each `m_i` opens to when messages are batched.
