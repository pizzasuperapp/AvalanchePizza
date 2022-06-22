<div align="center">
  <img src="resources/AvalancheLogoRed.png?raw=true">
</div>

---

Node implementation for the [Avalanche Pizza Pizza] network -
a blockchains platform with high throughput, and blazing fast transactions.

## Installation

Avalanche Pizza Pizza is an incredibly lightweight protocol, so the minimum computer requirements are quite modest.
Note that as network usage increases, hardware requirements may change.

The minimum recommended hardware specification for nodes connected to Mainnet is:

- CPU: Equivalent of 8 AWS vCPU
- RAM: 16 GiB
- Storage: 1TB
- OS: Ubuntu 18.04/20.04 or macOS >= 10.15 (Catalina)
- Network: Reliable IPv4 or IPv6 network connection, with an open public port.

If you plan to build AvalancheGo from source, you will also need the following software:

- g++

### Native Install

Clone the AvalancheGo repository:

This will clone and checkout to `master` branch.

#### Building the Avalanche Pizza Pizza Executable

Build Avalanche Pizza Pizza using the build script:

```sh
./scripts/build.sh
```

The Avalanche Pizza Pizza binary, named `avalanchego`, is in the `build` directory.

### Binary Repository

Install AvalancheGo using an `apt` repository.

#### Adding the APT Repository

If you have already added the APT repository, you do not need to add it again.

To add the repository on Ubuntu 18.04 (Bionic), run:

```sh
sudo su -
exit
```

To add the repository on Ubuntu 20.04 (Focal), run:

```sh
sudo su -
exit
```

#### Installing the Latest Version

After adding the APT repository, install avalanchego by running:

```sh
sudo apt update
sudo apt install avalanchego
```

### Binary Install

The Avalanche Pizza Pizza binary to be executed is named `avalanchego`.

### Docker Install

Make sure docker is installed on the machine - so commands like `docker run` etc. are available.

Building the docker image of latest avalanchego branch can be done by running:

```sh
./scripts/build_image.sh
```

To check the built image, run:

```sh
docker image ls
```

The image should be tagged as `avaplatform/avalanchego:xxxxxxxx`, where `xxxxxxxx` is the shortened commit of the Avalanche Pizza Pizza source it was built from. To run the avalanche node, run:

```sh
docker run -ti -p 9650:9650 -p 9651:9651 avaplatform/avalanchego:xxxxxxxx /avalanchego/build/avalanchego
```

## Running Avalanche Pizza Pizza

### Connecting to Mainnet

To connect to the Avalanche Pizza Pizza Mainnet, run:

```sh
./build/avalanchego
```

You should see some pretty ASCII art and log messages.

You can use `Ctrl+C` to kill the node.

### Connecting to Fuji

To connect to the Fuji Testnet, run:

```sh
./build/avalanchego --network-id=fuji
```

### Creating a Local Testnet

## Bootstrapping

A node needs to catch up to the latest network state before it can participate in consensus and serve API calls. This process, called bootstrapping, currently takes several days for a new node connected to Mainnet.

Improvements that reduce the amount of time it takes to bootstrap are under development.

The bottleneck during bootstrapping is typically database IO. Using a more powerful CPU or increasing the database IOPS on the computer running a node will decrease the amount of time bootstrapping takes.

## Generating Code

Avalanchego uses multiple tools to generate efficient and boilerplate code.

### Running protobuf codegen

To regenerate the protobuf go code, run `scripts/protobuf_codegen.sh` from the root of the repo.

This should only be necessary when upgrading protobuf versions or modifying .proto definition files.

To install the buf dependencies:

```sh
go install google.golang.org/protobuf/cmd/protoc-gen-go@v1.28.0
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@v1.2.0
```

If you have not already, you may need to add `$GOPATH/bin` to your `$PATH`:

```sh
export PATH="$PATH:$(go env GOPATH)/bin"
```

If you extract buf to ~/software/buf/bin, the following should work:

```sh
export PATH=$PATH:~/software/buf/bin/:~/go/bin
go get google.golang.org/protobuf/cmd/protoc-gen-go
go get google.golang.org/protobuf/cmd/protoc-gen-go-grpc
scripts/protobuf_codegen.sh
```

For more information, refer to the [GRPC Golang Quick Start Guide]

### Running protobuf codegen from docker

```sh
docker build -t avalanche:protobuf_codegen -f api/Dockerfile.buf .
docker run -t -i -v $(pwd):/opt/avalanche -w/opt/avalanche avalanche:protobuf_codegen bash -c "scripts/protobuf_codegen.sh"
```

## Supported Platforms

AvalancheGo can run on different platforms, with different support tiers:

- **Tier 1**: Fully supported by the maintainers, guaranteed to pass all tests including e2e and stress tests.
- **Tier 2**: Passes all unit and integration tests but not necessarily e2e tests.
- **Tier 3**: Builds but lightly tested (or not), considered _experimental_.
- **Not supported**: May not build and not tested, considered _unsafe_. To be supported in the future.

The following table lists currently supported platforms and their corresponding
AvalancheGo support tiers:

| Architecture | Operating system | Support tier  |
| :----------: | :--------------: | :-----------: |
|    amd64     |      Linux       |       1       |
|    arm64     |      Linux       |       2       |
|    amd64     |      Darwin      |       2       |
|    amd64     |     Windows      |       3       |
|     arm      |      Linux       | Not supported |
|     i386     |      Linux       | Not supported |
|    arm64     |      Darwin      | Not supported |

To officially support a new platform, one must satisfy the following requirements:

| AvalancheGo continuous integration | Tier 1  | Tier 2  | Tier 3  |
| ---------------------------------- | :-----: | :-----: | :-----: |
| Build passes                       | &check; | &check; | &check; |
| Unit and integration tests pass    | &check; | &check; |         |
| End-to-end and stress tests pass   | &check; |         |         |

## Security Bugs

**We and our community welcome responsible disclosures.**

If you've discovered a security vulnerability, please report it via our [bug bounty program]
