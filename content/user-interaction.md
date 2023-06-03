# User Interaction

-   No need of native token
-   Public key cryptography through user device privacy methods like FaceID

## Candid

-   Interface description language
-   Strong typed
-   Describes the public interface of a service
-   It’s independent from the canister language, which allows interoperability
-   Advantages:

    -   Assign the Candid value directly to type and value of the host language (language-agnostic)
    -   Define the way how a service can be updated
    -   It can pass values, references and methods (superior order)

        ```CANDID
        service counter : {
        add : (nat) -> ();
        subtract : (nat) -> ();
        get : () -> (int) query;
        subscribe : (func (int) -> ()) -> ();
        divMod : (dividend : nat, divisor : nat) -> (div : nat, mod : nat);
        }
        ```

    -   It has ICP specific functionality
    -   Complex types

        ```CANDID
        type address = record {
        street : text;
        city : text;
        zip_code : nat;
        country : text;
        };
        service address_book : {
        set_address: (name : text, addr : address) -> ();
        get_address: (name : text) -> (opt address) query;
        }
        ```

    -   They can be updated

        ```CANDID
        service counter : {
        add : (nat) -> ();
        subtract : (nat) -> ();
        get : () -> (int) query;
        subscribe : (func (int) -> ()) -> ();
        }
        ```

        ```CANDID
        type timestamp = nat;
        service counter : {
        set : (nat) -> ();
        add : (int) -> (new_val : nat);
        subtract : (nat, trap_on_underflow : opt bool) -> (new_val : nat);
        get : () -> (nat, last_change : timestamp) query;
        subscribe : (func (nat) -> (unregister : opt bool)) -> ();
        }
        ```

## Resources

-   [Create Identity](https://identity.ic0.app/)
-   [Internet identity repo](https://github.com/dfinity/internet-identity/blob/main/README.md#build-features-and-flavors)
-   [Candid repo](https://github.com/dfinity/candid)
-   [Candid spec](https://internetcomputer.org/docs/current/references/ii-spec)
-   [Candid spec repo](https://github.com/dfinity/candid/blob/master/spec/Candid.md)
-   [Example](https://github.com/krpeacock/auth-client-demo/tree/vanilla-js)
