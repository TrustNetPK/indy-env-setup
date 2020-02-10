# Setup Hyperledger Indy

This repository explicitly explains how to setup a hyperledger indy test ledger locally for development purpose. These insturctions may slightly update so it is recommended to also visit [Hyperledger Indy SDK repository](https://github.com/hyperledger/indy-sdk) for more details. This deployment is only inteded for local machines to setup for development and testing. It is highly advised to not use these instructions for production ledger deployment as it could lead to serious security issues.

Hyperledger Indy Ledger is a complex piece of software and it is comprised of two main components:
* [indy-node](https://github.com/hyperledger/indy-node)
* [indy-plenum](https://github.com/hyperledger/indy-plenum)

### Indy Node
Where indy-node is a server portion of a distributed ledger purpose-built for decentralized identity. It embodies all the functionality to run nodes (validators and/or observers) that provide a [self-sovereign identity](https://sovrin.org/) ecosystem on top of a distributed ledger.

### Indy Plenum
indy-plenum implements the [Plenum Byzantine Fault Tolerant Protocol](http://pakupaku.me/plaublin/rbft/5000a297.pdf). Plenum is the heart of the distributed ledger technology inside Hyperledger Indy. As such, it provides features somewhat similar in scope to those found in Fabric. However, it is special-purposed for use in an identity system, whereas Fabric is general purpose.

# MacOS

# Windows
