# xETH - Mitigation Review details
- Total Prize Pool: $6,500 USDC 
- [Warden guidelines for C4 mitigation reviews](https://code4rena.notion.site/Guidelines-for-C4-mitigation-reviews-ed10fc5cfbf640bd8dcec66f38b343c4)
- Submit findings [using the C4 form](https://code4rena.com/contests/2023-06-xeth-mitigation-review/submit)
- Starts June 15, 2023 20:00 UTC 
- Ends June 20, 2023 20:00 UTC 

## Important note 

Each warden must submit a mitigation review for *every High and Medium finding* from the parent audit. **Incomplete mitigation reviews will not be eligible for awards.**

## Findings being mitigated

Mitigations of all High and Medium issues will be considered in-scope and listed here.

- [M-02: Inconsistent check for LP balance in AMO](https://github.com/code-423n4/2023-05-xeth-findings/issues/33)
- [M-03: Zero token transfer can cause a potential DoS in CVXStaker](https://github.com/code-423n4/2023-05-xeth-findings/issues/30)
- [M-04: Unspent allowance may break functionality in AMO](https://github.com/code-423n4/2023-05-xeth-findings/issues/29)
- [M-05: Virgin stake can claim all drops](https://github.com/code-423n4/2023-05-xeth-findings/issues/23)
- [M-07: Incorrect slippage check in the AMO2.rebalanceUp can be attacked by MEV](https://github.com/code-423n4/2023-05-xeth-findings/issues/14)
- [M-08: CVXStaker.sol Unable to process newly add rewardTokens](https://github.com/code-423n4/2023-05-xeth-findings/issues/8)
- [M-09: withdrawAllAndUnwrap() the clpToken transfer to AMO.sol may be locked in the contract](https://github.com/code-423n4/2023-05-xeth-findings/issues/6)
- [M-10: First 1 wei deposit can produce lose of user xETH funds in wxETH](https://github.com/code-423n4/2023-05-xeth-findings/issues/3)

## Overview of changes

- Many issues assume a rogue defender as a starting point of attack, so we have chosen to add more granularity of parameters and checks to reduce risks there.
- Most mitigations would either be checks, or smaller changes of function parameters and calls
- In production we have planned to use MEV Protection services such as flashbots rpc

## Mitigations to be reviewed

The mitigations are already commited in [this branch](https://github.com/code-423n4/2023-05-xeth/tree/add-xeth) and they are in individual commits.

| URL | Mitigation of | Purpose | 
| ----------- | ------------- | ----------- |
| https://github.com/code-423n4/2023-05-xeth/commit/60b9e1e2562d585831e3d8e2a7e345294d9dacbd | M-02 | This mitigation adds a getTotalBalance function | 
| https://github.com/code-423n4/2023-05-xeth/commit/1f714868f193cdeb472ec097110901a997d87ec4 | M-03 | This mitigation adds a balance check | 
| https://github.com/code-423n4/2023-05-xeth/commit/793dade5217bd5751856f7cf0bccd4936286aeab | M-04 | change safeApprove to approve | 
| https://github.com/code-423n4/2023-05-xeth/commit/aebc3244cbb0deb67f3cdef160b390da27888de7 | M-05 | add a totalSupply check | 
| https://github.com/code-423n4/2023-05-xeth/commit/630114ea6a3782f2f539b5936c524f8da62d0179 | M-07 | Partial: add admin controlled slippage values |
| https://github.com/code-423n4/2023-05-xeth/commit/1f714868f193cdeb472ec097110901a997d87ec4 | M-08 | Add a setRewardTokens function |
| https://github.com/code-423n4/2023-05-xeth/commit/a840dc0b8a1de59a3ea06e0814ea3ce26707bdae | M-09 | change sendToOperator to sendToOwner |
| https://github.com/code-423n4/2023-05-xeth/commit/fbb29727ce21857f60c1c94fff43c73b4124a4b4 | M-10 | Don't allow minting of 0 shares.

## Out of Scope

2 issues (M-01 and M-06) will be solved at the time of deployment by minting deadshares, to avoid first-staker attacks. 
- We will mint shares through a multi-sig
- We will transfer these shares to a contract (that has no way of redeeming these funds and is just created for holding these shares).
