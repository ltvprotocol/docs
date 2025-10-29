# LTV Protocol — Vault Launch Process

This document describes how new **LTV leveraged vaults** are launched on mainnet.  
Each vault goes through a **four-phase rollout** to ensure stable growth, safe liquidity expansion, and verified performance before opening to the public.

## Launch Rationale

Every vault launch follows the same principle:  
**start small, prove stability, scale gradually.**

The process is designed to minimize risk during early stages and progressively expand capacity as confidence in vault mechanics grows.

## **Phase 1 — Initial / Testing liquidity**

Mainnet contracts are deployed with initial liquidity provided by the **LTV team and founders**.  
At this stage, the vault runs in a controlled environment to:

- Monitor real-time performance and contract behavior.  
- Verify integrations with the underlying lending protocol.  
- Ensure correct leverage, health factor tracking, and price feed behavior.

Only internal capital is used during this phase.

## **Phase 2 — Private LPs**

Once stability is confirmed, **private liquidity providers** are onboarded through a direct whitelist.  
This step expands the vault’s liquidity under controlled participation.

Key goals:

- Test scalability under moderate load.  
- Confirm smooth operation of deposits, withdrawals, etc.

## **Phase 3 — 42 NFT holders access**

Access expands beyond private LPs to a limited group of community participants - 42 NFT holders.
This phase allows early external users to participate before public access, while still maintaining conservative capacity limits.

It marks the beginning of vault’s broader operation but still under observation to ensure stability at scale.

## **Phase 4 — Public launch**

Vaults become fully **permissionless** and open to everyone.  
All prior restrictions are lifted, and capacity increases according to risk parameters and lending protocol liquidity.

From this point, vaults operate autonomously and continuously adjust to market conditions.

## **Capacity Management**

Each vault has its **own capacity limits**, which expand gradually throughout the rollout.

Two main factors control these limits:

1. **Internal Risk Management** — capacity grows step by step, starting from team capital and expanding as the system proves stable.  
2. **Protocol Liquidity** — total vault size depends on available liquidity within the underlying lending protocol (e.g., Aave V3), which naturally defines how far scaling can go.

This layered approach balances growth and safety, ensuring stability at every stage.
