# Desarrollo

## Entorno

-   Instalar la extensión de Motoko para VS Code a través de VS Marketplace

## Desplegado

-   Se necesita una terminal unix.
-   Descargar e instalar el paquete del SDK.

    ```Shell
    sh -ci "$(curl -fsSL https://internetcomputer.org/install.sh)"
    # Checar versión cuando termine
    dfx --version
    # dfx 0.14.0
    ```

-   Crear nuevo proyecto

    ```Shell
    dfx new hello && cd hello
    ```

-   **Desplegado local**
    -   Comenzar el ambiente de desarrollo y desplegar
        ```Shell
        dfx start --background
        # Revisar que las dependencias estén instaladas cuando se requieran
        npm install
        # Desplegar proyecto
        dfx deploy
        # Cuando hay varios canisters en juego hacer cada uno individualmente
        ```
    -   Interacción con el proyecto
        ```Shell
        # dfx canister call nombre_proyecto nombre_funcion argumentos
        dfx canister call hello_backend greet everyone
        # ("¡Hola, a todos!")
        ```
    -   Detener ambiente local
        ```Shell
        dfx stop
        ```
-   **Desplegado en mainnet**

    -   Verifica tu conexión con la IC
        ```Shell
        dfx ping ic
        # {"certified_height": 34912399  "ic_api_version": "0.18.0"  "impl_hash": "831..."  "impl_version": "6d2..."  "replica_health_status": "healthy"  "root_key": [48, 129, ...]}
        ```
    -   Verifica tu identidad (identificador principal)

        ```Shell
        # Identidad del desarrollador
        dfx identity whoami
        # default

        # Representación textual del principal
        dfx identity get-principal
        # tsqwz-udeik-5migd-ehrev-pvoqv-szx2g-akh5s-fkyqc-zy6q7-snav6-uqe

        # Identificador de cuenta
        dfx ledger account-id
        # 03e3d86f29a069c6f2c5c48e01bc084e4ea18ad02b0eec8fccadf4487183c223

        # Saldo
        dfx ledger --network ic balance
        # 0.00000000 ICP
        ```

    -   Cycles wallet

        -   Crea un nuevo canister con cycles

            ```Shell
            dfx ledger --network ic create-canister <principal-identifier> --amount <icp-tokens>
            ```

            -   `<principal-identifier>` es la representación textual del principal
            -   `<icp-tokens>` es la cantidad de tokens que deseas convertir en cycles

        -   Instala el código del ciclo de la cartera en el canister

            ```Shell
            dfx identity --network ic deploy-wallet <canister-identifer>
            # <canister-identifer> fue devuelto en el paso anterior
            ```

        -   Valida el ciclo de la cartera

            ```Shell
            dfx identity --network ic get-wallet
            # Devuelve el identificador del canister

            dfx wallet --network ic balance
            # Devuelve el saldo de tu ciclo de cartera

            # Disponible también en el navegador
            https://<WALLET-CANISTER-ID>.icp0.io
            ```

        -   Se pueden agregar más ciclos

            ```Shell
            dfx ledger --network ic top-up <WALLET-CANISTER-ID> --amount 1.005
            ```

    -   Despliega e interactua con tu canister en mainnet

        ```Shell
        dfx deploy --network ic

        # dfx canister --network ic call nombre_proyecto nombre_funcion argumentos
        dfx canister --network ic call hello_backend greet '("everyone": text)'

        # Verifica el saldo después
        dfx wallet balance
        ```
