---
title: "Payer pour un service Cloud"
weight: 1005
menu:
  general:
    parent: "phala-network"
---

## Introduction

Cette version est écrite par [Samuel Häfner](https://samuelhaefner.github.io/). Elle complète [Phala Supply-end Tokenomics v0.9](/en-us/general/phala-network/tokenomics/) et définit la *Demand-End Tokenomics* dans Phala Network, par exemple, comment les développeurs et les utilisateurs finaux paient pour les services informatiques. Il sera publié lorsque le contrat Phat sera mis en ligne. Les problèmes exacts abordés par cette tokenomics sont :

- Calculer les allocations de la puissance entre les clusters de contrats et leurs utilisateurs finaux ("contract deployers") ;
- L'algorithme de mise en correspondance des clusters et des workers ;
- Le mécanisme d'incitation des workers.

## Personnages principaux

| Characters                          | Description                                                                                                                                                | Types            |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------- |
| Contrat Clusters (abbr. Clusters) | Les environnements d'exécution pour des contrats spécifiques. Un Contrat Clusters est créé et géré par son propriétaire, et est soutenu par un ou plusieurs workers.             | Entité On-chain  |
| Workers                            | Les machines qui contribuent à la puissance de calcul du réseau Phala. Elles étaient auparavant appelées *miners*. Toute personne disposant du matériel approprié peut y participer. | Machine physique |
| Utilisateurs finaux                          | Les utilisateurs ou les développeurs qui déploient les Phat contracts  sur le contract cluster.                                                                             | Entité On-chain  |
| Contrats                          | Le Phat contract instances.                                                                                                                               | Entité On-chain  |

## Comment payer les services informatiques

Cette section se compose de 2 parties : la tokenomique des requêtes et la tokenomique des transactions. Elle s'applique à tous les clusters de biens publics qui sont gérés par l'équipe Phala. Dans le futur, les utilisateurs seront autorisés à créer leurs propres clusters et à définir leurs propres règles.

> Chaque contrat doit avoir un intérêt

### Requête tokenomique

Le Phat Contract utilise une tokenomique très simple pour les requêtes. Les règles peuvent être résumées ci-dessous :

- La part de puissance de calcul que reçoit un contrat sur chaque workers est égale à la part de cette puissance dans le cluster ;
- Tout le monde peut participer à n'importe quel contrat ;
- Le staking peut être ajustée ou retirée à la volée, instantanément.

On peut le résumer en une phrase :

`Le % de votre staking = Le % de la part de puissance de calcul que vous recevez`.

Habituellement, les utilisateurs finaux estiment la charge de travail de l'application, puis décident du montant à mettre en jeu pour obtenir une puissance de calcul suffisante. Cependant, sur le réseau Phala, l'estimation s'avère si délicate que l'aide d'outils supplémentaires peut être nécessaire. L'interface utilisateur du Phat Contract présentera aux utilisateurs finaux, à titre de référence, les besoins en ressources typiques de quelques applications de base.

### Transactions tokenomiques

L'objectif de la transaction tokenomique est d'atténuer les attaques DoS en facturant des frais de transaction (également appelés "gas fee"). Comme la transaction est rarement utilisée dans le Phat Contract, nous avons tendance à offrir une interface minimale aux développeurs. (Les utilisateurs interagiront principalement avec le Phat Contract par des requêtes, tandis que la transaction est utilisée pour déployer et configurer les Phat Contracts par le développeur).

Le Phat Contract adopte le même mécanisme de paiement des transactions que les contrats vanilla ink ! - lorsqu'il envoie un appel de transaction, il doit attacher des "gas fee" à la transaction.

Lors de l'exécution de la transaction dans la VM :

- Chaque instruction facturera des frais jusqu'à ce qu'elle soit terminée ou que les gas fee fournis soient épuisés.
- S'il reste des frais de gaz, la blockchain renverra le token restant à l'appelant.

Ce mécanisme peut atténuer le problème d'arrêt, en empêchant la VM de se coincer dans la boucle infinie.

![](/images/general/demand-end-tokenomics-4.png)

Dans l'implémentation actuelle, la transaction est accompagnée d'un `gasLimit` et d'un `gasPrice`. L'exécution des instructions est mesurée par le "gas", qui ne doit jamais dépasser le "gasLimit". L'utilisateur paie d'abord `gasLimite * gasPrice `, et après l'exécution, s'il reste du gas, la chaîne remboursera l'utilisateur `remainingGas & gasPrice `. Ethereum et ink ! adoptent le même mécanisme.

## Comment ça marche

Elle est différente de la [Supply-end Tokenomics](/en-us/general/phala-network/tokenomics/) qui résout principalement le problème de la manière d'inciter les workers à rejoindre le réseau pour y contribuer. La Tokenomique du Phat Contract se concentre sur l'allocation des ressources informatiques.

Voici le diagramme de structure de la demande de Phala - Phat Contract's tokenomics, vous pouvez facilement comprendre comment il fonctionne.

![](/images/general/demand-end-tokenomics-1.png)

| Layers       | Characters           | Functions                                                                                                                                                                                                                                                               |
| ------------ | -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Layer 1  | Clusters & Workers   | Répartir les ressources informatiques du réseau entre les différents clusters. Ici, Phala utilise une approche basée sur le staking.                                                                                                                                                    |
| Layer 2 | Clusters & Contracts | Les clusters allouent leur puissance de calcul aux travaux qu'ils reçoivent de leurs utilisateurs finaux. Le mécanisme d'allocation particulier à ce stade est principalement laissé aux propriétaires des clusters. Phala fournit une implémentation par défaut basée sur le staking pour les clusters de biens publics. |

### Demand-end

#### Première couche

Additionner les [scores de performance](/en-us/general/phala-network/tokenomics/#performance-test) de tous les workers du réseau en tant que $P$, où le score de performance d'un worker est proportionnel au nombre de calculs qu'il peut traiter par minute.

- Soit $P$ la puissance de calcul totale du réseau qui peut être distribuée ;
- Le réseau Phala réserve une fraction $0 ≤ α ≤ 1$ de la puissance de calcul totale $P$ aux utilisateurs finaux pour déployer des Phat contracts généraux ;
- Chaque propriétaire de cluster doit mettre en staking le PHA pour obtenir la puissance de calcul dans le réseau ;
- Le temps est divisé en ères, dans chaque ère $t$, la puissance de calcul réservée aux clusters est proportionnelle au pourcentage du montant du staking de chaque cluster dans la StakePool total ;
- La puissance de calcul réservée $P_i$ pour le cluster de contrat $i$ se traduit alors par une liste de workers spécifiques auxquels le cluster de contrat $i$ peut télécharger son code.

![](/images/general/demand-end-tokenomics-2.png)

Par exemple, supposons qu'il y ait 100 000 PHA dans la StakePool, qui sont stakés par le Cluster A (50 000 PHA), le Cluster B (30 000 PHA) et le Cluster C (20 000 PHA). Le mécanisme de la première couche allouera l'ensemble de la puissance de calcul selon un rapport de 5:3:2 aux clusters A, B et C. Ensuite, le mécanisme regroupera les workers sous forme de collections avec des scores de performance fixes qui correspondent aux besoins des clusters de manière aléatoire et assignera les collections de workers aux clusters A, B et C. Chaque worker du cluster correspondant peut obtenir la même part de puissance de calcul que le cluster auquel il appartient.

Le staking se fait par le biais d'un module de staking dédié auquel tous les titulaires de PHA peuvent contribuer. Le module peut avoir un plafond sur le montant total du staking, au-delà duquel aucun autre staking n'est accepté. Le développeur peut ajuster dynamiquement ce plafond, et :

- Un cluster peut rester financé pour toujours
- Les utilisateurs finaux peuvent retirer leurs staking à tout moment après la période de unbonding.


#### Deuxième couche

Une fois que les clusters individuels sont alloués avec leurs workers, les clusters doivent allouer les tâches de leurs utilisateurs finaux aux workers respectifs. Les propriétaires de clusters ont toute liberté pour allouer les tâches.

![](/images/general/demand-end-tokenomics-3.png)

Pour l'instant, la deuxième couche n'est pas encore totalement opérationnelle, mais Phala suggère un modèle de frais inversés sur cette couche. Selon ce modèle :

- Le cluster $i$ peut demander à ses utilisateurs de payer des PHA, des fiats ou leur propre token de cluster CTO pour utiliser ses services.
- Les utilisateurs finaux sont libres de choisir le montant, et le montant détermine le poids que le travailleur attache à un appel du contrat lors de la programmation des travaux entrants.

Les clusters peuvent conserver des listes d'utilisateurs autorisés qui peuvent appeler les contrats gratuitement. Dans tous les cas, le développeur du cluster sera en mesure de construire un ensemble de contrats de contrôle pour approuver ou refuser les demandes de déploiement et d'exécution des contrats en fonction de leurs règles individuelles.

> En tant que propriétaire de tous les clusters de biens publics, l'équipe Phala suivra le deuxième modèle dans ces clusters. Reportez-vous à la [section principale](/en-us/general/phala-network/phat-contract-fee/#how-to-pay-for-the-computing-services) pour plus de détails.


### Clusters - Matching des workers

#### Quel est le problème ?

Une fois que les clusters $i$ ont reçu leur puissance de calcul respective $P$, les workers doivent être alloués aux clusters individuels.

Pour qu'une allocation $A_t$ soit réalisable, chaque worker doit être associé à exactement un cluster.

L'algorithme de matching Cluster-to-Worker de Phala cherche à trouver parmi les allocations réalisables une allocation qui maximise la puissance de calcul allouée aux différents clusters sous réserve de leurs contraintes budgétaires.


#### Méthode de matching de base

Phala implémente un algorithme qui fournit une solution à ce problème, en prenant en compte que les clusters pourraient avoir des préférences sur les allocations satisfaisantes :

- Chaque worker doit être associé à un seul cluster.
- Maximise la puissance de calcul allouée aux différents clusters en tenant compte de leurs contraintes budgétaires.

L'algorithme se déroule comme suit :

Au début de chaque ère, chaque cluster de contrats se voit présenter une liste ordonnée de tous les workers disponibles, $L_{it} = \{\ell_{it1}, ...., \ell_{itm_t} \}$. C'est-à-dire que chaque élément de $L_{it}$ est unique et satisfait à $\ell_{itk}$. \{1,...m_t\}$. Pour chaque cluster de contrats, l'ordre de la liste est tiré au sort, mais les clusters de contrats sont libres de réorganiser la liste comme ils le souhaitent.

L'algorithme,

- Tout d'abord, il réorganise les différents clusters de contrats de manière à ce que leurs staking soient ordonnés, c'est-à-dire $S_{1t}\ge S_{2t}\ge S_{3t}\ge ...S_{n_tt}$, où les clusters de contrats ayant des stakings identiques sont ordonnés de manière aléatoire.
- Ensuite, l'algorithme parcourt les clusters de contrats dans l'ordre décroissant de leurs staking et les affecte à leurs workers préférés qui n'ont pas été affectés auparavant et qui sont encore dans leur ensemble de budget.
- **Que faire si un worker ne correspond à aucun cluster de contrat ?**
  - Comme chaque cluster de contrats a tous les workers sur sa liste, tous les clusters de contrats seront associés à certains workers et l'algorithme s'arrête dès que le cluster de contrats le moins staké se voit attribuer ses workers. Les workers qui restent non associés après cette dernière étape sont ensuite associés au cluster de contrats Phala pour les Phat contracts généraux.
- **Facteurs d'évolution**
    - Les nouveaux clusters
      - L'allocation résultante sera la même d'une ère à l'autre, si l'ensemble des workers, l'ensemble des clusters, et leurs staking ne changent pas, et si les développeurs soumettent les mêmes préférences. Si un autre cluster arrive à un moment donné, nous voulons réaffecter les workers sans changer trop radicalement l'allocation globale. La conception proposée ci-dessus garantit que plus votre mise est élevée, moins vous êtes affecté par une telle entrée.
      - En particulier, si le nouvel entrant à $t + 1$ a un staking de taille $\hat s$, alors le budget de tous les clusters restants est redimensionné, mais ce redimensionnement est d'autant moins dramatique qu'un cluster détient un staking élevé et que les clusters restants $i$ pour lesquels $s_{it} > \hat s$ obtiennent toujours leurs workers préférés (jusqu'à leur nouveau budget).
    - Les faux clusters
      - On peut également s'inquiéter du fait que des workers tentent de manipuler leur indice de popularité en enregistrant de faux clusters. L'algorithme empêche cela en ***donnant la préférence aux clusters ayant un staking plus élevé qu'à ceux ayant un staking plus faible***, rendant de telles tentatives de manipulation coûteuses. En particulier, c'est la fonction $g(.)$, abordée dans la section suivante, qui peut être utilisée pour déterminer dans quelle mesure la popularité d'un travailleur affecte sa rémunération. Ainsi, s'il y a des raisons de craindre que des manipulations se produisent, $g(.)$ peut en principe être modifié à tout moment par la gouvernance d'une fonction dont l'argument ne change pas beaucoup.


### Supply End - Côté workers

La rémunération d'un worker dépend de la sécurité du TEE, de la puissance de calcul qu'il apporte au réseau et de sa popularité.

La première est objectivement mesurable, et la seconde est déterminée sur la base des listes que les clusters soumettent à l'algorithme de mise en correspondance.

Pour améliorer la compétitivité des workers dans le système, voici plusieurs paramètres qui ont un impact sur les workers :

#### Popularité

- `Points` - nous attribuons des points à un worker en fonction des rangs qu'il prend dans ces listes. Plus précisément, nous donnons $m_t$ points au premier worker d'une liste, $m_t - 1$ au second, et ainsi de suite.
- `Listes` - Pour mesurer la popularité d'un worker dans une ère $t$, nous collectons les listes $\{{L_{it}}\}$n_t\atop i=1$ que les clusters soumettent à l'algorithme d'appariement cluster-worker.

#### Sécurité et puissance de calcul

- Nous mesurons `la sécurité d'un worker` à partir du rapport d'attestation Intel et la désignons par $*C ≥ 0*$ dans la suite, ce qui signifie qu'un $*C*$ plus élevé correspond à une plus grande confiance dans le worker.
- `La puissance de calcul d'un worker` est mesurée par la variable $*P ≥ 0*$, ce qui signifie qu'un $*P*$ plus élevé correspond à de meilleures performances.

#### Stake

Les workers qui veulent devenir actifs doivent déposer un staking $S ≥ 0$. Le staking peut dépendre de la sécurité et de la puissance de calcul d'un workers.

#### Slashing

Si un worker se comporte mal, il est sanctionné. Selon la gravité de son comportement, la sanction peut prendre différentes formes. Les spécificités du schéma de slashing sont soumises à la gouvernance. Soit $s_{it} ≥ 0$ le slash qui est appliqué au workers $i$ dans l'ère $ t$.

#### Rémunération

La rémunération des workers est suivie par une `promesse de valeur`, $V_t$ ≥ 0.

- Lorsqu'un worker devient actif, au tour $t = T$, sa valeur initiale est de zéro.
- Ensuite, à chaque round $t ≥ T$ dans lequel le Worker est encore actif, la promesse de valeur est augmentée de $*f(C, P)+g(b_{it})*$, où $*f(., .)*$ et $*g(.)*$ sont croissants dans leurs arguments respectifs.

Les formes fonctionnelles spécifiques de $*f*$ et $*g*$ sont soumises à la gouvernance.

**a.) Paiements intermédiaires**

Un workers peut toujours demander un paiement intermédiaire.

Soit $w_{it} ≥ 0$ le paiement intermédiaire du demandeur $i$ au round $t$. Le payout intermédiaire ne peut jamais être supérieur à la valeur promise dans ce round net des slashs et paiements des rounds précédents :

$w_it ≤$ $\displaystyle\sum_{τ=\underline{T}}^{t}$  $[f(C,P) + g(b_{iτ}) − s_{iτ} ] −$ $\displaystyle\sum_{τ=\underline{T}}^{t-1}$ $w_{iτ}$ .

Comme $w_{it} ≥ 0$, la condition ci-dessus implique que, dès que les slashs dépassent la valeur promise nette des versements précédents, aucun nouveau versement ne peut être demandé.

**b.) Paiement final**

Un workers peut rester dans la StakePool soit jusqu'à ce que son staking plus la promesse de valeur nette des slashs et des payouts deviennent négatifs, soit jusqu'à ce qu'il décide de partir.

C'est-à-dire qu'un workers sera exclu du StakePool au temps $T$ si et seulement si

$S +$$\displaystyle\sum_{t=\underline{T}}^{\overline{T}}$ $[f(C, P) + g(b_{it}) − s_{it} − w_{it}] ≤ 0$.

Si, en revanche, un worker décide de quitter l'ensemble des workers actifs, il reçoit un paiement final, $W$.

Si un workers part à la période $t = T$, alors le paiement final s'élève à

$W = S +$ $\displaystyle\sum_{t=\underline{T}}^{\overline{T}}$ $[f(C, P) + g(b_{it}) − s_{it} − w_{it}]$.
