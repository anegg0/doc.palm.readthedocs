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

Transactions nodes do not communicate directly with the validators, transactions nodes communicate
with each other and the [bootnodes]. You must [specify the bootnodes] to connect to the Palm network

The following instructions allow you to run a Palm transaction node locally for the required
network environment. Available environments are `development`, `testing`, and `production`(Mainnet).

**Prerequisites**:

* [Hyperledger Besu installed]

* [Curl (or similar webservice client)](https://curl.haxx.se/download.html).

## 1. Create the node directory

Create the directory in which to store the required files and data directory:

```bash
mkdir palm-node
```

Change into the `palm-node` directory:

```bash
cd palm-node
```

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

The following configuration file examples include the bootnodes addresses for the required
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
    rpc-http-api=["ETH","NET","IBFT","ADMIN","WEB3"]
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
    rpc-http-api=["ETH","NET","IBFT","ADMIN","WEB3"]
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
    rpc-http-api=["ETH","NET","IBFT","ADMIN","WEB3"]
    ```

!!! note

    If running multiple Besu clients, ensure you configure the
    [`rpc-http-port`](https://besu.hyperledger.org/en/latest/Reference/CLI/CLI-Syntax/#rpc-http-port)
    option to avoid port conflicts.

## 4. Start Besu

Start Besu and specify the configuration file

```bash
besu --config-file=/home/myuser/palm_node/config.toml
```

The node attempts to connect to the bootnodes and other transaction nodes, and begins
synchronization once enough peers are found.

## 5. Confirm the network is running

Once the network is synchronized, start another terminal and use curl to call the JSON-RPC API
[`ibft_getvalidatorsbyblocknumber`](https://besu.hyperledger.org/en/latest/Reference/API-Methods/#ibft_getvalidatorsbyblocknumber)
method to check for validators on the network:

```bash
curl -X POST --data '{"jsonrpc":"2.0","method":"ibft_getValidatorsByBlockNumber","params":["latest"], "id":1}' localhost:8545
```

The result displays the validators on the Palm network:

```json
{
  "jsonrpc" : "2.0",
  "id" : 1,
  "result" : [ "0x11781ba3cd85671a6f8481514f84bce660b75919", "0x7f9a67f84a010bda3d83493e4f1476f2651b1dab", "0x88cd6a0d883f9104432d729df772131efe44b820", "0x948b655e3a1e3505c57d15f2c5c813e4abad9cb4", "0xb49ce87bcb7f8a1dde59bde1b4c18fbf00b424ac" ]
}
```
[bootnodes]: https://besu.hyperledger.org/HowTo/Find-and-Connect/Bootnodes/
[Hyperledger Besu installed]: https://besu.hyperledger.org/HowTo/Get-Started/Installation-Options/Install-Binaries/
[specify the options directly when starting Besu]: https://besu.hyperledger.org/Reference/CLI/CLI-Syntax/
[configuration file]: https://besu.hyperledger.org/HowTo/Configure/Using-Configuration-File/
[specify the bootnodes]: #3-create-the-besu-configuration-file
