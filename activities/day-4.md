# Desplegado

## Entorno de pruebas _(Staging)_

-   Beneficios de un entorno de pruebas:
    -   Permite realizar **pruebas _end-to-end_** de las funciones antes de implementar en producción.
    -   Facilita las **pruebas de integración** del proyecto con otros servicios.
    -   Permite pruebas de los **flujos de implementación** en un entorno controlado.
    -   Ayuda a **estimar los costos** antes de lanzar una función a todos los usuarios.
-   Configuración de un entorno de pruebas:
    -   Las redes (o entornos) se definen en `dfx.json`
    -   Se agrega una red de pruebas llamada `myStaging` a `dfx.json` en la sección `networks`
        ```JSON
        "myStaging": {
            "providers": [
                "https://icp0.io"
            ],
            "type": "persistent"
        }
        ```
-   Configurar una **_wallet_** de ciclos para el entorno de pruebas estableciendo la identidad y el ID de la billetera correctos.
    -   Utilizar la **misma billetera de ciclos que la red principal** de Internet Computer para el entorno de pruebas.
        -   Establecer la billetera para la red de pruebas utilizando la billetera configurada actualmente en la red principal de Internet Computer.
            ```Shell
            dfx identity set-wallet "$(dfx identity get-wallet --network ic)" --network myStaging
            ```
    -   Utilizando una **billetera separada:**
        -   Seguir las instrucciones en la [guía de inicio rápido](https://internetcomputer.org/docs/current/developer-docs/setup/deploy-mainnet) de la red para crear una billetera de ciclos separada para el entorno de pruebas.

## Costos

-   El **NNS controla los costos** de cálculo y almacenamiento.
-   El **NNS determina la cantidad de ciclos requeridos** para las acciones de cálculo a nivel bajo en base a diversos factores y propuestas de la comunidad.
-   Costos de transacciones de cómputo y almacenamiento en Internet Computer:

    -   Los canisters utilizan ciclos, **similar al gas en Ethereum.**
    -   Internet Computer sigue un modelo de **_contrato inteligente paga,_** donde los canisters se cargan previamente con ciclos, **eliminando la necesidad de que los usuarios finales paguen por el gas.**
    -   Los **precios de los ciclos** en las subredes de alta replica **aumentan de manera lineal en función del número de nodos.**
    -   La fórmula de precios para las transacciones en subredes de `n` nodos se calcula **en función del precio de referencia de la subred de tamaño de referencia.**
    -   Los canisters **implementados en subredes de alta replica** deben anticipar el **aumento de los precios de los ciclos con el factor de replica** de la subred y **asignar más ciclos a las llamadas.**
    -   Ver los costos en la [página oficial.](https://internetcomputer.org/docs/current/developer-docs/gas-cost)

## Módulos de WebAssembly grandes

-   Los módulos de WebAssembly que son (ligeramente) **más grandes que 2MB** aún pueden ser instalados en el IC utilizando la **compresión de archivos gzip antes de la carga.**

## Dominio personalizado

-   Consulta la documentación completa en [internetcomputer.org/docs/current/developer-docs/production/custom-domain/](https://internetcomputer.org/docs/current/developer-docs/production/custom-domain/)
-   Adquiere un dominio a través de tu proveedor preferido.
-   Elige uno de los dos enfoques:

    -   **Registrar el dominio con _Boundary Nodes_**

        -   **Configura el registro `DNS` de tu dominio.**
            -   Agrega una entrada `CNAME` para tu dominio apuntando a `icp1.io`
            -   Agrega una entrada `TXT` que contenga el ID del canister para el subdominio `_canister-id` de tu dominio.
            -   Agrega una entrada `CNAME` para el `subdominio _acme`-challenge apuntando a `_acme-challenge.CUSTOM_DOMAIN.icp2.io` para adquirir el certificado.
        -   Crea un archivo llamado `ic-domains` en tu canister dentro del directorio `.well-known` enumerando el dominio(s) personalizado(s) que deseas utilizar.
            -   Crea un archivo adicional llamado `.ic-assets.json` para incluir el directorio `.well-known` en los activos de tu canister.
            -   Implementa el canister actualizado.
        -   **Registra el dominio** con los _Boundary Nodes_ emitiendo una solicitud POST
            ```Shell
            curl -sLv -X POST \
                -H 'Content-Type: application/json' \
                https://icp0.io/registrations \
                --data @- <<EOF
            {
                "name": "CUSTOM_DOMAIN"
            }
            EOF
            # Respuesta: {"id":"REQUEST_ID"}
            ```
        -   Sigue el **progreso de tu solicitud** de registro emitiendo una solicitud GET
            ```Shell
            curl -sLv -X GET https://icp0.io/registrations/REQUEST_ID
            ```
        -   Una vez que tu **solicitud de registro esté disponible,** espera a que el certificado esté disponible en todos los _Boundary Nodes._
        -   Después de que el certificado esté disponible, deberías poder **acceder a tu canister utilizando el dominio personalizado.**
        -   Para **actualizar** un dominio personalizado para apuntar a un canister diferente, **actualiza el registro `DNS`** para el ID del canister y notifica a un nodo frontera emitiendo una solicitud PUT

            ```Shell
            curl -sLv -X PUT https://icp0.io/registrations/REQUEST_ID
            ```

        -   Para **eliminar** un dominio personalizado, elimina los registros `DNS` (`TXT` `CNAME`) y notifica a un _Boundary Node_ la eliminación mediante una solicitud DELETE
            ```Shell
            curl -sLv -X DELETE https://icp0.io/registrations/REQUEST_ID
            ```

    -   **Hospedar el dominio en tu propia infraestructura.**
        -   Sigue las instrucciones en la [sección Dominios personalizados usando tu propia infraestructura.](https://internetcomputer.org/docs/current/developer-docs/production/custom-domain/#custom-domains-using-your-own-infrastructure)
