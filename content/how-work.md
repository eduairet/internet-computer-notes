# How does it Work?

<hr/>

## Architecture

-   **Architecture of the Internet Computer**

    -   The Internet Computer (IC) is a scalable and secure blockchain network functioning as a World Computer.
    -   Smart contracts, called canisters, are hosted on subnets— independent blockchains running on globally-distributed nodes.
    -   Canisters communicate asynchronously, allowing for seamless scaling by adding more subnets.
    -   The IC utilizes chain-key cryptography for decentralized operation and has a tokenized DAO called the Network Nervous System (NNS) for governance.
    -   The IC was launched and open-sourced in May 2021 by the DFINITY Foundation, and it is now controlled by ICP token holders with ongoing support from DFINITY.

<hr/>

## Core IC Protocol:

-   Internet Computer is powered by the Internet Computer Protocol (ICP) and utilizes the ICP token
-   Subnets process messages for scalability
-   Core IC protocol consists of four layers: peer-to-peer, consensus, message routing, and execution

    -   **Peer-to-peer:**

        -   Enables secure communication between subnet nodes
        -   Establishes a virtual broadcast network using IP connectivity
        -   Nodes can broadcast artifacts to all subnet nodes

    -   **Consensus:**

        -   Ensures agreement and ordering of messages within a subnet
        -   Each subnet runs the IC core protocol independently
        -   Provides deterministic execution of messages for state transitions

    -   **Message routing:**

        -   Receives blocks of messages from consensus
        -   Places messages in target canisters' input queues and routes output messages
        -   Enables inter-canister communication and state certification
        -   State certification and secure XNet messaging enhance security and scalability

    -   **Execution:**

        -   Topmost layer of the core IC protocol stack
        -   Executes canister smart contract code using a WebAssembly (Wasm) virtual machine
        -   Wasm bytecode executed deterministically and with near-native speed
        -   Canister messages inducted into queues before execution
        -   Features deterministic time slicing, concurrency, and a pseudorandom number generator

<hr/>

## Chain-key technology:

-   **Chain-key cryptography:**

    -   Internet Computer protocol uses advanced chain-key cryptography.
    -   Enables unique functionalities and scalability.
    -   Threshold signature scheme distributes secret signing key among replicas.
    -   Benefits include easy content verification, autonomous network evolution, and secure pseudo-random number generation.

-   **Chain-key signatures:**

    -   Extends chain-key technology for on-chain computations targeting other blockchains.
    -   Trustless integration with blockchains like Bitcoin and Ethereum.
    -   Canisters securely store and transact Bitcoin using shared secret keys.
    -   Transactions require agreement from at least 2/3 of nodes.

-   **Bitcoin integration:**

    -   Relies on chain-key signatures and direct interaction with the Bitcoin network.
    -   Canisters have Bitcoin addresses and create valid transactions.
    -   Direct message exchange maintains Bitcoin blockchain state and transmits transactions.

-   **Chain-key tokens:**

    -   Decentralized replacement for wrapped tokens, e.g., ckBTC.
    -   Eliminates risks of intermediary-based wrapping.
    -   Canister smart contracts own key pairs and derive addresses for real Bitcoin transfers.
    -   Mint ckBTC upon receiving Bitcoin and redeem ckBTC for Bitcoin.
    -   Mirrors properties and valuation of the original token on the Internet Computer.

<hr/>

## Tokenomics & Governance:

-   **Tokenomics:**

    -   ICP token is the utility token of the Internet Computer.
    -   Functions include staking for voting rights and rewards, participation in SNS swaps, purchasing cycles for dApps, and rewarding node providers.

-   **Network Nervous System (NNS):**

    -   Coordinates decentralized nodes on the Internet Computer.
    -   Determines subnet topology, protocol versions, and upgrades.
    -   Relies on open tokenized governance.
    -   One of the largest DAOs.
    -   Participation through staking ICP tokens.

-   **Service Nervous System (SNS):**

    -   Decentralized control solution for applications
    -   Tokenizes and decentralizes dapps into a DAO
    -   New token creation through community-based fundraising
    -   Token holders contribute to decision-making
    -   Eliminates single points of failure and enhances censorship resistance
    -   Tokens incentivize user adoption and participation

<hr/>

## Chain-evolution technology:

-   **Infinite Scalability:**

    -   New subnets created for horizontal scalability
    -   NNS selects spare nodes for subnet configuration
    -   New subnet blockchain expands platform capacity

-   **Fault Tolerance:**

    -   Individual node failures addressed by NNS
    -   Spare nodes replace failed nodes in subnets
    -   Synchronization and contribution to subnet consensus

-   **Protocol Upgrade:**

    -   NNS governs Internet Computer blockchain
    -   Handles protocol upgrades based on community adoption
    -   Addresses challenges of protocol changes, state preservation, downtime minimization, and autonomous rollouts

<hr/>

## Smart contracts

-   **Canisters:**

    -   Computational units bundling code and state
    -   Allow for concurrent execution and asynchronous messaging
    -   Managed by controllers with deployment and execution control

-   **Motoko:**

    -   Purpose-built language for Internet Computer smart contracts
    -   Strongly typed, actor-based, and supports persistence and message passing
    -   Productivity and safety features for ease of development

-   **Certified Variables:**

    -   Canister smart contract variables with automatic Merkle tree certificates
    -   Certificates signed by Internet Computer blockchain
    -   Enables authenticity verification using the platform's public key

<hr/>

## Web access

-   **Smart Contracts Serve the Web:**

    -   The Internet Computer hosts full dapps, including frontend, backend, and data
    -   Dapps run 100% on-chain, maintaining blockchain security and speed
    -   HTTP requests are securely served by the IC

-   **Asset Certification:**

    -   Assets on the Internet Computer are tamper-proof with accompanying certificates
    -   Certificates are signed by the entire subnet, ensuring authenticity
    -   Users can verify asset correctness, even with potentially malicious nodes

-   **Boundary Nodes:**

    -   Boundary nodes are gateways to the Internet Computer, enabling access to canister smart contracts
    -   They translate user requests to on-chain API canister calls
    -   Boundary nodes act as a cache, enhancing dapp performance

-   **Internet Identity:**

    -   Internet Identity offers advanced and secure cryptographic authentication
    -   It provides convenience, cross-device functionality, and privacy protection
    -   Users create independent accounts for each website, safeguarding privacy and reducing management burden
