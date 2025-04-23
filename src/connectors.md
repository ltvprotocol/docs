# Connectors - Interfaces for Integration.

The LTV protocol uses connectors to stay modular and adaptable, allowing seamless integration with various external systems like lending protocols and price oracles. This design ensures that the vault logic remains unchanged even as the ecosystem evolves or new protocols emerge. By abstracting these components, the protocol can support permissionless deployments across different chains and asset pairs. It also enables governance flexibility.
# Lending Protocol Connector

The Lending Protocol Connector determines how the vault interacts with external lending platforms to deposit collateral and borrow assets. It gives the LTV system flexibility to integrate with various protocols such as Aave, Compound, or custom-built solutions like the HodlMyBeer protocol used on testnet. Since the connector acts as a modular layer, the core vault logic remains unchanged even if the underlying lending protocol is swapped out.

# Oracle Connector

The Oracle Connector handles how the vault obtains and interprets price data for assets like LSTs or borrow tokens. This data can come from sources like Chainlink, a specialized oracle such as Spooky Oracle, or even DEX-based price feeds. With accurate and modular oracle integration, the vault can continuously evaluate its leverage position and make informed rebalancing decisions.

# Slippage Connector

The Slippage Connector in the LTV protocol framework is a mechanism that governs how price slippage is accounted for during operations like rebalancing and auctions. It influences how assets are valued and incentivizes participation in auctions by adjusting for market impact.
