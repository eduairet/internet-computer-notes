# Internet Computer

## ¿Qué es?

-   Es una _blockchain_ que ejecuta aplicaciones descentralizadas a la **velocidad del Internet.**
-   Se ejecuta en nodos distribuidos en _Data Centers_ alrededor del mundo.
-   Los nodos se agrupan en **_subnets_**
-   El **_Network Nervous System (NNS)_** es su **_decentralized source of truth_**
-   Rastrea cuentas y transacciones de manera pública.
-   Permite la organización de los participantes.

## Canisters

-   **_Smart contracts_** que incluyen el programa de cómputo y los datos (completamente _on-chain_).
-   Pueden almacenar **grandes cantidades de memoria a bajo costo** ~5 USD/GB.
-   Pueden pagar por su propia computación ayudando a la experiencia de usuario, **previniendo costes por gas.**

## Consenso

-   Red **controlada por una DAO.**
-   Consenso de **baja latencia y alta velocidad** (1 a 2 segundos usando MBs).
-   Logra un **`finality` criptográfico,** asegurando cambios de estado permanentes.
-   Resuelve la **tolerancia a fallas bizantina** incluso con cierto número de nodos defectuosos.

## _Chain-key cryptography_

-   Autentica solicitudes de usuarios, estados de **_subnet_** y mensajes entre **_subnets._**
-   Las claves públicas de la **_subnet_** se respaldan en la **NNS**
-   Los usuarios necesitan la clave pública del **NNS** para la **validación.**
-   Permite cambios de membresía de **_subnet,_** seguridad proactiva, claves públicas permanentes y recolección de basura.

## Tokens y ciclos

-   ICP token es el token nativo.
-   Se puede convertir en ciclos para pagar recursos computacionales y son necesarios para el funcionamiento de los canisters.
-   Pueden intercambiarse o **bloquearse en _neurons_**.
-   Los nodos reciben compensación en este token.
