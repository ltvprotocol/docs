# LTV Protocol Points program

## Overview

The **LTV Protocol Points program** is a loyalty and engagement system designed to reflect participation activity for users contributing liquidity to the protocol.  
It reflects both **commitment** and **duration** of participation, aligning incentives between the protocol and its community.

## Program objectives

The Points Program is built to:

- Encourage long-term liquidity provision.  
- Recognize active community members who support early stages of the protocol.  
- Create a transparent metric for user contribution and engagement.  

Points are a **measure of contribution and loyalty** — reflecting both **how much** and **how long** liquidity has been provided.

## Earning Points

### Base Points

Participants accumulate base points automatically through:

- **Liquidity Amount:** The volume of assets deposited into LTV Protocol vaults.  
- **Holding Duration:** The length of time liquidity remains deposited without withdrawal.

Both metrics will be calculated continuously to ensure fair and transparent distribution.

### Multipliers

Points can be **boosted** through specific multipliers that reflect early or extended participation.

| Multiplier Type | Description | Boost |
|------------------|--------------|--------|
| **42 NFT Holder Bonus** | Exclusive multiplier for holders of 42 NFTs. | **+42%** |

Further multipliers and conditions will be announced closer to the public mainnet release.

## Points calculation formula

The total number of points earned by a participant is determined by the following formula:
```
Total Points = Base Points (Liquidity × Duration) × (1 + ΣMultipliers)
```
Where:
- **Base Points** reflect the combination of deposit size and time held.  
- **ΣMultipliers** represents the sum of all applicable percentage boosts (e.g., +42% from NFT).  

Formula subject to adjustment prior to final point program release.

## Participation rules

- **Automatic enrollment:** Any wallet interacting with LTV Protocol vaults after the program launch becomes eligible to participate
- **Transparency:** User balances and total leaderboard rankings will be publicly viewable via the LTV Protocol interface after launch.  
- **Non-transferable:** Points are tied to wallet activity and cannot be sold, transferred, or delegated.  

## Relation to 42 NFT

The **42 NFT** is directly integrated with the Points program.  
Holders receive a **+42% multiplier** on all points earned.  
It also grants early access to the closed beta phase, allowing holders to participate in the point phase earlier than the general public.

For detailed information, refer to the [42 NFT documentation](./nft_42.md).
