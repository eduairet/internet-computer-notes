# Deploy

-   ICP developer workflow
    -   Download tools and examples
    -   Plan the basic design
    -   Create a new project
    -   Start local execution environment
    -   Create the canister
    -   Code
        -   Write frontend and backend code
        -   Build the project
        -   Install code on the local execution environment
            -   WASM execution
        -   Test and debug the DApp
    -   Repeat code until you’re ready to go to mainnet deployment
-   Deployment on mainnet needs cycles and an Internet Identity
    -   Is important to understand that computing space and power should be funded, here are some ideas
        -   By sales (NFT sales)
        -   By a DAO

## Deploying and upgrade

-   Canister lifecycle
    -   Canisters are WebAssembly code
    -   Every canister has an ID
    -   Deployed canisters can call code from other canister
    -   Canister consume cycles while running and also their data
        -   Very important for developers to track that
    -   Low cycle canisters will be frozen
    -   No cycle canisters are deleted automatically from IC
-   Deploying code (atomic)
    -   `install` - This mode is called by canister without installed code
    -   `reinstall` - deletes previous code and state, and adds new
    -   `upgrade` - changes the code without loosing its state
