# apathetic rollups

similar to optimistic rollups, as in we accept updates sort of "optimistically", but we're not concerned with immediate validity since they won't be able to prove it in the long run. tryna do away with proactive fraud proofs in optimistic rollups.

anyone can append wtv they want to the state root, but a malicious party will never be able to prove an invalid withdrawal. apathetic rollups stop malicious actions at "exit time" but allow anything else.

hoping to achieve higher throughput than pessimistic rollups while avoiding the dispute periods that come with fraud proofs.

i think [zkopru](https://docs.zkopru.network/) is pretty close to what apathetic rollups are trying to accomplish.

[eth-9¾](https://ethresear.ch/t/ethereum-9-send-erc20-privately-using-mimblewimble-and-zk-snarks/6217)

## fundamentals
we accept updates without verification, but we need some version of the past state to remain so that we can check for validity at exit time. the typical optimistic setup isn't suitable for this since we replace state at each transition. this state needs to be checked for fraud quickly so that we can revert. if we want to do away with fraud proofs and only check for validity at exit time, we need an append-only data structure such that we can retrace our current withdrawal's origin. an append-only data structure will preserve valid transactions along with invalid ones. we just need a mechanism such that invalid transations can never pass a withdrawal check, and that valid transactions can't get replayed.

a zk-SNARK proof at exit time could do the trick, altho checking a snark at every rollup (the zk-rollup/pessimistic approach) would be too expensive, checking once at exit time more than makes up for the cost. something without a snark would be best since it's hard to represent general computation in snarks.

|                 | Optimistic           | Pessimistic          | Apathetic                                               |
|:---------------:|:--------------------:|:--------------------:|:--------------------------------------------------------|
| Computation     | generalizable        | hard to generalize   | hard to generalize                                      |
| Finality        | slow (~1 week)       | fast (seconds)       | fast (seconds)                                          |
| Proof Type      | fraud proof          | validity proof       | validity proof (at exit time)                           |
| Transition      | cheap & fast         | expensive & slower   | expensive & slower at exit time, otherwise cheap & fast |
| Private         | possible             | by default           | possible                                                |
| Scaling Benefit | ~100 txs per rollup  | ~1000 txs per rollup | ~1000 txs per rollup                                    |

pessimistic are conceptually slower and more expensive due to longer prover times and verifier times. luckily this is sublinear in the amount of txs in the rollup ans they make up for it with fast finality, but transitions are still slower than acccepting updates optimistically or apathetically. apathetic rollups delay the slow and expensive part until exit time, where a snark is needed to prove a valid withdrawal.

the security assumptions made in optimistic rollups are a lot simpler than the ones in pessimistic or apathetic rollups.

## cryptography
can halo snarks be accumulated apathetically? so we incrementally accumulate it apathetically and check for validity at the end.

[streaming merkle proofs within binary numeral trees](https://eprint.iacr.org/2021/038.pdf), with support for:
- single leaf, single range and multi (disjoint) range proofs
- merkle "diff" proofs

thinking this data structure will be key for apathetic rollups

## points of failure
Given that we accept updates "apathetically", we don't check for validity until the end. This gives a lot of time for adversaries to commit malicious acts like:
1. double spends
2. valid txs but not committed
3. valid txs and committed but no valid trace from origin
4. exit replay attack

### facts
- valid exits refer to valid history
- partiess are in charge of monitoring their own valid state

we append transition, to be coallessed at exit time. can we append snarks as transitions?
