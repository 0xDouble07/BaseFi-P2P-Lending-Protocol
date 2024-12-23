# Aderyn Analysis Report

This report was generated by [Aderyn](https://github.com/Cyfrin/aderyn), a static analysis tool built by [Cyfrin](https://cyfrin.io), a blockchain security company. This report is not a substitute for manual audit or security review. It should not be relied upon for any purpose other than to assist in the identification of potential security vulnerabilities.
# Table of Contents

- [Summary](#summary)
  - [Files Summary](#files-summary)
  - [Files Details](#files-details)
  - [Issue Summary](#issue-summary)
- [Low Issues](#low-issues)
  - [L-1: Centralization Risk for trusted owners](#l-1-centralization-risk-for-trusted-owners)
  - [L-2: Unsafe ERC20 Operations should not be used](#l-2-unsafe-erc20-operations-should-not-be-used)
  - [L-3: Solidity pragma should be specific, not wide](#l-3-solidity-pragma-should-be-specific-not-wide)
  - [L-4: `public` functions not used internally could be marked `external`](#l-4-public-functions-not-used-internally-could-be-marked-external)
  - [L-5: Event is missing `indexed` fields](#l-5-event-is-missing-indexed-fields)
  - [L-6: PUSH0 is not supported by all chains](#l-6-push0-is-not-supported-by-all-chains)
  - [L-7: Empty Block](#l-7-empty-block)
  - [L-8: Large literal values multiples of 10000 can be replaced with scientific notation](#l-8-large-literal-values-multiples-of-10000-can-be-replaced-with-scientific-notation)


# Summary

## Files Summary

| Key | Value |
| --- | --- |
| .sol Files | 2 |
| Total nSLOC | 153 |


## Files Details

| Filepath | nSLOC |
| --- | --- |
| src/Counter.sol | 10 |
| src/LendingPool.sol | 143 |
| **Total** | **153** |


## Issue Summary

| Category | No. of Issues |
| --- | --- |
| High | 0 |
| Low | 8 |


# Low Issues

## L-1: Centralization Risk for trusted owners

Contracts have owners with privileged rights to perform admin tasks and need to be trusted to not perform malicious updates or drain funds.

<details><summary>3 Found Instances</summary>


- Found in src/LendingPool.sol [Line: 8](src/LendingPool.sol#L8)

	```solidity
	contract LendingPool is ReentrancyGuard, Ownable {
	```

- Found in src/LendingPool.sol [Line: 40](src/LendingPool.sol#L40)

	```solidity
	    function addSupportedToken(address token) external onlyOwner {
	```

- Found in src/LendingPool.sol [Line: 171](src/LendingPool.sol#L171)

	```solidity
	    function updatePrices() external onlyOwner {
	```

</details>



## L-2: Unsafe ERC20 Operations should not be used

ERC20 functions may not behave as expected. For example: return values are not always meaningful. It is recommended to use OpenZeppelin's SafeERC20 library.

<details><summary>6 Found Instances</summary>


- Found in src/LendingPool.sol [Line: 56](src/LendingPool.sol#L56)

	```solidity
	        bool success = IERC20(token).transferFrom(msg.sender, address(this), amount);
	```

- Found in src/LendingPool.sol [Line: 88](src/LendingPool.sol#L88)

	```solidity
	        bool collateralSuccess = IERC20(collateralToken).transferFrom(msg.sender, address(this), collateralAmount);
	```

- Found in src/LendingPool.sol [Line: 91](src/LendingPool.sol#L91)

	```solidity
	        bool borrowSuccess = IERC20(borrowToken).transfer(msg.sender, actualBorrowAmount);
	```

- Found in src/LendingPool.sol [Line: 116](src/LendingPool.sol#L116)

	```solidity
	        bool success = IERC20(token).transferFrom(msg.sender, address(this), amount);
	```

- Found in src/LendingPool.sol [Line: 132](src/LendingPool.sol#L132)

	```solidity
	        bool success = IERC20(token).transfer(msg.sender, amount);
	```

- Found in src/LendingPool.sol [Line: 155](src/LendingPool.sol#L155)

	```solidity
	        bool success = IERC20(position.collateralToken).transfer(msg.sender, position.collateralAmount);
	```

</details>



## L-3: Solidity pragma should be specific, not wide

Consider using a specific version of Solidity in your contracts instead of a wide version. For example, instead of `pragma solidity ^0.8.0;`, use `pragma solidity 0.8.0;`

<details><summary>2 Found Instances</summary>


- Found in src/Counter.sol [Line: 2](src/Counter.sol#L2)

	```solidity
	pragma solidity ^0.8.13;
	```

- Found in src/LendingPool.sol [Line: 2](src/LendingPool.sol#L2)

	```solidity
	pragma solidity ^0.8.28;
	```

</details>



## L-4: `public` functions not used internally could be marked `external`

Instead of marking a function as `public`, consider marking it as `external` if it is not used internally.

<details><summary>3 Found Instances</summary>


- Found in src/Counter.sol [Line: 7](src/Counter.sol#L7)

	```solidity
	    function setNumber(uint256 newNumber) public {
	```

- Found in src/Counter.sol [Line: 11](src/Counter.sol#L11)

	```solidity
	    function increment() public {
	```

- Found in src/LendingPool.sol [Line: 163](src/LendingPool.sol#L163)

	```solidity
	    function calculateInterest(address token, address user) public view returns (uint256) {
	```

</details>



## L-5: Event is missing `indexed` fields

Index event fields make the field more quickly accessible to off-chain tools that parse events. However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (three fields). Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

<details><summary>6 Found Instances</summary>


- Found in src/LendingPool.sol [Line: 31](src/LendingPool.sol#L31)

	```solidity
	    event Deposit(address indexed token, address indexed user, uint256 amount, uint256 ownerFee);
	```

- Found in src/LendingPool.sol [Line: 32](src/LendingPool.sol#L32)

	```solidity
	    event Borrow(address indexed token, address indexed user, uint256 amount, address collateralToken, uint256 collateralAmount, uint256 ownerFee);
	```

- Found in src/LendingPool.sol [Line: 33](src/LendingPool.sol#L33)

	```solidity
	    event Repay(address indexed token, address indexed user, uint256 amount, uint256 ownerFee);
	```

- Found in src/LendingPool.sol [Line: 34](src/LendingPool.sol#L34)

	```solidity
	    event Withdraw(address indexed token, address indexed user, uint256 amount);
	```

- Found in src/LendingPool.sol [Line: 35](src/LendingPool.sol#L35)

	```solidity
	    event Liquidate(address indexed token, address indexed borrower, address liquidator, uint256 amount, uint256 ownerFee);
	```

- Found in src/LendingPool.sol [Line: 36](src/LendingPool.sol#L36)

	```solidity
	    event OwnerFeeClaimed(address indexed token, uint256 amount);
	```

</details>



## L-6: PUSH0 is not supported by all chains

Solc compiler version 0.8.20 switches the default target EVM version to Shanghai, which means that the generated bytecode will include PUSH0 opcodes. Be sure to select the appropriate EVM version in case you intend to deploy on a chain other than mainnet like L2 chains that may not support PUSH0, otherwise deployment of your contracts will fail.

<details><summary>2 Found Instances</summary>


- Found in src/Counter.sol [Line: 2](src/Counter.sol#L2)

	```solidity
	pragma solidity ^0.8.13;
	```

- Found in src/LendingPool.sol [Line: 2](src/LendingPool.sol#L2)

	```solidity
	pragma solidity ^0.8.28;
	```

</details>



## L-7: Empty Block

Consider removing empty blocks.

<details><summary>1 Found Instances</summary>


- Found in src/LendingPool.sol [Line: 171](src/LendingPool.sol#L171)

	```solidity
	    function updatePrices() external onlyOwner {
	```

</details>



## L-8: Large literal values multiples of 10000 can be replaced with scientific notation

Use `e` notation, for example: `1e18`, instead of its full numeric value.

<details><summary>1 Found Instances</summary>


- Found in src/LendingPool.sol [Line: 46](src/LendingPool.sol#L46)

	```solidity
	        return (amount * OWNER_FEE_BPS) / 10000;
	```

</details>


