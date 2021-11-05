## Uniswap V3 Staker Subgraph

Subgraph to be used by the [Elyfi LP staking](https://github.com/elsia-dev/elyfi-web).

### Examples

- https://api.studio.thegraph.com/query/862/lp-staking/v0.0.1 (mainnet)
- https://api.studio.thegraph.com/query/862/lp-staking-rinkeby/v0.0.1 (rinkeby)

## Schema

We add a relation entity(IncentivePosition) for getting more detail information of staked positions which is used elyfi-web.
Position's `staked` status meaning is also modified. In our repo, it means that the position is owned by the Staker. When `DepositTransferred` event is emitted, the status is udpated to false.

```
type Incentive @entity {
  id: ID!
  rewardToken: Bytes!
  pool: Bytes!
  startTime: BigInt!
  endTime: BigInt!
  refundee: Bytes!
  reward: BigInt!
  ended: Boolean!
  incentivePotisions: [IncentivePosition!]! @derivedFrom(field: "incentive")
}

type IncentivePosition @entity {
  id: ID!
  active: Boolean!
  incentive: Incentive!
  position: Position!
}

type Position @entity {
  id: ID!
  tokenId: BigInt!
  owner: Bytes!
  staked: Boolean!
  oldOwner: Bytes
  liquidity: BigInt!
  approved: Bytes
  incentivePotisions: [IncentivePosition!]! @derivedFrom(field: "position")
}
```

### Deployment

1. Create a new subgraph at https://thegraph.com/explorer/subgraph/create. You might want to create an additional one for other testnets e.g. rinkeby
2. Update `package.json` and `Makefile` to match the created subgraphs.
3. Ran `yarn` to install node packages.
4. Deploy e.g. to mainent with `make mainnet`
5. Visit the subgraphs and verify no errors in indexing.

### License

MIT
