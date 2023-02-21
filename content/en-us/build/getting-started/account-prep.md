---
title: "Générer un compte de test"
weight: 2002
menu:
  build:
    parent: "phat-basic"
---

<!-- Nous recommandons d'utiliser [Phala Testnet](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpoc5.phala.network%2Fws#/explorer) pour les tests contractuels. -->

Pour déployer votre contrat sur n'importe quel testnet, vous devez :
-  générez votre propre compte de test ;
-  transférez quelques tokens de test vers celui-ci afin de vous assurer que vous disposez d'un solde suffisant pour payer les frais de transaction de déploiement.

Dans le tutoriel suivant, nous utilisons l'extension [Polkadot.js](https://polkadot.js.org/extension/) comme wallet par défaut. D'autres wallets sont disponibles [ici](/en-us/general/applications/extension-wallet/).

<!-- ## Configurez votre compte de test -->

## Créer un compte de test avec Polkadot.js

> N'utilisez jamais vos comptes personnels pour effectuer des tests en cas de pertes financières inattendues.

1. Installez [l'extension Polkadot.js](https://polkadot.js.org/extension/) ;
2. Choisissez votre extension Polkadot.js installée ;
3. Cliquez sur "+", et choisissez "Créer un nouveau compte" ;

<p align="center">
  <img src="/images/docs/khala-user/new-account.png" width="400"/>
</p>

4. Gardez la seed mnémonique en sécurité ;
5. Choisissez le réseau "Autoriser l'utilisation sur n'importe quelle chaîne", remplissez le nom descriptif et le mot de passe, et cliquez sur "Ajouter le compte avec la seed générée".

<p align="center">
  <img src="/images/docs/khala-user/choose-network.png" width="400"/>
</p>

> Vous pouvez vous référer au [tutoriel officiel](https://wiki.polkadot.network/docs/learn-account-generation#polkadotjs-browser-extension) Polkadot.js pour plus d'utilisations.

<!-- ### Transfert des tokens de test

Le solde de tous les comptes est disponible sur l'application [Polkadot.js](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpoc5.phala.network%2Fws#/accounts).

1. Envoyez des tokens depuis n'importe quel compte de développement ;

![](/images/build/tutor-transfer.png)

2. Choisissez votre compte dans la rubrique "envoyer à l'adresse" et tapez le montant (1000 est plus que suffisant) ;

![](/images/build/tutor-choose-to.png)

3. Cliquez sur "Effectuer le transfert" ;
4. Vérifiez le solde de votre compte dans la page [Comptes](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fpoc5.phala.network%2Fws#/accounts). -->
