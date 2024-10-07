# Zestcoin
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract ZestCoin {
    string public name = "ZestCoin"; // Name of the token
    string public symbol = "ZEST"; // Symbol of the token
    uint8 public decimals = 18; // Decimal places
    uint256 public totalSupply; // Total supply of tokens

    mapping(address => uint256) public balanceOf; // Balances of each address
    mapping(address => mapping(address => uint256)) public allowance; // Allowances for token transfers

    event Transfer(address indexed from, address indexed to, uint256 value); // Transfer event
    event Approval(address indexed owner, address indexed spender, uint256 value); // Approval event

    constructor(uint256 _initialSupply) {
        totalSupply = _initialSupply * (10 ** uint256(decimals)); // Setting total supply
        balanceOf[msg.sender] = totalSupply; // Assigning the entire supply to the contract creator
    }

    function transfer(address _to, uint256 _value) public returns (bool success) {
        require(_to != address(0), "Invalid address"); // Check for zero address
        require(balanceOf[msg.sender] >= _value, "Insufficient balance"); // Check sender's balance

        balanceOf[msg.sender] -= _value; // Deducting balance
        balanceOf[_to] += _value; // Adding balance to the recipient
        emit Transfer(msg.sender, _to, _value); // Emitting transfer event

        return true; // Returning success
    }

    function approve(address _spender, uint256 _value) public returns (bool success) {
        allowance[msg.sender][_spender] = _value; // Setting allowance
        emit Approval(msg.sender, _spender, _value); // Emitting approval event

        return true; // Returning success
    }

    function transferFrom(address _from, address _to, uint256 _value) public returns (bool success) {
        require(_from != address(0), "Invalid address"); // Check for zero address
        require(_to != address(0), "Invalid address"); // Check for zero address
        require(balanceOf[_from] >= _value, "Insufficient balance"); // Check sender's balance
        require(allowance[_from][msg.sender] >= _value, "Allowance exceeded"); // Check allowance

        balanceOf[_from] -= _value; // Deducting balance
        balanceOf[_to] += _value; // Adding balance to the recipient
        allowance[_from][msg.sender] -= _value; // Updating allowance
        emit Transfer(_from, _to, _value); // Emitting transfer event

        return true; // Returning success
    }
}
