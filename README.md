# Building-on-Avalanche: Degen Token(ERC-20)
This Solidity smart contract creates a custom ERC20 token and deploy it on the Avalanche network for Degen
Gaming. The following functionality: Minting, Transffering, Redeeming, Checking balance, and Burning tokens. 

## Description
Good day! My name is MARC and Im here again for another poroject assessment in Metacrafters. This is my code and I will explain my code
and share how to Create ERC-20 token and Deployit on the Avalanche network for Degen Gaming.
## Getting Started
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

Your task is to create a ERC20 token and deploy it on the Avalanche network for Degen Gaming. The smart contract should have the following functionality:

Minting new tokens: The platform should be able to create new tokens and distribute them to players as rewards. Only the owner can mint tokens.
Transferring tokens: Players should be able to transfer their tokens to others.
Redeeming tokens: Players should be able to redeem their tokens for items in the in-game store.
Checking token balance: Players should be able to check their token balance at any time.
Burning tokens: Anyone should be able to burn tokens, that they own, that are no longer needed.

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract MyDegenToken is ERC20 {
    struct Item {
        uint256 value;
        bool exists;
    }

    mapping(uint256 => Item) public items;

    address public owner;

    event ItemRedeemed(address indexed player, uint256 indexed itemId, uint256 amount);
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor(string memory name, string memory symbol) ERC20(name, symbol) {
        owner = msg.sender;
        addItem(1, 250); // item 1: Love Piece - Value = 250 DegenTokens
        addItem(2, 450); // item 1: Valkyrie Drop - Value = 450 DegenTokens
        addItem(3, 650); // item 1: Toy Ring - Value = 650 DegenTokens
        addItem(4, 850); // item 1: Tempest Shadow Weapon Box - Value = 850 DegenTokens
        addItem(5, 1050); // item 1: Costume Lord of Royals - Value = 1050 DegenTokens
        addItem(6, 1250); // item 1: Poison Gold Bottle Box x2 - Value = 1250 DegenTokens
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Ownable: caller is not the owner");
        _;
    }

    function addItem(uint256 itemId, uint256 value) internal {
        require(!items[itemId].exists, "Item already exists");
        items[itemId] = Item(value, true);
    }

    function mint(address account, uint256 amount) external onlyOwner {
        _mint(account, amount);
    }

    function burn(uint256 amount) external {
        _burn(msg.sender, amount);
    }

    function redeemItem(uint256 itemId, uint256 amount) external {
        require(items[itemId].exists, "Invalid item id");
        require(balanceOf(msg.sender) >= items[itemId].value * amount, "Insufficient balance");

        _burn(msg.sender, items[itemId].value * amount);
        emit ItemRedeemed(msg.sender, itemId, amount);
    }

    function redeemTokens(uint256 amount) external {
        require(balanceOf(msg.sender) >= amount, "Insufficient balance");

        _burn(msg.sender, amount);
        emit ItemRedeemed(msg.sender, 0, amount); // ItemId 0 represents redeeming tokens
    }
}


   
        
## Executing Program
To run this program, you can use Remix, an online Solidity IDE. To get started, go to the Remix website at https://remix.ethereum.org/. 
Once you are on the Remix website, create a new file by clicking on the "+" icon in the left-hand sidebar. Save the file with a .sol extension 
(e.g., HelloWorld.sol). Then copy the code given in the assessment.

## Author
Marc Danniel Mariazeta 61902476@ntc.edu.ph
