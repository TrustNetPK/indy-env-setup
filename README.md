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

1. Install Rust and rustup (https://www.rust-lang.org/install.html).
2. Install required native libraries and utilities (libsodium is added with URL to homebrew since version<1.0.15 is required)
   ```
   brew install pkg-config
   brew install https://raw.githubusercontent.com/Homebrew/homebrew-core/65effd2b617bade68a8a2c5b39e1c3089cc0e945/Formula/libsodium.rb   
   brew install automake 
   brew install autoconf
   brew install cmake
   brew install openssl
   brew install zeromq
   brew install zmq
   ```
3. Setup environment variables:
   ```
   export PKG_CONFIG_ALLOW_CROSS=1
   export CARGO_INCREMENTAL=1
   export RUST_LOG=indy=trace
   export RUST_TEST_THREADS=1
   ```
4. Setup OPENSSL_DIR variable: path to installed openssl library
   ```
   for version in `ls -t /usr/local/Cellar/openssl/`; do
        export OPENSSL_DIR=/usr/local/Cellar/openssl/$version
        break
   done
   ```
5. Checkout and build the library:
   ```
   git clone https://github.com/hyperledger/indy-sdk.git
   cd ./indy-sdk/libindy
   cargo build
   ```
6. To compile the CLI, libnullpay, or other items that depend on libindy:
   ```
   export LIBRARY_PATH=/path/to/sdk/libindy/target/<config>
   cd ../cli
   cargo build
   ```
7. Set your `DYLD_LIBRARY_PATH` and `LD_LIBRARY_PATH` environment variables to the path of `indy-sdk/libindy/target/debug`. You may want to put these in your `.bash_profile` to persist them.

## Note on running local nodes

In the cloned 'indy-sdk' folder from (https://github.com/hyperledger/indy-sdk.git) you will see a 'ci' folder, Start the pool of local nodes on 127.0.0.1:9701-9708 with Docker by running:

```
docker build -f ci/indy-pool.dockerfile -t indy_pool .
docker run -itd -p 9701-9708:9701-9708 indy_pool
```
#### Automated Build Alternative
You can also try automated build by cloning the (https://github.com/hyperledger/indy-sdk.git) repo and run `mac.build.sh` in the `libindy` folder.

## Windows

Comming soon...
