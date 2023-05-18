# Canisters

## Canisters and code

-   Computational units on the Internet Computer that execute code.
-   Contain both program code and state information.
-   Can be developed in various programming languages like Motoko, C, Rust, and more.
-   Users interact with canisters through entry point functions using a frontend client.
-   They have query calls (read-only) and update calls (state-modifying).
-   Motoko is a programming language designed for the Internet Computer with features like actor-based programming and message serialization.
-   They have registered identities and use digital signing keys for authentication.
-   They consume resources and pay for them using cycles.
-   Cycles reflect the cost of operations and can be obtained by converting ICP tokens.

## Trust in Canisters

-   Trust in canisters is crucial for DeFi applications.
-   Ensuring canister behavior involves inspecting the source code and comparing it to the deployed the `WASM` module.
-   Reproducibility of the build is important for verifying the integrity of the canister's code.
-   Canister controllers have the ability to change the code and control the assets held by it.
-   Options for decentralizing canister control include sole control by the **NNS** or a distributed governance mechanism.
-   Verifying the list of controllers can be done using `dfx` or a `read_state` request.
-   Setting the canister's controller to be itself requires careful verification to prevent unauthorized upgrades.
-   Using a "black hole" canister allows third parties to obtain useful information while maintaining control.
-   Setting the controller to a distributed governance mechanism provides more focused control and governance.

## HTTPS Outcalls:

-   Canister smart contracts on the Internet Computer can make HTTP outcalls to obtain off-chain data or interact with off-chain systems.
-   HTTPS outcalls allow canisters to request URLs, and the results are compared for consistency across nodes.
-   Consistent results are agreed upon by consensus and returned to the requesting canister.
-   The architecture involves new components in the IC protocol stack, such as the HTTP pool manager and the HTTP adapter shim.
-   Responses may differ due to variable elements or API implementations.
-   Transform functions normalize responses to achieve consensus.
-   Canister developers provide the transformation function if needed.
-   Documentation and a recipe are available for developers to utilize the feature.

## Deployment

-   **Staging**

    -   Benefits of a Staging Environment:
        -   **Testing:** Allows end-to-end testing of features before deploying to production.
        -   **Integration Testing:** Testing the integration of the project with other services.
        -   **Deployment Workflows:** Testing the deployment workflows in a controlled environment.
        -   **Cost Estimation:** Estimating costs before releasing a feature to all users.
    -   Setting up a Staging Environment:
        -   **Network Configuration:** Networks (or environments) are defined in `dfx.json` either assumed or explicitly configured.
        -   **Adding Staging Network:** Add a **staging network** named `myStaging` to `dfx.json` under the _"networks"_ section.
    -   **Network Definition:**

        ```JSON
        "myStaging": {
            "providers": [
                "https://icp0.io"
            ],
            "type": "persistent"
        }
        ```

    -   **Configuring a Wallet:** Configure the cycles wallet for the staging environment by setting the correct identity and wallet ID.

        -   Using Default Wallet:

            -   Use the same cycles wallet as the main ic network for the staging environment.
            -   Set the wallet for the staging network using the main ic network's currently configured wallet.
                ```Shell
                dfx identity set-wallet "$(dfx identity get-wallet --network ic)" --network myStaging
                ```

        -   Using Separate Wallet:

            -   Create a separate cycles wallet for the staging environment.
            -   Follow the instructions in the [network quickstart guide](https://internetcomputer.org/docs/current/developer-docs/setup/deploy-mainnet) to create a separate cycles wallet for the staging environment.

-   **Costs**

    -   The Role of the **Network Nervous System (NNS)** in **Defining Costs:**

        -   The **NNS controls the computation and storage costs.**
        -   The **NNS determines the number of cycles required for low-level computation actions** based on various factors and community proposals.

    -   **Cost of Compute and Storage** Transactions on the Internet Computer:

        -   **Canister** smart contract **computations** on the Internet Computer **use cycles, similar to gas on Ethereum.**
        -   Internet Computer follows a **"smart contract pays" model,** where canister smart contracts are pre-charged with cycles, **eliminating the need for end users to pay for gas.**
        -   High-replication application subnets were introduced in late November 2022, with varying replication factors.
        -   **Cycles prices for high-replication subnets scale linearly** based on the number of nodes.
        -   The **pricing formula** for transactions on n-node subnets is computed **based on the reference-size subnet price.**
        -   Canisters deployed on **high-replication** subnets should **anticipate increasing cycles prices with the subnet's replication factor** and attach more cycles to calls.

    -   Check costs at the [official page](https://internetcomputer.org/docs/current/developer-docs/gas-cost)

-   **Large web assembly modules**

    -   WebAssembly modules that are (slighly) larger than 2MB can still be installed on the IC by using gzip file compression before uploading

        ```Shell
        gzip my-canister.wasm
        dfx canister install my-canister --wasm my-canister.wasm.gz

        # You may need to add --mode reinstall or --mode upgrade when uploading the module to an existing canister

        # Compression is currently not supported by dfx deploy
        ```

-   **Custom domain**

    -   Check full documentation at [internetcomputer.org/docs/current/developer-docs/production/custom-domain/](https://internetcomputer.org/docs/current/developer-docs/production/custom-domain/)
    -   Acquire a domain through your preferred registrar.
    -   Choose one of the two approaches:

        -   **Register the domain with the boundary nodes.**

            -   Configure the DNS record of your domain
                -   Add a `CNAME` entry for your domain pointing to `icp1.io`
                -   Add a `TXT` entry containing the canister ID to the `_canister-id`-subdomain of your domain.
                -   Add a `CNAME` entry for the `_acme-challenge`-subdomain pointing to `_acme-challenge.CUSTOM_DOMAIN.icp2.io` for certificate acquisition.
            -   Create a file named `ic-domains` in your canister under the `.well-known` directory, listing the custom domain(s) you want to use.
                -   Create an additional file called `.ic-assets.json` to include the `.well-known` directory in your canister's assets.
                -   Deploy the updated canister.
            -   Register the domain with the boundary nodes by issuing a POST request

                ```Shell
                curl -sLv -X POST \
                    -H 'Content-Type: application/json' \
                    https://icp0.io/registrations \
                    --data @- <<EOF
                {
                    "name": "CUSTOM_DOMAIN"
                }
                EOF
                # Response: {"id":"REQUEST_ID"}
                ```

            -   Track the progress of your registration request by issuing a GET request

                ```Shell
                curl -sLv -X GET https://icp0.io/registrations/REQUEST_ID
                ```

            -   Once your registration request is available, wait for the certificate to become available on all boundary nodes.
            -   After the certificate is available, you should be able to access your canister using the custom domain.
            -   To **update** a custom domain to **point to a different canister,** update the `DNS` record for the canister ID and notify a boundary node by issuing a PUT request

                ```Shell
                curl -sLv -X PUT https://icp0.io/registrations/REQUEST_ID
                ```

            -   To **remove** a custom domain, remove the `DNS` records (` TXT` `CNAME `) and notify a boundary node of the removal using a DELETE request
                ```Shell
                curl -sLv -X DELETE https://icp0.io/registrations/REQUEST_ID
                ```

        -   **Host the domain on your own infrastructure.**

            -   Follow the instructions in the [Custom Domains using your Own Infrastructure section.](https://internetcomputer.org/docs/current/developer-docs/production/custom-domain/#custom-domains-using-your-own-infrastructure)
