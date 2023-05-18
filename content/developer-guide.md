# Motoko Programming Language Guide

## Environment

-   [Motoko VS Code extension](https://github.com/dfinity/vscode-motoko)
    -   Install through VS Marketplace
    -   Features
        -   Package managers Vessel and MOPS included
        -   Doesn't need to install `dfx` separately
        -   Includes chema validation and autocompletion for `dfx.json` config files
        -   Documentation on `HOVER`

## Deployment

-   You'll need a `unix` terminal
-   Downloaw and install the SDK package
    ```Shell
    sh -ci "$(curl -fsSL https://internetcomputer.org/install.sh)"
    ```
-   Accept the license agreement by pressing `y` and `return`
-   Check that it was correctly installed
    ```Shell
    dfx --version
    ```
-   Create a new project
    ```Shell
    dfx new hello && cd hello
    ```
-   **Start local deployment**
    -   In a first terminal start the canister execution environment
        ```Shell
        hello $ dfx start --background
        ```
    -   In the second terminal you'll run your commands to work on the project
        -   Install node packages
            ```Shell
            npm install
            ```
        -   Register, build, and deploy the dapp
            ```Shell
            # It will deploy frontend and backend
            dfx deploy
            ```
        -   Now you can interact with your deployment
            ```Shell
            # dfx canister call project_name function_name arguments
            dfx canister call hello_backend greet everyone
            # ("Hello, everyone!")
            ```
        -   Stop the execution environment
            ```Shell
            dfx stop
            ```
-   **Mainnet deployment**

    -   Check your connection to the IC
        ```Shell
        dfx ping ic
        ```
    -   It should return data about your connection
        ```Shell
        {
        "certified_height": 34912399  "ic_api_version": "0.18.0"  "impl_hash": "83128b3d340d81e601413dcebcbda3bdf0fd5dcb151f7cc0e43790cedd3e5f40"  "impl_version": "6d21535b301fee2ad3e8a0e8af2c3f9a3d022111"  "replica_health_status": "healthy"  "root_key": [48, 129, 130, 48, 29, 6, 13, 43, 6, 1, 4, 1, 130, 220, 124, 5, 3, 1, 2, 1, 6, 12, 43, 6, 1, 4, 1, 130, 220, 124, 5, 3, 2, 1, 3, 97, 0, 129, 76, 14, 110, 199, 31, 171, 88, 59, 8, 189, 129, 55, 60, 37, 92, 60, 55, 27, 46, 132, 134, 60, 152, 164, 241, 224, 139, 116, 35, 93, 20, 251, 93, 156, 12, 213, 70, 217, 104, 95, 145, 58, 12, 11, 44, 197, 52, 21, 131, 191, 75, 67, 146, 228, 103, 219, 150, 214, 91, 155, 180, 203, 113, 113, 18, 248, 71, 46, 13, 90, 77, 20, 80, 95, 253, 116, 132, 176, 18, 145, 9, 28, 95, 135, 185, 136, 131, 70, 63, 152, 9, 26, 11, 170, 174]
        }
        ```
    -   Verify your identity (principal identifier)

        ```Shell
        # Developer identity
        dfx identity whoami
        # default

        # Textual representation of the principal
        dfx identity get-principal
        # tsqwz-udeik-5migd-ehrev-pvoqv-szx2g-akh5s-fkyqc-zy6q7-snav6-uqe

        # Account identifier
        dfx ledger account-id
        # 03e3d86f29a069c6f2c5c48e01bc084e4ea18ad02b0eec8fccadf4487183c223

        # Balance
        dfx ledger --network ic balance
        # 0.00000000 ICP
        ```

    -   Cycles wallet

        -   Create a new canister with cycles
            ```Shell
            dfx ledger --network ic create-canister <principal-identifier> --amount <icp-tokens>
            ```
            -   `<principal-identifier>` is the textual representation of the principal
            -   `<icp-tokens>` is the amount of tokens you want to convert in `cycles`
        -   Install the cycles wallet code in the canister
            ```Shell
            dfx identity --network ic deploy-wallet <canister-identifer>
            ```
            -   `<canister-identifer>` was return in the previous step
        -   Validate the cycles wallet

            ```Shell
            dfx identity --network ic get-wallet
            # Returns the canister identifier

            dfx wallet --network ic balance
            # Returns the balance for the your cycles wallet
            ```

            -   You can check your wallet in your browser with an URL like this `https://<WALLET-CANISTER-ID>.icp0.io`

        -   Add more cycles

            ```Shell
            dfx ledger --network ic top-up <WALLET-CANISTER-ID> --amount 1.005
            ```

        -   Check the [documentation](https://internetcomputer.org/docs/current/developer-docs/setup/deploy-mainnet) for extra steps (like authentication)

    -   Deploy to mainnet
        ```Shell
        dfx deploy --network ic
        ```
    -   If everything went well you'll be able to interact with your DApp

        ```Shell
        # dfx canister --network ic call canister_name function_name arguments
        dfx canister --network ic call hello_backend greet '("everyone": text)'

        ## Check balance after the interaction
        dfx wallet balance
        ```
