# Metacrafter_ETH_AVAX_Proof_IntermediateEVM_Module1_ProjectAssesment

This Solidity program is a simple demonstration of error handling. The purpose of the program is to manage hotel room information while demonstrating the use of require(), assert(), and revert() for error handling.

## Description
This contract is written in the Solidity language, used for developing smart contracts on the Ethereum blockchain. The contract allows the owner to add and update hotel room information. The error handling mechanisms ensure that only valid operations are performed.

### Features
➛ Add new rooms
➛ Update room details
➛ View room details
➛ Get the total number of rooms

## Getting Started
### Executing the Program
To run this program, you can use Remix, an online Solidity IDE. To get started, go to the Remix website at [Remix](https://remix.ethereum.org/) (https://remix.ethereum.org/).

Once you are on the Remix website, create a new file by clicking on the "+" icon in the left-hand sidebar. Save the file with a .sol extension (e.g., Hotel.sol). Copy and paste the following code into the file:

```javascript
// SPDX-License-Identifier: MIT
pragma solidity 0.8.26;

contract Hotel {
    address public owner;
    uint256 public roomCount;

    struct Room {
        uint256 id;
        string name;
        uint256 price;
        bool isAvailable;
    }

    mapping(uint256 => Room) public rooms;

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can perform this action");
        _;
    }

    event RoomAdded(uint256 id, string name, uint256 price);
    event RoomUpdated(uint256 id, string name, uint256 price, bool isAvailable);

    function addRoom(uint256 _id, string memory _name, uint256 _price) public onlyOwner {
        require(_price > 0, "Price must be greater than zero");

        rooms[_id] = Room(_id, _name, _price, true);
        roomCount++;

        emit RoomAdded(_id, _name, _price);
    }

    function updateRoom(uint256 _id, string memory _name, uint256 _price, bool _isAvailable) public onlyOwner {
        require(_price > 0, "Price must be greater than zero");

        Room storage room = rooms[_id];
        if (bytes(room.name).length == 0) {
            revert("Room does not exist");
        }

        room.name = _name;
        room.price = _price;
        room.isAvailable = _isAvailable;

        emit RoomUpdated(_id, _name, _price, _isAvailable);
    }

    function getRoom(uint256 _id) public view returns (string memory, uint256, bool) {
        Room storage room = rooms[_id];
        require(bytes(room.name).length > 0, "Room does not exist");
        return (room.name, room.price, room.isAvailable);
    }

    function totalRooms() public view returns (uint256) {
        assert(roomCount >= 0);
        return roomCount;
    }
}

```
