# Security recommendations

## General

-   Queries that are dependant of update calls need authentication guarantees
-   Don’t load unsafe assets or scripts
-   Don’t use outdated components
    -   `npm audit` `cargo audit`
-   Don’t use forked repositories not maintained
-   Don’t use battle tested components
-   Don’t use crypto algorithms created by you, always use well known libraries (NIST or IETF approved)
-   Don’t store keys on local storage
-   Deep test your code
    -   And remove test on production
-   Approve with transactions

## Canister

-   Use decentralized governance system for canisters
    -   Using the Computer's Service Nervous System to approve canister control
    -   Making the canister immutable
-   Verify the ownership of smart contracts you depend on
    -   Make sure it is controlled by the NNS, a service nervous system (SNS) or a decentralized governance system
-   Authentication
    -   Ask for Internet Identity authentication
    -   Deny service to anonymous users
-   Disallow the anonymous principal in authenticated calls
    -   Return errors on anonymus calls
-   Use HTTP asset certification and avoid serving your dApp through `raw.icp0.io`
    -   `raw.icp0.io` is used by scammers to phish users
    -   Serve assets using the `http_request` method
-   `Rust` Use thread_local! with Cell/RefCell for state variables and put all your globals in one basket
    -   Use `thread_local!` with `Cell/RefCell` for state variables (from effective Rust canisters).
    -   Put all your globals in one basket (from effective Rust canisters).
-   Limit the amount of data that can be stored in a canister per user
    -   Check how much memory can be used by an user at every update call
-   Consider using stable memory, version it, test it
-   Consider encrypting sensitive data on canisters
-   Create backups
-   Message execution basics
    -   Only a single message is processed at a time per canister
    -   Each call (query / update) triggers a message
    -   Successfully delivered requests are received in the order in which they were sent
    -   Messages from interleaving calls have no reliable execution ordering
        -   Be aware that there is no reliable message ordering to avoid reentrancy bugs
    -   On a trap / panic, modifications to the canister state for the current message are not applied
        -   Don’t lock shared resources across await boundaries (from effective Rust canisters).
        -   Don’t panic after await (from effective Rust canisters).
    -   Inter-canister messaging is not reliable
        -   Handle rejected inter-canister calls correctly
-   Carefully review any canister code that makes async inter-canister calls (await) that can be schedule ash the same time
-   Only make inter-canister calls to trustworthy canisters
-   Make sure there are no loops in call graphs
    -   If you can, never use loops for these calls
-   Upgrades
    -   Be careful with panics during upgrades, they can be locked forever
        -   Test the upgrades
    -   Reinstantiate timers during upgrades
        -   Keep track of global timers in the pre_upgrade hook. Store any state in stable variables.
        -   Set timers in the post_upgrade hook.
    -   Test your canister code even in presence of system API calls
    -   Make canister builds reproducible
    -   Expose metrics from your canister
    -   Don’t rely on time being strictly monotonic
    -   Protect against draining the cycles balance
    -   Validate inputs accordingly to [OWASP](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html)
    -   `Rust` Don’t use unsafe Rust code
    -   `Rust` Avoid integer overflows
    -   `Rust` Avoid floating point arithmetic for financial information
    -   For expensive calls, consider using captchas or proof of work

## Web Development

-   Authentication
    -   Use a well-audited authentication service and client side IC libraries
    -   Use Internet Identity
    -   Set authentication timeout (ideally 30 minutes)
    -   Use `agent-js` and `auth-client`
        -   Don’t use fetchRootKey in agent-js in production
-   Validate input in the frontend
-   Don’t load JavaScript (and other assets) from untrusted domains
-   Define security headers including a content security policy (CSP)
-   Crypto: protect key material against XSS using Web Crypto API
-   Use a secure web framework such as Svelte, without relying on insecure function like `@html`
-   Make sure the logout is effective (clean storage)
-   Use prompts to warn the user on any security critical actions, let the user explicitly confirm

## Resources

-   [General Security Best Practices](https://internetcomputer.org/docs/current/developer-docs/security/general-security-best-practices)
-   [Canister Security Best Practices](https://internetcomputer.org/docs/current/developer-docs/security/rust-canister-development-security-best-practices)
-   [Web Development Security Best Practices]()
-   [Best Practices Conversations](https://www.youtube.com/watch?v=PneRzDmf_Xw&list=PLuhDt1vhGcrez-f3I0_hvbwGZHZzkZ7Ng&index=3&t=4s)
-   [Web Crypto API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API)
