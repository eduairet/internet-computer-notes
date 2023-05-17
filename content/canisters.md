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
