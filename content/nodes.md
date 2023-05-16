# Nodes and Subnet Blockchains

-   Internet Computer subnet blockchains consist of decentralized nodes that run the blockchain protocol's software components.
-   Each subnet is a blockchain with independently owned and controlled nodes that replicate state and computation across the subnet.

### Replica Architecture

-   Replicas are the software components running on each node in a subnet blockchain.
    -   The replica architecture includes:
    -   Peer-to-peer networking layer for message collection and advertisement.
    -   Consensus layer for selecting and sequencing messages to create blockchain blocks.
    -   Message routing layer for routing messages between subnets and managing dapp queues.
    -   Execution environment for deterministic computation of smart contract execution.

### Internet Computer Components in a Developer's Environment

-   Developers don't need to know all the details of the blockchain architecture but understanding key components can be useful.
-   The development environment includes replica components to provide an execution environment for deployment.

### Subnet Blockchains

-   Subnets are collections of replicas running a separate instance of the consensus mechanism to create their own blockchain.
-   Subnets can communicate with each other and are controlled by the root subnet using chain key cryptography.
-   Subnets allow the Internet Computer to scale indefinitely by breaking the single-machine barrier.
-   Different subnets may have different configurations based on canister security, size, and feature requirements.
-   Main subnet types include system and application subnets.
-   The system subnet has no cycles accounting, generous per-call instruction limits, and wasm module size limits.
