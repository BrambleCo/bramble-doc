# Bramble
Programmable Crypto Rewards API for Apps & Games

## What is this document about?

This document will make you understand the technical flow of Bramble and how the Bramble API will work.

## Vision

To make cryptocurrency accessible and usable to every person on the planet.

## Objectives and Goals

To Increase engagement & revenue of an app or game by rewarding users in native cryptocurrency

### User Interaction Design

The current implementation of Blockchain is not meant for regular/novice users. Added steps to ensure privacy, decentralization while maintaining the usability aspect of the app has further made things complex. We can't imagine a non-tech person using the current iteration of blockchain implementation. So, What to do now?

#### Progressive Decentralization

We believe for a product to have majority adoption across the world, it must be easy to use while providing ten times the value of the incumbents. At our earliest iteration. Focusing more on user interface i.e. making it easy for non-tech users to access cryptocurrency, abstracting the core-tech of blockchain while keeping things less decentralized to get a product-market fit. This ensures that users can use the app without the knowledge of private keys, public keys, gas charges etc.
````
Less decentralization = Less friction = Better usability & Scalability
````
When we achieve a sufficient product market fit then, we can move on to Phase-2 of progressive decentralization by making the processes more open and transparent to users, adding scalability mechanisms of Layer-2 or ETH 2.0, while retaining the ease-of-usability aspect of the app. Thus, making the app more decentralized.

Learn more about progressive decentralization [HERE](https://a16z.com/2020/01/09/progressive-decentralization-crypto-product-management/)

#### Terms**
**1. Bramble's Wallet** - A custodial wallet based standalone app with user's profile, tracking the transaction happening across the api network. User's can sign in with their email ID and password like any regular sign-in/sign-up flow. Private keys will be stored in a secure server vault (For e.g. Coinbase, Binance).

**2. Push notification and Data transfer** - Just like how Google Play Standalone app automatically connect the game and rewards users in XP (experience points). Learn more about it [here](https://developers.google.com/games/services/management). 

**3. Gas Charges Abstraction** - Bramble will pay the gas charges on behalf of users thus, abstracting the complexities around blockchain transaction. Learn more about it [HERE](https://medium.com/zastrin/how-to-save-your-ethereum-dapp-users-from-paying-gas-for-transactions-abd72f15e14d)

### Bramble Flow

#### Game Developer's Flow

A player scores a reward, in background he redeems the brambles. If in game server he gets a daily quest completion / achievement completion, it will report this information to his bramble wallet via the game server. So he will not need to redeem every bramble, he will get that automatically once he links the bramble wallet to in-game account. So the game would just make push notifications to the wallet and not request any info. If he wants to see the wallet, he would need to login manually or from in-game link to bramble wallet. After a quest completion the game will send something as simple as 
````
{ user: 'user_id', reward: 'crossing_100_pipes'} 
````
and then the Bramble API will manage the data. If there are too easy and frequent gained rewards then the algorithm would decrease the reward. If the reward would be gained very rare, the algorithm would increase its value. The wallet will be linked to the game via Oauth 2.0 authorization. User would click on the in-game link to Bramble Wallet. The algorithm works on numerous parameters to mint tokens which users will be rewarded in, eventually.

````
for example: bramblewallet.com/link_account/flappybird?user=user_id&token=some_random_token
````
Bramble OAuth API will generate some wallet_token and send to game developer's backend to
````
flappybird.com/bramble_link?user=user_id&token=some_random_token&wallet_token=your_wallet_token
````
So the next time when user gets reward, the game will just send the wallet_token nd the reward info and if user wants to see Bramble wallet info, he will be redirected to Bramble page bramblewallet.com/login?user=user_id
So this would be an easy integration from the developer's side. The game developer don't need any info of your wallet, just will export data to the server on every achievement user gets and the game will have just a link to Bramble wallet. If user has linked, there will be a login link to the website. If user not yet linked the account, on the first time the account will be linked, transfered all yet gained old rewards. This flow can work for In-App purchases too where the game's native currency is replaced by Bramble. This process flow will be automated.


#### Bramble's Backend Flow
After the Reward data is received from the game, the Bramble API's algorithm will calculate the reward that is to be given to the user and call the Smart contract wherein the Bramble Cryptocurrency ( Ethereum blockchain ) is located. The Smart Contract will have a code ( Provable / Chainlink) to connect with the Bramble API. This whole process flow will be automated.

```
contract BrambleContract is usingProvable {
	----------
	--------
	-----------
    function __callback(string result) {
        if (msg.sender != provable_cbAddress()) revert();
        reward = result;
        LogPriceUpdated(result);
        balance[msg.sender] += reward;
    }

    function rewardPayout() payable { 
           provable_query(60, "URL", "json(https://api.bramble.co.in/verify_reward/user_id?).reward");
        }
    
    ------------
    ------------
}
```
