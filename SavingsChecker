// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract SavingsChecker {
    address public owner;
    uint public constant taskPoints = 1;
    uint public constant threshold = 30;
    uint public constant due = 86400; // 24 hours in seconds

    struct Check {
        string date;
        string description;
        uint timestamp;
        bool completed;
    }

    mapping(address => Check[]) private check;
    mapping(address => uint) private points;

    constructor() {
        owner = msg.sender;
    }

    // date input and descrition/ amount for tracking
    function dateInput(string memory _date, string memory _description) public {
        require(bytes(_date).length > 0, "Date is required");
        require(bytes(_description).length > 0, "Description is required");
        check[msg.sender].push(Check(_date, _description, block.timestamp, false));
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    // successful input earns 1 point
    function successfulInput(uint _input) public {
        require(_input < check[msg.sender].length, "Invalid input due to timeframe");

        Check storage task = check[msg.sender][_input];
        require(!task.completed, "Input already completed");

        // Check if the task is being completed within the 24-hour window
        if (block.timestamp <= task.timestamp + due) {
            task.completed = true;
            points[msg.sender] += taskPoints; // Award points once complete
        } else {
            revert("Input completion time exceeded");
        }
    }

    // redeem points
    function redeem(uint _points) public {
        require(points[msg.sender] >= _points, "Insufficient points to redeem");

        uint previousPoints = points[msg.sender];
        points[msg.sender] -= _points;
        assert(points[msg.sender] == previousPoints - _points);
    }

    //balance
    function balance() public view returns (uint) {
        return points[msg.sender];
    }

    //list of Inputs
    function list() public view returns (Check[] memory) {
        return check[msg.sender];
    }
}
