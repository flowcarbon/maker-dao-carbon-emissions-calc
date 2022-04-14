# Onchain Emissions

## Overview

Maker DAO is calculating emissions on a per-transaction basis. While the miners themselves 
are responsible for their carbon emissions, they are incentivised to do offset as a service for 
the protocols that require it for transactions to be processed.

We define the cut-off point for emissions on a per-protocol basis - meaning that the protocol 
initiating a transaction will be responsible for the emitted carbon. Consequently, transfers of 
tokens occurring within other protocols (e.g. the final balance transfer in an AMM transaction) 
is not part of the Maker DAO responsibility. 

This methodology allows for an exhaustive offsetting strategy: If each protocol would offset the transactions it is initiating, all transactions 
would be offset by definition - except pure base currency (Ether) transactions,  which would remain the responsibility of the miners.

Maker DAO is facilitating its on-chain transactions through two tokens: The DAI stablecoin and 
the MKR governance token. All transactions related to the protocol are occurring on these 
contracts. Flow Carbon will therefore define the cut-off point for the Maker DAO protocol to be 
exhaustive if the transactions for the aforementioned contracts are included in the calculation.


## Details

Maker DAO is on the ethereum blockchain, hence there are no other layers to take into account. 

Our methodology is transaction based. This means that we calculate the carbon footprint on a per-transaction basis,
that is the number of transactions of ethereum divided by the total carbon footprint within a given timeframe 
- a year in our case - is the carbon footprint per transaction.

The way we look at this is that the miners of the blocks are mining blocks as a service to protocols. 

While it would theoretically be possible to mine empty blocks, without transactions, that does not make sense from an economic perspective:The value of the tokens on the network are driven by its use case.

In a next step we are defining a transaction as “protocol originated” if and only if  an end user
(that is a wallet address and not a smart contract) has initiated a transaction or if it is a raw transaction on the protocol token (e.g. a transfer).

If a different protocol is facilitating function calls on a smart contract of the protocol subject to calculation the transaction is not considered to contribute to the footprint of the protocol. 
An example for that would be a token swap on uniswap: 

If someone facilitates a swap on an automated market maker like uniswap that includes DAI (the stablecoin token of MakerDAO), we contribute that transaction to uniswap.

This methodology exhaustive in terms of smart contract transactions: 

If every protocol on the ethereum blockchain would offset by that methodology, the chain would be carbon neutral - 

except for the raw ethereum transactions (which are not contributing that much but would most likely have to be offsetted by the ethereum foundation itself).

However, the downside to this is that quite a lot of protocols are only wrappers around wallets, multisignature wallets, transfer automation or plainly of unknown origin

In carbon emission calculation it is therefore the standard to overestimate emissions - after all our common goal is to make the planet liveable for decades to come.

Therefore we follow a simple principle:

> If in doubt: count towards our emissions.

That means if the origin is not a known (verified) protocol, we do count them to MakerDAOs emissions. If slightest doubts about the origin exists, we count it as well.


## How to interpret the dataset

We added two CSV files - one for the MKR token and one for the DAI token. Please refer to the respective Jupyter Notebook for a visual walkthrough.

The `num_tx` column is the total interactions with DAI/MKR over the respective timeframe by the `address`.

If `smart_contract` is `true`, the `address` is a smart-contract, meaning code has been deployed to that address. 
`false` indicates that it is a wallet.

`origin` can have the following:

1. `MultiSigWalletWithDailyLimit`, `MultiSigWalletWithTimeLock`, `GnosisSafeProxy`, `multi_sig`, `DSProxy`, `Argent`

These are multisignature wallets and they contribute to Maker's footprint.

2. `market_maker`

Bot, market maker or otherwise automated trades. If they are not a smart contract their interactions 
are contributing to Maker's footprint - because that means they interacted with Maker's Smart Contracts directly. 

Smart Contracts are interacting with exchanges under the hood and are therefore not added if they are 
a smart contract.

3. `exchange`

Addresses that can be assigned to exchanges. 

Since it could be argued that they are responsible for their own emissions, it's a direct interaction 
with our protocol, therefore we count it towards Maker's emissions.

4. `not_verifiable` 

Manually double checked addresses where we couldn't verify the origin. The more tx are done, the more likely it 
is that this is a service or facilitating protocol. However, in the event of doubt we count to Maker's emissions.

Therefore all within this category are counted.

5. `unknown`
Smart Contract of unknown and not manually checked origin. This only applies for addresses with less than 100tx.

Counted towards Maker's emissions.

7. `wallet`
Same as unknown, but for wallets. Again, this only applies for addresses with less than 100tx.

Counted towards Maker's emissions.

8. `bridge`

Bridging wallet or smart contract. If this is a wallet it's a direct interaction and counts towards
makers emissions. Smart Contracts are not taken into account. 

9. Everything else.
All other transcations are known and verified smart contract protocols that are responsible for their own emissions.

They are not counted as Maker's emissions.



