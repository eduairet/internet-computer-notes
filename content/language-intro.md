# Language Intro

-   Comments

    ```Motoko
    // Comment

    // Multi-
    // line
    ```

-   Strings

    -   Type name -> `Text`
    -   UTF-8 encoded binaries

        ```Motoko
        // Double quotes
        "👩‍💻 こんにちは Motoko 💫"
        ```

    -   Concatenation
        ```Motoko
        let n = "Lalo";
        "I am, " # n # "!" // Uses operator #
        // "I am, Lalo!"
        ```
    -   Escaping
        ```
        \n	Newline
        \r	Carriage Return
        \t	Tab
        \"	Double Quote
        \\	Backslash
        ```
    -   Common methods

        ```Motoko
        myString.size() // length
        Text.chars() // returns iter to loop over the chars
        "A" == "A" // can be compared with other string
        ```

    -   Types can be converted to string
        ```Motoko
        32.toText(natVal)
        ```

-   Booleans
    -   Type name -> `Bool`
    -   `true` and `false`
    -   `and` and `or` can be used to compare and are _short circuiting_
        ```Motoko
        true and false // false
        false or true // true
        ```
    -   `not` is for negation
        ```Motoko
        not false // true
        ```
-   Numbers

    -   Integers

        -   Type names -> `Int` `Nat`
        -   `Int`
            ```Motoko
            // Positive or negative integers
            +1 // 1 : Int -> Will be treated as Nat
            -1 // -1 : Int
            ```
        -   `Nat`

            ```Motoko
            1_000 // : Nat can be written with underscore
            123456789 // 123_456_789 : Nat -> Output will have underscore
            0xF // 15 : Nat -> Can be hexadecimal
            ```

        -   Number operations

            ```Motoko
            1 + 1 // => 2 : Nat
            +2 - 5 // => -3 : Int
            3 * 3 // => 9 : Nat
            5 / 2 // => 2 : Nat
            5 % 2 // => 1 : Nat
            2 > 1  // => true
            2 < 1  // => false
            2 >= 1 // => true
            2 <= 1 // => false
            0.1 + 0.1 + 0.1 == 0.3 // => false / Float have limited precision
            ```

    -   Bounded integers

        -   Integer types with fixed precision
            -   Memory efficiency
            -   Exact sizing
            -   Execution efficiency
            -   Bitwise arithmetic
        -   Are declared manually
        -   Types (bites are specified in the type)

            ```Motoko
            // Nat8, Nat16, Nat32, Nat64
            2 : Nat8
            // Int8, Int16, Int32, Int64
            -2 : Int8
            ```

        -   Modular arithmetics

            -   Protect the numbers to overflow their bounds `+%, -%, *%, **%`

                ```Motoko
                let a = 255 : Nat8;
                let b = 1 : Nat8;
                a + b // execution error, arithmetic overflow

                let a = 255 : Nat8;
                let b = 1 : Nat8;
                a +% b // 0 : Nat8

                let a = 127 : Int8;
                let b = 1 : Int8;
                a +% b // -128 : Int8
                ```

        -   Bitwise arithmetics

            ```Motoko
            // Binary OR "|"
            let a = 64 : Nat8; // binary 1000000
            let b = 65 : Nat8; // binary 1000001
            a | b // 65 : Nat8 == binary 1000001

            // Binary XOR "^"
            let a = 64 : Nat8; // binary 1000000
            let b = 65 : Nat8; // binary 1000001
            a ^ b // 1 : Nat8  == binary 0000001

            // Binary shift left "<<"
            let a = 64 : Nat8; //    binary  1000000
            a << 1 // 128 : Nat8  == binary 10000000

            // Binary shift right ">>"
            let a = 64 : Nat8; //   binary 1000000
            a >> 1 // 32 : Nat8  == binary  100000

            // Binary rotation left "<<>"
            let a = 255 : Nat8; //    binary 11111111
            a <<> 1 // 255 : Nat8  == binary 11111111

            // Binary rotation right "<>>"
            let a = 64 : Nat8; //   binary 1000000
            a <>> 1 // 32 : Nat8 == binary  100000
            ```

        -   Bounded and arithmetics types interop

            -   You need to treat types before performing operations between them

                ```Motoko
                // Generating an unbound type from a bound value
                import Nat8 "mo:base/Nat8";
                let a = 64 : Nat8;
                let b : Nat = Nat8.toNat(a);
                b

                // Generating a bound type from an unbound value
                import Nat8 "mo:base/Nat8";
                let b = 64 : Nat;
                let c : Nat8 = Nat8.fromNat(b);
                c
                ```

    -   Floats

        -   Decimal numbers

            ```Motoko
            1.5 // 1.5 : Float
            2 : Float // 2 : Float

            // Bad for precision
            -0.1 // -0.100_000_000_000_000_01 : Float

            // Science
            -0.3e+15 // -300_000_000_000_000 : Float

            // Support hex format with binary exponential
            -0xFp+10 // = 16 * 2^10 = -15_360 : Float
            ```

        -   Operators

            ```Motoko
            1.0 + 1.4 // => 2.399_999_999_999_999_9 : Float
            5.0 - 1.5 // => 3.5 : Float
            5.0 / 2.0 // => 2.5 : Float
            3.0 * 3.1 // => 9.300_000_000_000_000_7 : Float
            2.0 > 1.0  // => true
            2.0 < 1.0  // => false
            2.0 >= 1.0 // => true
            2.0 <= 1.0 // => false
            // Be aware that precision might make comparisons tricky
            ```
