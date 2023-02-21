---
title: "Introduction"
weight: 1001
draft: false
menu:
  build:
    parent: "phat-general"
---

## Qu'est-ce que le Phat Contract

Le Phat Contract est un modèle de programmation innovant permettant la [*Computation Off-Chain*](https://medium.com/phala-network/fat-contract-introduce-off-chain-computation-to-smart-contract-dfc5839d5fb8). Il est également connu sous le nom de Fat Contract en tant que pratique du concept "Fat Protocol & Thin Application", ainsi que pour ses riches fonctionnalités par rapport aux smart contracts existants. Le Phat Contract utilise le langage [ink !](https://paritytech.github.io/ink/) basé sur Rust .

> **Indication**
>
> Chaque Phat contract est naturellement un programme *distribué* puisqu'il comporte plusieurs instances réparties entre tous les workers d'un cluster. Toutes ces instances peuvent traiter les demandes des utilisateurs simultanément, mais elles peuvent aussi causer des problèmes lorsqu'elles essaient de mettre à jour l'état du contrat en même temps.
>
> C'est pourquoi nous recommandons aux débutants de commencer par construire des applications sans état, qui sont exemptes des problèmes ci-dessus.

![](/images/build/phat-offchain-comp.png)

Bien que portant le nom de "contrat", le Phat Contract ressemble davantage à la version Web3 d'[Amazon Lambda](https://aws.amazon.com/lambda/) soutenue par le cloud computing décentralisé de Phala et destinée à supporter des calculs complexes avec une faible latence et un faible coût.

Vous pouvez facilement déployer votre Phat contract sur plusieurs workers du réseau Phala, chaque instance traitant les demandes des utilisateurs en parallèle. Le contrat est exécuté dans Secure Enclave, et tous ses états et entrées/sorties sont chiffrés de manière transparente.

Le Phat Contract n'est pas destiné à remplacer les smart contracts, il tente plutôt d'être l'unité de calcul décentralisée manquante pour eux.
Par exemple, au lieu de mettre en œuvre un token [ERC-20](https://ethereum.org/en/developers/docs/standards/tokens/erc-20/) avec le Phat Contract (dont le solde doit être stocké on-chain), nous recommandons de déployer votre contrat ERC-20 sur Ethereum et d'utiliser Phat Contract pour le faire fonctionner.


## Quand en aurez-vous besoin ?

> Lorsqu'un seul smart contract n'est pas suffisant pour votre DApp, implémentez le reste de la logique avec Phat Contract.

Étant donné que le stockage et l'exécution on-chain peuvent être coûteux (d'un point de vue monétaire et de performance), il est raisonnable de garder le contrat on-chain petit et compact et de mettre en œuvre l'autre logique ailleurs. Les DApps existantes implémentent généralement leur propre logique backend comme des programmes normaux et les déploient vers des services centralisés comme AWS (Amazon Web Services).

![](/images/build/web2-stack.png)

Avec le service computing de Phala et son Phat Contract, vous pouvez exécuter vos programmes backend sur une infrastructure décentralisée en toute confidentialité, avec des performances et à faible coût.

![](/images/build/web3-stack.png)
