---
description: >-
  This section outlines the step-by-step process found on prylabs.net to submit
  a deposit and initialise a validator for participation in the testnet.
---

# Activating a Validator

### Downloading Prysm

To begin, follow the instructions found in the [installation section](./#installing-prysm) of the [Getting Started](./) guide to fetch and install Prysm with either Docker or Bazel.

### Receiving Göerli ETH

On step 2, you will be asked to link a wallet address to your validator with either the [Metamask](https://metamask.io/) browser extension \(recommended\) or [Portis](https://portis.io). Select your preferred platform and click through the steps presented.

![](.gitbook/assets/2%20%281%29.png)

The wallet is scanned for the required amount of Göerli ETH after being linked. If the wallet does not have sufficient funds, you will be given the option to receive the required GöETH from our faucet.

### Generating a validator keypair

Step 3 requires running a command to generate a public / private keypair for your validator, as well as deposit data to submit on the prylabs.net page. Depending on your platform, issue the appropriate command from the examples below..

#### Generating with Docker on GNU/Linux or macOS

To generate a keypair via Docker on UNIX, issue the command:

```bash
docker run -it -v /usr/local/prysm/validator:/data \
   gcr.io/prysmaticlabs/prysm/validator:latest \
   accounts create --keystore-path=/data --password=changeme
```

#### Generating with Docker on Windows

To generate a keypair via Docker on Windows, issue the command:

```text
docker run -it -v /tmp/prysm-data:/data gcr.io/prysmaticlabs/prysm/validator:latest accounts create --keystore-path=/data --password=changeme
```

#### Generating with Bazel

To generate a keypair via Bazel, issue the command:

```text
bazel run //validator -- accounts create --keystore-path=$HOME/beacon-chain --password=changeme
```

This command will output a `Raw Transaction Data` block:

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LRNnKRqTm4z1mzdDqDF%2F-LuJpxGKxOpat8TfDxPP%2F-Lua3OVmMOefnzXXvdGq%2F4.png?alt=media&token=96459a93-055c-4bf1-a0af-07a900d7b47f)

Copy and paste the deposit data into the input field on prylabs.net:

![](.gitbook/assets/77.png)

### Starting up the beacon node

{% hint style="info" %}
If you have already started and syncronised your beacon node by following the [Getting Started](./#connecting-to-the-testnet-running-a-beacon-node) guide section on the topic, this portion can be skipped.
{% endhint %}

Open a terminal window. Depending on your platform, issue the appropriate command from the examples below to start the beacon node.

#### Starting the beacon node with Docker on GNU/Linux or macOS

```text
docker run -it -v /usr/local/prysm/validator:/data --network="host" \
  gcr.io/prysmaticlabs/prysm/validator:latest \
  --beacon-rpc-provider=127.0.0.1:4000 \
  --keystore-path=/data \
  --password=changeme
```

#### Starting the beacon node with Docker on WIndows

```text
docker run -it -v /tmp/prysm-data:/data gcr.io/prysmaticlabs/prysm/validator:latest accounts create --keystore-path=/data --password=changeme
```

#### Starting the beacon node with Bazel

```text
bazel run //beacon-chain -- --datadir=$HOME/beacon-chain --init-sync-no-verify
```

The beacon node will spin up and immediately begin communicating with the Prysm testnet, outputting data similar to the image below.

![](.gitbook/assets/9.png)

The process of syncronising may take a while; the incoming block per second capacity is dependent upon the connection strength, network congestion and overall peer count. 

### Starting up the validator client

{% hint style="danger" %}
The beacon node must be **completely synced** before attempting to initialise a validator client, otherwise the validator will not be able to complete the deposit and **funds will lost**.
{% endhint %}

Open a second terminal window. Depending on your platform, issue the appropriate command from the examples below to start the validator.

#### Starting the validator client with Docker on GNU/Linux or macOS

```text
docker run -it -v /usr/local/prysm/validator:/data --network="host" \
  gcr.io/prysmaticlabs/prysm/validator:latest \
  --beacon-rpc-provider=127.0.0.1:4000 \
  --keystore-path=/data \
  --password=changeme
```

#### Starting the validator client with Docker on Windows

```text
docker run -it -v /tmp/prysm-data:/data --network="host" gcr.io/prysmaticlabs/prysm/validator:latest --beacon-rpc-provider=127.0.0.1:4000 --keystore-path=/data --password=changeme
```

#### Starting the validator client with Bazel

```text
bazel run //validator -- --keystore-path=$HOME/beacon-chain --password=changeme
```

### Submitting a deposit contract

Once both the beacon node and validator client are successfully running, make your deposit by clicking the button and following the steps presented in your wallet.

![](.gitbook/assets/5.png)

It will take a while for the nodes in the network to process a deposit. Once the node is active, the validator will immediately begin performing its responsibilities.

The validator is now awaiting its first assignment from the network. This should only take a few minutes, and an indication will be displayed on both the prylabs.net page as well as the validator client.

**Congratulations, you are now fully participating in the Prysm testnet!** ♡

_Still have questions?_ Stop by our [Discord](https://discord.gg/KSA7rPr) for assistance!
