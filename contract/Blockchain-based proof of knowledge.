// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract NFTMarketplace {
    struct NFT {
        uint id;
        address creator;
        address owner;
        string metadata;
        uint price;
        bool forSale;
    }

    uint public nftCount;
    mapping(uint => NFT) public nfts;
    mapping(address => uint[]) public userNFTs;

    event NFTCreated(uint id, address creator, string metadata);
    event NFTBought(uint id, address buyer, uint price);
    event NFTAuctionStarted(uint id, uint startingPrice);
    event NFTAuctionEnded(uint id, address highestBidder, uint highestBid);

    function createNFT(string memory metadata, uint price) public {
        nftCount++;
        nfts[nftCount] = NFT(nftCount, msg.sender, msg.sender, metadata, price, true);
        userNFTs[msg.sender].push(nftCount);
        emit NFTCreated(nftCount, msg.sender, metadata);
    }

    function buyNFT(uint id) public payable {
        NFT storage nft = nfts[id];
        require(nft.forSale, "NFT is not for sale");
        require(msg.value >= nft.price, "Insufficient funds");

        address payable seller = payable(nft.owner);
        seller.transfer(msg.value);

        nft.owner = msg.sender;
        nft.forSale = false;
        userNFTs[msg.sender].push(id);

        emit NFTBought(id, msg.sender, msg.value);
    }
}
