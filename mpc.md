# notes on secure multiparty computation (mpc)

> Secure multi-party computation (also known as secure computation, multi-party computation (MPC), or privacy-preserving computation) is a subfield of cryptography with the goal of creating methods for parties to jointly compute a function over their inputs while keeping those inputs private. - [wikipedia](https://en.wikipedia.org/wiki/Secure_multi-party_computation)

## shamir secret sharing (sss)
shamir secret sharing is an interesting algorithm for splitting a secret into pieces called shares such that the secret can only be reconstructed if no less that *t of n* shares are assembled. what's so cool about this is having even as much as *t - 1* shares will not get you closer to reconstructing the secret as having only 1 share.

a quick overview of the math behind this:
- generate a polynomial *p* of degree *t - 1*
- note: it takes no less than *t* points to define a polynomial of degree *t - 1*
- note: the coefficients of the polynomial can be anything, but p(0) must equal the secret we are trying to mask
- construct n points defined as (*i*, *p(i)*) for any integer *i*
- when *t* or more points are assembled, reconstruct the polynomial using Lagrange Interpolation
- uncover the secret by evaluating the polynomial at 0

shamir secret sharing is a powerful algorithm for masking secret in a way that makes them impossible to reconstruct without enough pieces.

## homomorphic encryption (he)
a homomorphic encryption scheme is an encryption scheme that allows for homomorphic computation on encrypted data.
```
let f be an encryption function
say we want to compute f(a + b)
where a is a secret value that belongs to A, and b belongs to B
A doesn't want a to be revealed, likewise for B with b
instead of submitting a and b to be summed up
A computes f(a) and B computes f(b)
the two sum up to f(a + b) i.e. f(a) + f(b) = f(a + b)
```

## succint secure aggregation (ssa)
secure aggregation (sa) is a protocol developed by google to securely aggregate data from *n* parties. the goal is to compute the aggregation of the data from all n parties without our aggregator being able to read any individual party's data.

an *aggregation* is a function, like a summing function for example. an *aggregator* is a third party that aggregates every party's data using an aggregation. the aggregator can be a server for example. the aggregator is not supposed to have any insight on an individual's data.

the main use case google developed this protocol for was for federated machine learning, where users can submit gradients to google's ml models so that google could train its models in a distributed fashion.

my gripe with sa is how much communication is needed between each party in order to properly mask your data before sending it to the aggregator. a much better protocol would be one where each party simply sends their data to the aggregator, and the aggregator cannot compute anything until a sufficient threshold of users have submitted their data for aggregation as well as not being able to decipher any individual submission.

a non-interactive protocol would significantly speed things up when it comes to secure aggregation. i call this potential protocol **succint secure aggregation**.
