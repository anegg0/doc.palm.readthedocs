---
Description: Run a transaction node.
---

# Run a transaction node

Transaction nodes are used to read transaction history or send transactions on the Palm network
without performing validator duties.

!!! note

    In IBFT 2.0 networks, validators validate transactions and blocks. Validators take turns to
    create the next block. Before inserting the block onto the chain, a super-majority
    (greater than 66%) of validators must first sign the block.

To ensure that your node can successfully join the Palm network, you must specify the
[correct bootnodes] for the environment you are targeting. This allows your node to find
existing nodes on the network. Connecting to existing nodes is important as it allows your node
to download the chain history and send transactions to validator nodes.

The instructions in this section allow you to run a Palm transaction node locally for the target
network environment. Available network environments are:

| Network     | Description                                                         |
|-------------|---------------------------------------------------------------------|
| Development | Useful for quick testing during development. This network may reset periodically, meaning data may not persist. |
| Production  | Palm's Mainnet environment. Contracts and accounts hold real value and assets can be bridged across to Ethereum's Mainnet. |
| Testing     | A persistent environment that will not be reset. This is useful as a staging environment and for long-term testing. A bridge exists on the Rinkeby testnet for transferring assets to and from this environment. |

**Prerequisites**:

* [Hyperledger Besu installed]

* [Curl (or similar web service client)](https://curl.haxx.se/download.html).

## 1. Create the node directory

Create the directory in which to store the required files and data directory:

```bash
mkdir palm-node
```

Change into the `palm-node` directory:

```bash
cd palm-node
```

!!! note

    If deploying your node on a cloud service like AWS, we recommend you use a second large data
    volume for the data directory. In the event of host failure, the data volume can be quickly
    moved to another host to re-establish connectivity with minimal downtime.

## 2. Download the genesis file

The genesis file specifies the network-wide settings and defines the first block in the chain. Each
Palm environment uses a different genesis file.

The following curl commands download the genesis file for the required environment.

=== "Production"

    ```bash
    curl -O https://genesis-files.palm.io/prd/genesis.json
    ```

=== "Development"

    ```bash
    curl -O https://genesis-files.palm.io/dev/genesis.json
    ```

=== "Testing"

    ```bash
    curl -O https://genesis-files.palm.io/uat/genesis.json
    ```

## 3. Create the Besu configuration file

The [configuration file] is a TOML file used to specify the Besu options. Alternatively,
[specify the options directly when starting Besu].

The following configuration file examples include the [bootnode addresses] for the required
environments. Create a TOML file named `config.toml` with the following options:

!!! important

    For the `data-path`option, replace `<PATH>` with the location of the node directory created in
    [step 1](#1-create-the-node-directory).

=== "Production"

    ```toml
    # Production network genesis file
    genesis-file="genesis.json"

    # Network bootnodes
    bootnodes=["enode://9cccbaae702d477c5fd4d704a2d6f92a90005f62de980b11b0d042877bf759774cf7d68d358c59427622e87538bc46afa1195d6ac12cb153d6771461c1830d1b@54.243.108.56:30303","enode://d6518f4f318a172158cf73c3e615c4eb488efb14c20b4a2f13570bf01092573222cd6935599a80017512457fb7f229cf6562f9d038b5d0dc98db95074d4a98b3@18.235.247.31:30303"]

    # Data directory
    data-path="<PATH>/palm-node"

    #Enable the JSON-RPCs
    rpc-http-enabled=true
    ```

=== "Development"

    ```toml
    # Development network genesis file
    genesis-file="genesis.json"

    # Network bootnodes
    bootnodes=["enode://6a21d1fe0e283412e65a3dd3f4cdc63879eec0a939c4a6dff7dba90cd368ce89dcd732909c2f64d26267f85a56c43627f11742ef88feb712595362e1590ed077@18.205.172.1:30303","enode://7992a25ead5579feb1573aca85d17e49c7cb84d4391a7ac59430eecb9e24bb76a57d749b886e47e19ea8cec7ffbda326d8778434f418edda4ca1950937b3df34@52.2.6.154:30303"]

    # Data directory
    data-path="<PATH>/palm-node"

    #Enable the JSON-RPCs
    rpc-http-enabled=true
    ```

=== "Testing"

    ```toml
    # Testing network genesis file
    genesis-file="genesis.json"

    # Network bootnodes
    bootnodes=["enode://7c6e935eca89b230002294420c10d645844419ac50c5fc03fa53bf24fd82600508f5a4d5b89f7690c7e8f9c5dc833605d60bb1dd35997669ab7f1fc274683803@54.162.14.76:30303","enode://2f5d0489e2bbbc495e3d38ae3df9cc0a47faf42818057d193f0f4863d44505277c3d1b9a863f7ad961830ef15a8f8b72ec52791f3cca5ef84284a29f82f2dd73@18.235.20.166:30303"]

    # Data directory
    data-path="<PATH>/palm-node"

    #Enable the JSON-RPCs
    rpc-http-enabled=true
    ```

!!! note

    If running multiple Besu clients, ensure you configure the
    [`rpc-http-port`](https://besu.hyperledger.org/en/latest/Reference/CLI/CLI-Syntax/#rpc-http-port)
    option to avoid port conflicts.

## 4. Start Besu

Start Besu and specify the configuration file:

```bash
besu --config-file=/home/myuser/palm_node/config.toml
```

The node attempts to connect to the bootnodes and other transaction nodes, and begins
synchronization once enough peers are found.

## 5. Confirm the network is running

Once the network is synchronized, start another terminal and use curl to call the JSON-RPC API
[`net_peerCount`](https://besu.hyperledger.org/en/latest/Reference/API-Methods/#net_peerCount)
method to check for connected peers::

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"net_peerCount","params":[], "id":1}' localhost:8545
```

The result displays the validators on the Palm network:

```json
{
  "jsonrpc" : "2.0",
  "id" : 1,
  "result" : [ "0x5" ]
}
```

[bootnode addresses]: https://besu.hyperledger.org/HowTo/Find-and-Connect/Bootnodes/
[Hyperledger Besu installed]: https://besu.hyperledger.org/HowTo/Get-Started/Installation-Options/Install-Binaries/
[specify the options directly when starting Besu]: https://besu.hyperledger.org/Reference/CLI/CLI-Syntax/
[configuration file]: https://besu.hyperledger.org/HowTo/Configure/Using-Configuration-File/
[correct bootnodes]: #3-create-the-besu-configuration-file
