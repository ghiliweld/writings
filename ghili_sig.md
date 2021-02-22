# ghili signatures

based on [KZG commitments](https://alinush.github.io/2020/05/06/kzg-polynomial-commitments.html) and the [polynomial remainder theorem](https://en.wikipedia.org/wiki/Polynomial_remainder_theorem).

# key gen
```
sk := (x, y) <--- x,y in Z_p sampled randomly
pk := (g^x, g^y) in a pairing group G
```

# signing
on a set of messages **m**, generate a random polynomial *p* such that `deg(p) > |m|` and `p(x) = y`. a signature `s` on any message `m_i` is a KZG opening proof that `p(m_i) = H(m_i)` for some hash function `H`.

# verification
verification works like how one would verify an opening in a KZG scheme.
