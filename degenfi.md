# degenerate finance
crypto finance but 😈degenerate😈, scared money don't make no money after all. 

this doc is for following new crazy trends in defi, and the new financial primitives being built on ethereum.

[twitter thread](https://twitter.com/ghiliweld/status/1370199762813747200)

crypto opens up interesting avenues for more sophisticated investment/trading strategies, esp since defi protocols are permissionless and can easily be composed with each other.

<details>
  <summary>questions</summary>
  
  - why does each protocol need their own liquidity pool? couldn't there be one liquidity pool supplyinng the entire defi ecosystem?
</details>

### flash loans
flash loans are uncollateralized loans of assets for a one transaction block in a blockchain. any amount from a liquidity pool can be borrowed for anything as long as the borrowed amount is returned at the end of the trannsaction, or else the flash loan is rolled back. one natural use case for flash loans are [capital-free arbitrage](https://uniswap.org/docs/v2/core-concepts/flash-swaps/#capital-free-arbitrage) opportunities. if you spot an arbitrage opportunity that you cannot make full use of with your assets alone, you can borrow millions of dollars for one tx, return it at the end of the tx and pocket the difference. the loan is risk-free for the lender since the smart contract can ensure the funds are always returned and the only cost associated is gas fees the borrower has to pay for the loan. 

**it's free money!**

flash loans are the most exciting thing for me in defi since it democratizes arbitrage for anyone, removing the high barrier of sufficient capital to profit from market inefficiencies.

flash loans are also appealing to malicious actors who need the capital to carry out attacks they would otherwise not be able to pull off on their own.

here are some interesting articles and papers on flash loans:
- [Flash Loans Are Providing Instant Cash to Crypto Speculators](https://www.bloomberg.com/news/articles/2021-02-07/flash-loans-are-providing-instant-cash-to-crypto-speculators)
- [Attacking the DeFi Ecosystem with Flash Loans for Fun and Profit](https://hackingdistributed.com/2020/03/11/flash-loans/), [paper](https://arxiv.org/pdf/2003.03810.pdf)
- [The DeFi ‘Flash Loan’ Attack That Changed Everything](https://www.coindesk.com/the-defi-flash-loan-attack-that-changed-everything)
- [On the Just-In-Time Discovery of Profit-Generating Transactions in DeFi Protocols](https://arxiv.org/pdf/2103.02228.pdf)

![cmon fam...](https://cdn.discordapp.com/attachments/819698225492525106/819977208357060638/Screen_Shot_2021-03-12_at_11.56.21_AM.png)

#### bypassing MEV
**ok so it's not free money apparently, lol**
here's some articles on it:
- [MEV and me](https://research.paradigm.xyz/MEV)
- [Ethereum is a Dark Forest](https://medium.com/@danrobinson/ethereum-is-a-dark-forest-ecc5f0505dff)
- [tweet](https://twitter.com/FrankResearcher/status/1366795352330948610)

in short, no sane miner will leave money on the table. since miners can see txs before they're included on-chain, they can also frontrun it themselves to bank the profit for themselves. any solution to counteract this needs to prevent miners from extracting value for themselves, as well as incentivize at least one miner to act honestly.

is there a way to get miners competing against each other that's in our best interest? such that any miner that attempts a chain reorg will lose to other miners acting honestly, or sum like that.

miners get rekt in [salmonella](https://github.com/Defi-Cartel/salmonella)

### yield farming
i will summarize [this article](https://coinmarketcap.com/alexandria/article/what-is-yield-farming) and [this article](https://www.coindesk.com/defi-yield-farming-comp-token-explained).

yield farming is staking or lending crypto assets to generate interest from it. starts off simple enough, but the real fun starts when defi protocols also issue out governance tokens to users providing liquidity (LPs). given defi's composability, sophisticated investors have started hacking together ways to generate more yield across different defi protocols and maximize their APY. the yields even on simple farming tactics are fairly impressive, but with higher rewards comes way higher risk, especially as these farming tactics can get pretty complex.

defi pulse has a [great newsletter](https://yieldfarmer.substack.com/) for following trends in yield farming.
