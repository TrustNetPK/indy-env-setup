# Setup Hyperledger Indy Environment
![Alt text](/indy-logo.png?raw=true "Hyperledger Indy")

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

### Note on running local nodes

In the cloned 'indy-sdk' folder from (https://github.com/hyperledger/indy-sdk.git) you will see a 'ci' folder, Start the pool of local nodes on 127.0.0.1:9701-9708 with Docker by running:

```
docker build -f ci/indy-pool.dockerfile -t indy_pool .
docker run -itd -p 9701-9708:9701-9708 indy_pool
```
#### Automated Build Alternative
You can also try automated build by cloning the (https://github.com/hyperledger/indy-sdk.git) repo and run `mac.build.sh` in the `libindy` folder.

## Linux (Ubuntu 16.04)

1. Install Rust and rustup (https://www.rust-lang.org/install.html).
1. Install required native libraries and utilities:

   ```
   apt-get update && \
   apt-get install -y \
      build-essential \
      pkg-config \
      cmake \
      libssl-dev \
      libsqlite3-dev \
      libzmq3-dev \
      libncursesw5-dev
   ```
   
1. `libindy` requires the modern `1.0.14` version of `libsodium` but Ubuntu 16.04 does not support installation it's from `apt` repository.
 Because of this, it requires to build and install `libsodium` from source:
 ```
cd /tmp && \
   curl https://download.libsodium.org/libsodium/releases/old/unsupported/libsodium-1.0.14.tar.gz | tar -xz && \
    cd /tmp/libsodium-1.0.14 && \
    ./configure --disable-shared && \
    make && \
    make install && \
    rm -rf /tmp/libsodium-1.0.14
```

4. Build `libindy`

   ```
   git clone https://github.com/hyperledger/indy-sdk.git
   cd ./indy-sdk/libindy
   cargo build
   cd ..
   ```
   
**Note:** `libindy` debian package, installed from the apt repository, is statically linked with `libsodium`. 
For manually building this can be achieved by passing `--features sodium_static` into `cargo build` command.
   
 
### Note on running local nodes

In the cloned 'indy-sdk' folder from (https://github.com/hyperledger/indy-sdk.git) you will see a 'ci' folder, Start the pool of local nodes on 127.0.0.1:9701-9708 with Docker by running:

```
docker build -f ci/indy-pool.dockerfile -t indy_pool .
docker run -itd -p 9701-9708:9701-9708 indy_pool
```

## Windows

### Build Environment

1. Setup a windows virtual machine. Free images are available at [here](https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/)
1. Launch the virtual machine 
1. Download Visual Studio Community Edition 2017 (these instructions also work with Visual Studio Professional 2017)
1. Check the boxes for the _Desktop development with C++_ and _Linux Development with C++_
1. In the summary portion on the right hand side also check _C++/CLI support_
1. Click install
1. Download git-scm for windows [here](https://git-scm.com/download/win)
1. Install git for windows using:
   1. _Use Git from Git Bash Only_ so it doesn't change any path settings of the command prompt
   1. _Checkout as is, commit Unix-style line endings_. You shouldn't be commiting anything anyway but just in case
   1. _Use MinTTY_
   1. Check all the boxes for:
      1. Enable file system caching
      1. Enable Git Credential Manager
      1. Enable symbolic links
1. Download rust for windows [here](https://www.rust-lang.org/en-US/install.html)
   1. Choose installation option *1*

### Get/build dependencies

- Open a the Git Bash command prompt
- Change directories to Downloads:
```bash
cd Downloads
```

- Clone the _indy-sdk_ repository from github.
```bash
git clone https://github.com/hyperledger/indy-sdk.git
```

- Download the prebuilt dependencies [here](https://repo.sovrin.org/windows/libindy/deps/)
- Extract them into the folder _C:\BIN\x64_
> It really doesn't matter where you put these as long as you remember where so you can set
> the environment variables to this path

- If you are not building dependencies from source you may skip to *Build*

### Binary deps

- https://www.npcglib.org/~stathis/downloads/openssl-1.0.2k-vs2017.7z
- https://download.libsodium.org/libsodium/releases/old/libsodium-1.0.14-msvc.zip

### Source deps

- http://www.sqlite.org/2017/sqlite-amalgamation-3180000.zip
- https://github.com/zeromq/libzmq

### Build sqlite

Download http://www.sqlite.org/2017/sqlite-amalgamation-3180000.zip

Create an empty static library project in Visual Studio and add `sqlite.c` file and 2 headers from extracted
archive. Then just build it.

### Build libzmq

Follow to http://zeromq.org/intro.
- Download sources from last stable release for Windows. 
- Open `zeromq-x.x.x/builds/msvc/vs2015/libzmq.sln` with Visual Studio
- If necessary change solution platforms on x64(if you are working on x64 arch).
- On main menu bar choose build->build libzmq.
- If build project was successful, two files `libzmq.dll` and `libzmq.lib` should appear 
  in path `zeromq-x.x.x/bin/x64/Debug/vXXX/dynamic`.
- rename `libzmq.lib` to `zmq.lib`.

### Build

- Get binary dependencies (libamcl*, openssl, libsodium, libzmq, sqlite3).
- Put all *.{lib,dll} into one directory and headers into include/ subdirectory.
- Open a windows command prompt
- Configure MSVS environment to privide 64-bit builds by execution of `vcvars64.bat`:
  
  ```
  "C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Auxiliary\Build\"vcvars64.bat
  ```
  
  Note that depending on the version of Visual Studio placement of vcvars64.bat can be different. For example, it can be
  `"C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\bin\amd64\vcvars64.bat"`  
- Execute `"C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Auxiliary\Build\vcvars64.bat"`
- Point path to this directory using environment variables:
  - `set INDY_PREBUILT_DEPS_DIR=C:\BIN\x64`
  - `set INDY_CRYPTO_PREBUILT_DEPS_DIR=C:\BIN\x64`
  - `set MILAGRO_DIR=C:\BIN\x64`
  - `set LIBZMQ_PREFIX=C:\BIN\x64`
  - `set SODIUM_LIB_DIR=C:\BIN\x64`
  - `set OPENSSL_DIR=C:\BIN\x64`
- Set PATH to find .dlls:
  - `set PATH=C:\BIN\x64\lib;%PATH%`
- change dir to `indy-sdk/libindy` and run `cargo build` (you may want to add `--release --target x86_64-pc-windows-msvc`
  keys to cargo)

### openssl-sys workaround

If your windows build fails complaining on gdi32.lib you should edit

```
  ~/.cargo/registry/src/github.com-*/openssl-sys-*/build.rs
```

and add

```
  println!("cargo:rustc-link-lib=dylib=gdi32");
```

to the end of `main()` function.

Then try to rebuild whole project.

### Run integration tests

* Start local nodes pool on `127.0.0.1:9701-9708` with Docker:
 
  ```     
  docker build -f ci/indy-pool.dockerfile -t indy_pool .
  docker run -itd -p 9701-9709:9701-9709 indy_pool
  ```          
 
  Please note that this port mapping between container and local host requires
  latest Docker for Windows (linux containers) and windows system with Hyper-V support.
  
  If you use some Docker distribution based on Virtual Box you can use Virtual Box's 
  port forwarding future to map 9701-9709 container ports to local 9701-9709 ports.
 
* Run tests
  
  ```
  RUST_TEST_THREADS=1 cargo test
  ```
