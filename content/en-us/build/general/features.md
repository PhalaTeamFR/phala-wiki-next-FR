---
title: "Fonctionnalités"
weight: 1002
draft: false
menu:
  build:
    parent: "phat-general"
---

## Comparaison avec les smart contracts

Les solutions existantes en matière de smart contracts présentent de nombreuses limites :

- Latence élevée avec un nombre limité d'instructions à exécuter ;
- Aucun support de base de données ;
- Aucun accès au réseau ;
- Peu de librairies ;

Le Phat Contract est destiné à être l'unité de calcul manquante pour les smart contracts existants, de sorte que vous n'avez plus besoin de déployer vos programmes backend sur un cloud centralisé.

![](/images/general/fat-features.jpeg)

Le Phat Contract hérite de la nature auto-renforçante et sécurisé des smart contracts tout en introduisant plus d'avantages, notamment :

- Préservation de la vie privée avec la performance. Vous pouvez stocker et traiter vos données confidentielles en toute sécurité dans le Phat Contract, car elles sont protégées par un cryptage matériel tout au long de leur cycle de vie ;
- Latence inexistante, frais de gaz inexistants. Les interactions avec le Phat Contract ne peuvent impliquer aucune transaction on-chain, ce qui permet d'obtenir des réponses en lecture et en écriture au niveau de la milliseconde, sans frais de gaz ;
- Connectivité avec les requêtes HTTP. Le Phat Contract supporte nativement les requêtes HTTP. Utilisez-le pour connecter n'importe quel service Web2 existant pour stocker des données et construire un Oracle, ou un nœud RPC d'autres blockchains pour des opérations inter-chaînes simples et sûres ;
- La liberté d'utiliser les librairies de l'écosystème Rust. Rédigez votre contrat avec le [language ink !](https://paritytech.github.io/ink/) basé sur Rust et utilisez des librairies avec le support `no_std`. Nous supporterons `std` dans la future version du Phat Contract et vous pourrez alors utiliser toutes les librairies que vous souhaitez.

## Comparaison avec les services sans serveur Web2

Le Phat Contract offre les mêmes fonctionnalités que nos homologues Web2 mais ouvre davantage de possibilités grâce à sa nature Web3.

- Exécution forcée de programmes à usage général. L'exécution forcée est la fonctionnalité principale des smart contracts : les développeurs ne peuvent pas altérer ou arrêter leurs programmes après le déploiement, ce qui renforce la confiance. Avec le Phat Contract, nous apportons cela à des programmes plus généralistes. Ne sous-estimez jamais cela car c'est la raison pour laquelle les crypto-monnaies peuvent avoir de la valeur ;
- Infrastructure décentralisée et sans tiers de confiance. La conception de notre infrastructure est totalement publique et tout son code est disponible pour que vous puissiez le vérifier. Traiter vos données sensibles dans le Phat Contract n'implique aucun tiers de confiance dans l'équipe Phala, mais dans le code et le matériel basé sur Secure-Enclave ;
- Intégration plus simple avec d'autres blockchains. Nous fournissons des modèles de contrat pour des interactions simples et sécurisées avec les blockchains existantes. De plus, vous pouvez déléguer en toute sécurité les comptes de votre chaîne au Phat Contract, qui ne craint pas les pertes de confidentialité ;
- Un écosystème de contrats libres. La différence la plus typique entre les contrats et les programmes Web2 est qu'ils sont naturellement publics : vous êtes libre d'appeler n'importe quel contrat existant pour composer vos propres applications avec un minimum d'effort.

<!-- ## Quelles sont les nouveautés ?

Par rapport à sa version précédente, le dernier Phat Contract évolue également sur les aspects suivants :

- Support de la fonctionnalité HTTP Request dans les contrats ink ! Précédemment, nous avons montré que nous pouvons exécuter des contrats ink ! non modifiables dans les Secure Workers de Phala. Mais pour utiliser la fonctionnalité de requête HTTP, un développeur doit forker la base de code phala-blockchain et écrire le contrat natif. Dans la nouvelle version, nous supportons la fonction de requête HTTP dans le contrat ink ! et en faisons un contrat ink ! [extension](https://crates.io/crates/pink-extension). Elle fournit des requêtes HTTP et d'autres fonctionnalités liées à la cryptographie pour ink ! contract ;
- Testnet devient opérationnel. Auparavant, nos développeurs devaient gérer un testnet local pour le développement des contrats, ce qui pouvait prendre beaucoup de temps. Maintenant, nous avons activé le [Phala Testnet (PoC 5)](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpoc5.phala.network%2Fws#/explorer), de sorte que le développement du contrat peut être plus facile ;
- Utilisez un Phat Contract pour exécuter des programmes x86 non modifiés. Nous avons présenté une [démo](https://github.com/Phala-Network/blender-contract) permettant d'utiliser Phat Contract pour exécuter le moteur de rendu non modifié Blender avec l'aide du [projet Gramine](https://github.com/gramineproject/gramine). Cela signifie que le service de rendu public décentralisé est en route. Cela prouve également le potentiel de Phat Contract à exécuter des programmes complexes du monde réel. -->
