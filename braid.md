# braid protocol

> braid is a set of extensions that generalize HTTP from a state
   *transfer* protocol into a state *synchronization* protocol.  braid
   puts the power of operational transform and CRDTs on the web,
   improving network performance and enabling natively peer-to-peer,
   collaboratively-editable, offline-first web applications. - [taken from IETF internet draft](https://raw.githubusercontent.com/braid-work/braid-spec/master/draft-toomim-httpbis-braid-http-03.txt)
   
braid adds these features to HTTP:
- versioning to HTTP resources
- subscriptions to GET requests
- patches to Range Requests
- merge-types to specify OT or CRDT behavior

Learn more at [braid.news](https://braid.news/)

---
## motivation
braid's goal is to extend http to state sync protocol (from http to hypertext synchronization protocol), in order to do away with custom sync protocols and make state across the web more interroperable.

## modifications
it turns out that http is very close to being a htsp, we just need to add 5 headers to requests and responses.
  
| Header Name        | Property             | Description                                         |
|:------------------:|:--------------------:|:----------------------------------------------------|
| Version            | version-hash         | Version identifier for a resource in time           |
| Parents            | [version-hash]       | Array of version hashes, versions form a DAG        |
| Merge-Type         | sync-protocol        | Specify a merge type to synchronize a resource      |
| Patches            | [patch]              | Array of patches, transformations to a resources    |
| Subscribe          | keep-alive=(seconds) | Flag for subscription, optional (seconds) parameter |

braid does make http stateful, servers need to maintain version history and a list of subcribers to send minimal diffs to clients. how this is persisted is left up to clients and servers, braid describes a implementation agnostic format for synchronizing between peers.

## adoption
the easiest strategy for accelerating adoption of the braid protocol is by implementing middleware for many server/clients in different languages. developers won't have to worry about "braidifying" their requests/responses.

[wai-braid](https://github.com/ghiliweld/wai-braid) is a haskell [wai](https://www.yesodweb.com/book/web-application-interface) middleware for transforming outgoing requests into a braid format adding the appropriate headers.
