# ROK Coin (ERC20 Token)

ROK Coin is a fungible token created using the ERC20 standard on the Ethereum-compatible Fuji Testnet blockchain. This project demonstrates the creation and deployment of an ERC20 token using Remix IDE.

---

## Overview

- **Token Name**: ROK Coin  
- **Symbol**: ROK  
- **Blockchain**: Fuji Testnet (Chain ID: 43113)  
- **Contract Address**: `0x8F210689F8fa9849B14BF7B9780Ed625327197eb`  
- **Total Supply**: 1,000.00 ROK  
- **Decimals**: 2  
- **Holders**: 1  

---
## Transaction Screenshot

Below is a screenshot of the first transaction made with the ROK Coin:

![Transaction Screenshot](https://github.com/RochaEverton/ROK-COIN/blob/main/Images/Fuji%20Explorer.PNG)

---
## Features

1. **ERC20 Compliance**: Adheres to the ERC20 token standard.
2. **Decimals**: Supports two decimal places for easier calculations.
3. **Initial Supply**: 1,000 ROK tokens minted at deployment.

---

## Tools and Environment

- **IDE**: [Remix IDE](https://remix.ethereum.org/)
- **Blockchain**: Avalanche Fuji Testnet
- **Programming Language**: Solidity
- **Explorer**: Fuji Explorer

---

## Deployment Details

The transaction hash of the deployment is `0xdc54f...31c7e`, with an initial transfer of 1,000 ROK tokens to the address `0x40830...fa72b5`. The screenshot above reflects the first and only transaction as of now.

---

## Prerequisites

To interact with this token, you need:

1. **Metamask**: Configured to connect to the Avalanche Fuji Testnet.
2. **ETH Test Tokens**: Required for gas fees (obtainable from a Fuji testnet faucet).
3. **Remix IDE**: To view or modify the token code.

---

## How to Use

### 1. Add ROK Coin to Your Wallet

1. Open your wallet (e.g., Metamask).
2. Navigate to "Import Tokens."
3. Use the contract address `0x8F210689F8fa9849B14BF7B9780Ed625327197eb`.
4. Confirm to add ROK Coin.

### 2. View Transactions

Visit the [Fuji Explorer](https://explorer.testnet.avax.network/) and search for the contract address or the transaction hash.

---

## Token Contract Code

The ROK Coin's source code follows the ERC20 standard. Below is the full implementation used for the creation of this token:

```solidity
pragma solidity ^0.4.24;
 
//Safe Math Interface
 
contract SafeMath {
 
    function safeAdd(uint a, uint b) public pure returns (uint c) {
        c = a + b;
        require(c >= a);
    }
 
    function safeSub(uint a, uint b) public pure returns (uint c) {
        require(b <= a);
        c = a - b;
    }
 
    function safeMul(uint a, uint b) public pure returns (uint c) {
        c = a * b;
        require(a == 0 || c / a == b);
    }
 
    function safeDiv(uint a, uint b) public pure returns (uint c) {
        require(b > 0);
        c = a / b;
    }
}
 
 
//ERC Token Standard #20 Interface
 
contract ERC20Interface {
    function totalSupply() public constant returns (uint);
    function balanceOf(address tokenOwner) public constant returns (uint balance);
    function allowance(address tokenOwner, address spender) public constant returns (uint remaining);
    function transfer(address to, uint tokens) public returns (bool success);
    function approve(address spender, uint tokens) public returns (bool success);
    function transferFrom(address from, address to, uint tokens) public returns (bool success);
 
    event Transfer(address indexed from, address indexed to, uint tokens);
    event Approval(address indexed tokenOwner, address indexed spender, uint tokens);
}
 
 
//Contract function to receive approval and execute function in one call
 
contract ApproveAndCallFallBack {
    function receiveApproval(address from, uint256 tokens, address token, bytes data) public;
}
 
//Actual token contract
 
contract ROCHAToken is ERC20Interface, SafeMath {
    string public symbol;
    string public  name;
    uint8 public decimals;
    uint public _totalSupply;
 
    mapping(address => uint) balances;
    mapping(address => mapping(address => uint)) allowed;
 
    constructor() public {
        symbol = "ROK";
        name = "ROK Coin";
        decimals = 2;
        _totalSupply = 100000;
        balances[0x4083098a1cEbeA299c3895cdfe73a05A77fa72b5] = _totalSupply;
        emit Transfer(address(0), 0x4083098a1cEbeA299c3895cdfe73a05A77fa72b5, _totalSupply);
    }
 
    function totalSupply() public constant returns (uint) {
        return _totalSupply  - balances[address(0)];
    }
 
    function balanceOf(address tokenOwner) public constant returns (uint balance) {
        return balances[tokenOwner];
    }
 
    function transfer(address to, uint tokens) public returns (bool success) {
        balances[msg.sender] = safeSub(balances[msg.sender], tokens);
        balances[to] = safeAdd(balances[to], tokens);
        emit Transfer(msg.sender, to, tokens);
        return true;
    }
 
    function approve(address spender, uint tokens) public returns (bool success) {
        allowed[msg.sender][spender] = tokens;
        emit Approval(msg.sender, spender, tokens);
        return true;
    }
 
    function transferFrom(address from, address to, uint tokens) public returns (bool success) {
        balances[from] = safeSub(balances[from], tokens);
        allowed[from][msg.sender] = safeSub(allowed[from][msg.sender], tokens);
        balances[to] = safeAdd(balances[to], tokens);
        emit Transfer(from, to, tokens);
        return true;
    }
 
    function allowance(address tokenOwner, address spender) public constant returns (uint remaining) {
        return allowed[tokenOwner][spender];
    }
 
    function approveAndCall(address spender, uint tokens, bytes data) public returns (bool success) {
        allowed[msg.sender][spender] = tokens;
        emit Approval(msg.sender, spender, tokens);
        ApproveAndCallFallBack(spender).receiveApproval(msg.sender, tokens, this, data);
        return true;
    }
 
    function () public payable {
        revert();
    }
} 
```

---

## License

This project is licensed under the MIT License. Feel free to use and modify as needed.

---

## Acknowledgements

- **Remix IDE**: Simplifying Ethereum smart contract development.
- **OpenZeppelin**: Providing secure and reusable Solidity libraries.
- **Avalanche Fuji Testnet**: Offering an Ethereum-compatible test environment.

---

Feel free to contribute to this project or share your feedback!

