---
title: "Cas d'utilisation"
weight: 1003
menu:
  build:
    parent: "phat-general"
---

Le Phat Contract est conçu pour les cas d'utilisation suivants, et n'oubliez pas : *toutes ces fonctionnalités sont fournies avec l'infrastructure sans tiers de confiance et décentralisée de Phala Network*.

## Faible latence

Services comme GameFi ou Metaverse backends
- Exécuter le serveur de jeu comme Decentraland dans les workers Phala avec peu de modifications ([catalyst](https://github.com/Phala-Network/catalyst/tree/sgx-shielded) et [catalyst-owner](https://github.com/Phala-Network/catalyst-owner/tree/sgx-shielded)).

## Intensif en calcul

Applications telles que le rendu de NFT, le machine learning et l'analyse des big data.
- Voir la [vidéo de démonstration](https://www.youtube.com/watch?v=corp9wMlkfI&t=1s) de l'exécution de [Blender](https://www.blender.org/) non modifié dans des workers Phala, avec le [code source](https://github.com/Phala-Network/blender-contract).

## Protection de la confidentialité

Scénarios tels qu'un échange décentralisé et d'autres où la confidentialité est importante.
- Découvrez notre conception et notre mise en œuvre de [secret NFT](https://github.com/tenheadedlion/phat-nft) avec Phat Contract.

![](/images/build/usecase-secret-nft.png)

## Composable

Applications, notamment d’Oracle, de bots et autres applications impliquant un ou plusieurs services Web2/Web3
- Voir la [vidéo de démonstration](https://www.youtube.com/watch?v=THeM8E-3lec) de l'utilisation de Phat Contract pour lire les états d'autres chaînes par le biais de services d'indexation.
- Suivez notre [workshop Oracle](https://github.com/Phala-Network/phat-offchain-rollup/tree/sub0-workshop/phat) et construisez un Oracle entièrement fonctionnel pour toutes les chaînes compatibles EVM ([vidéo de démonstration](https://drive.google.com/file/d/1Hg9HFEBbCiXGiyQZPKPd1Zs1BiJtP7kg/view) pour BSC).
