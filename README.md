# Setup Hyperledger Indy Environment

This repository explicitly explains how to setup a hyperledger indy test ledger locally and its environment for **development purpose**. These insturctions may slightly update so it is recommended to also visit [Hyperledger Indy SDK repository](https://github.com/hyperledger/indy-sdk) for more details. These intructions are strictly inteded for local machines to setup for a development environment. It is highly advised to not use these instructions for production environment deployment as it could lead to serious security issues.

Hyperledger Indy is a complex software ecosystem and it is comprised of 3 core components:
* [indy-node](https://github.com/hyperledger/indy-node)
* [indy-plenum](https://github.com/hyperledger/indy-plenum)
* [indy-sdk](https://github.com/hyperledger/indy-sdk)

*[Hyperledger Aries](https://github.com/hyperledger/aries) and [Hyperledger Ursa](https://github.com/hyperledger/ursa) are libraries that are important for developing with hyperledger indy and will be used in later phase. They are contexually out of scope for this particular tutorial.*


### Indy Node
Indy Node is a server portion of a distributed ledger purpose-built for decentralized identity. It embodies all the functionality to run nodes (validators and/or observers) that provide a [self-sovereign identity](https://sovrin.org/) ecosystem on top of a distributed ledger.

### Indy Plenum
Indy Plenum implements the [Plenum Byzantine Fault Tolerant Protocol](http://pakupaku.me/plaublin/rbft/5000a297.pdf). Plenum is the heart of the distributed ledger technology inside Hyperledger Indy. As such, it provides features somewhat similar in scope to those found in Hyperledger Fabric. However, it is special-purposed for use in an identity system, whereas Fabric is general purpose.

### Indy SDK
It is the official SDK for [Hyperledger Indy](https://www.hyperledger.org/projects/hyperledger-indy), which provides a distributed-ledger-based foundation for [self-sovereign identity](https://sovrin.org/). Indy provides a software ecosystem for private, secure, and powerful identity, and the Indy SDK enables clients for it. The major artifact of the SDK is a C-callable library; there are also convenience wrappers for various programming languages and Indy CLI tool.


# Development Environment

## MacOS

## Windows
