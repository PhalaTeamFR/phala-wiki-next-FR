---
title: "Closed Beta"
weight: 1009
draft: false
menu:
  build:
    parent: "phat-general"
---

Bienvenue au test de la Closed Beta du Phat Contract !

> Si vous n'êtes pas encore inscrit, remplissez le [formulaire de candidature](https://docs.google.com/forms/u/0/d/1LUmSQ_7B3Yh7tNCPAluBUAwhAN0HjZ3b9wragWI1Bbs) pour vous impliquer.

## Ce que vous allez découvrir

Vous serez en mesure d'utiliser un calcul off-chain sans tiers de confiance qui ne subit pas les limitations traditionnellement rencontrées par le calcul Web3.

- Phala Network apporte la décentralisation, la scalabilité et la sécurité, en plus de gérer le déploiement de votre calcul off-chain.
- Le Phat Contract fait un pas de géant en matière de capacité, un SDK serverless avec lequel vous pouvez écrire presque n'importe quoi.

Pour la première fois, vous pouvez facilement mettre en œuvre d'importants services Web3 sur une infrastructure décentralisée, comme par exemple

- Construire facilement un Oracle décentralisé ;
- Permettre aux clients de se connecter à vos DApps avec des comptes Web2 via OAuth ;
- Contrôlez vos smart contracts même s'ils se trouvent sur des chaînes différentes ;
- Lire et stocker des données sur n'importe quel service de stockage tout en restant privé ;

et bien d'autres choses à explorer ensemble.

## Table des matières

- La version [Getting Started](/en-us/build/getting-started/prep/) est préparée pour tous les nouveaux utilisateurs du Phat Contract, même si vous n'avez aucune expérience en programmation. Il n'a aucune exigence sur votre système d'exploitation ou votre environnement. Suivez notre tutoriel pour :
  - Générer le compte de test
  - Essayer la console Phat Contract
  - Déployer le contrat précompilé sur notre réseau de test.
  - Appelez le contrat, il utilisera son accès au réseau pour lire le solde du compte Ethereum pour vous.
- Le document [Build on Phat Contract](/en-us/build/stateless/intro/) vous permettra de configurer votre environnement et de compiler votre première DApp sans état avec le Phat Contract. Il couvre toutes les informations de base, y compris :
  - Configuration de l'environnement
  - Les bases du langage de programmation
  - Interaction avec l'utilisateur
  - Test unitaire local
- [Store Contract States](/en-us/build/stateful/storage-hierarchy/) présente les différents stockages que vous pouvez utiliser pour stocker vos états et données de contrat, notamment
  - Cache local volatile
  - Un stockage on-chain cohérent
  - Services de stockage externe
- [Advanced Techniques](/en-us/build/advanced/system-contract/) comprend des fonctionnalités avancées comme le contrat de système et SideVM, qui étendent encore les capacités du Phat Contract.
- [Blockchain Infrastructure](/en-us/build/infrastructure/blockchain-detail/) contient la conception du système de l'infrastructure sous-jacente qui supporte le Phat Contract. Il permet de comprendre comment votre contrat est déployé et pourquoi il est sécurisé.

## Autres ressources

- Des exemples de code comme référence
    - <https://github.com/Phala-Network/awesome-phat-contracts>
    - <https://github.com/Phala-Network/phat-contract-examples>
- Outils pour les développeurs
    - Web console: [https://phat-cb.phala.network](https://phat-cb.phala.network/)
        - Construit un front-end où vous pouvez déployer et interagir avec votre contrat.
    - Project manager
        - Obtenez rapidement un modèle vierge et créez un cluster local pour le tester avec https://github.com/l00k/devphase.
- Librairies de base
    - Support de stockage : Prise en charge de Filecoin, Arweave, Storj et Amazon S3 avec une API standard S3
        - [https://crates.io/crates/pink-s3](https://crates.io/crates/pink-s3)
    - Support des transactions inter-chaînes : Tx et requêtes d'ETH et de Substrat
        - [https://crates.io/crates/pink-web3](https://crates.io/crates/pink-web3)
        - <https://github.com/Phala-Network/phat-offchain-rollup/tree/sub0-workshop/phat/crates/subrpc>
- Librairie avancée
    - Stateful rollup : traitement sécurisé des demandes inter-chaînes
        - Permet de surveiller les demandes des contrats EVM et des chaînes basées sur substrat et d'envoyer de manière sécurisée les transactions inter-chaînes en réponse.
        - <https://github.com/Phala-Network/phat-offchain-rollup/tree/sub0-workshop>
