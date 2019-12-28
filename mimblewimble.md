# Notes on MimbleWimble

MimbleWimble (mw from now on), is a new blockchain design I just started looking into. First introduced in July 19, 2016, dropped anonymously in a bitcointalk thread under the pseudonym  "Tom Elvis Jedusor" (French Voldemort), mw is a proposal for light, simple, and elegant blockchain design that uses tried and tested cryptography (the same primitives in bitcoin and other chains!) in pretty dope ways.

For a detailed primer on mw, please read the [introduction to mw](https://github.com/mimblewimble/grin/blob/master/doc/intro.md) written by the team at Grin.

My understanding of it, by analogy (might be a very poor analogy), is that mw does away with the public ledger design of blockchains of old (lol) and replaces it with a more compact "black box" if you will.

Imagine you are in a room transacting with a friend, no one can see you enter, transact or leave. You are both facing this black box/blockchain ready to transact. You have 10 coins, you want to send 4 to your friend. To do so, you both place your fingers on the box, the box scans your fingerprint and will use it to mark the coins (fingerprints are crypto signatures). The box searches its coffers for coins marked by your fingerprint, removes the print on 4 of them, and adds your friends prints on them. The coins are now theirs, congrats you've just transacted!

The box, in the background, made sure that in the process of finding your coins, removing the markings and adding new ones, no coins were added from thin air or removed from circulation.
Please note that we cannot inspect the box for any histroy telling us who transacted at certain times, we can only see that transactions were in fact done with the box. The markings on the coins are also important for the analogy because if we were to see the coins inside the coffers we would be able to see there is in fact money, but since they're all in one pot we can't tell who's money is who's. Assuming it's a human that is analyzing the coins and trying to map to fingerprints. Ofc, cryptography is a lot more robust than fingerprints and can withstand computers but regardless the analogy still works(ish). So we can prove that we in fact do possess a certain amount of coins without anyone being able to deduce how many coins we have.

mw's main privacy benefits over bitcoin are:
- we cannot tell who is transacting with who
- we cannot deduce what amount of coins are moving from transaction to transaction
- we cannot inspect accounts for balances, notably since the concepts of accounts do not exist in mw, only marked funds that belong to a public key (the fingerprint)

mw also has scalability benefits that I will not delve into, but that's also cool too

My interests lie mostly in equiping mw with zk proofs for proving our balance is above a certain value and giving mw  scripting abilities thru the magic of cryptography.
I'll be exploring those here and in [magic scripts](magic-scripts.md).
