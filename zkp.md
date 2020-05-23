# zero knowledge proofs

> note: idk jack about the math or crypto behind this and it hurts my brain every time i try to understand so i will be using analogies *a lot*.

a zero knowledge proof (zkp) is a cryptographic proof of computational integrity. in this doc, we'll mostly be referring to zk-SNARKs which are zkps that are succint (small in size, fast and easy to verify) and non-interactive (you send the proof to the verifier and that's it).

a zkp allows a *prover* to provide to a *verifier* a proof of this nature:
> i know a ***s*** such that a statement ***p*** is true

and the verifier will be able to verify that *p* is true without the prover having to divulge *s*.

[why and how zk-snark works: definitive explanation](https://arxiv.org/pdf/1906.07221.pdf) is a great primer on zk-snarks that requires very little pre-requisite knowledge.

[zk-snarks in a nutshell](https://chriseth.github.io/notes/articles/zksnarks/zksnarks.pdf) is also an excellent read.


## examples

**secrets** will be in bold, and *statements* will be italicized
- i can point out **waldo's exact location** so *i know where waldo is in this where's waldo puzzle*.
  
  a zkp of this will prove we know where waldo is without divulging his location.
- i know a **set of transformation/computations** such that *we can get to this block starting from the genesis block*.
  
  this is how [coda](https://codaprotocol.com/) can compress their verifications of the entire blockchain into a very small zkp.
 
## techniques
the field of zkps advances every day with new constructions that make zkps faster and smaller seem to come out every month. however, new ways to leverage snarks are somewhat rarer. this is where we'll be reviwing techniques that allow you to do interesting things with zkps.
### recursive composition of snarks
this is essentially using sub-snarks to prove an overarching snark. say you had two snarks: `p1` that proves that `a -> b` and `p2` that proved that `b -> c`. you could produce a snark `p3` that proves `a -> c` using `p1 ^ p2`. this technique is very intuitive and quite simple but has *immense* potential for speeding up verifications of large computations (like blockchain verifications) since you're wrapping all these small and fast snark into a snark that's just as small and just as fast.
### proof modelling
this is less of a technique per se and more of a way of thinking about snarks. while only a select few in the world might be able to tell you *how* to prove something with a zkp, i believe anyone can think of interesting answers as to *what* to prove. modelling your proofs to prove interesting statements might be able to supercharge your products with seemingly "magical" powers.
