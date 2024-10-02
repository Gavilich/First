// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract AptosBalanceChecker {
    mapping(address => uint256) public balances;
    address[] public wealthyAccounts;
    address[] public poorAccounts;

    event BalanceUpdated(address account, uint256 balance);

    // Function to update the balance for a given account
    function updateBalance(address account, uint256 balance) internal {
        balances[account] = balance;
        emit BalanceUpdated(account, balance);
    }

    // Function to check and divide balances into wealthy and poor accounts
    function checkAndDivideBalances(address[] memory accounts) external {
        delete wealthyAccounts; // Reset the lists each time this function is called
        delete poorAccounts;

        for (uint i = 0; i < accounts.length; i++) {
            uint256 balance = accounts[i].balance; // This line is incorrect; should use address(this).balance for the contract
            updateBalance(accounts[i], balance);

            // Categorize accounts based on balance
            if (balance >= 1 ether) { // Assuming 1 ether as the threshold for wealthy accounts
                wealthyAccounts.push(accounts[i]);
            } else {
                poorAccounts.push(accounts[i]);
            }
        }
    }

    // Function to get balance of an account
    function getAccountBalance(address account) external view returns (uint256) {
        return balances[account];
    }
}
