# apathetic rollups

similar to optimistic rollups, as in we accept updates "optimistically", but we're not concerned with immediate validity since they won't be able to prove it in the long run. tryna do away with proactive fraud proofs in optimistic rollups.

anyone can append wtv they want to the state root, but a malicious party will never be able to prove an invalid withdrawal. apathetic rollups stop malicious actions at "exit time" but allow anything else.

hoping to achieve higher throughput than pessimistic rollups while avoiding the dispute periods that come with fraud proofs.

## fundamentals
we accept updates without verification, but we need some version of the past state to remain so that we can check for validity at exit time. the typical optimistic setup isn't suitable for this since we replace state at each transition. this state needs to be checked for fraud quickly so that we can revert. if we want to do away with fraud proofs and only check for validity at exit time, we need an append-only data structure such that we can retrace our current withdrawal's origin. an append-only data structure will preserve valid transactions along with invalid ones. we just need a mechanism such that invalid transations can never pass a withdrawal check, and that valid transactions can't get replayed.

a zk-SNARK proof at exit time could do the trick, altho checking a snark at every rollup (the zk-rollup/pessimistic approach) would be too expensive, checking once at exit time more than makes up for the cost. something without a snark would be best since it's hard to represent general computation in snarks.

## cryptography
[streaming merkle proofs within binary numeral trees](https://eprint.iacr.org/2021/038.pdf), with support for:
- single leaf, single range and multi (disjoint) range proofs
- merkle "diff" proofs

thinking this data structure will be key for apathetic rollups
