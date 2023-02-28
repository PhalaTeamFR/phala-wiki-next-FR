---
title: "Récompenses pour les Workers"
weight: 1006
menu:
  general:
    parent: "phala-network"
---

Ce document contient la *Tokenomics Supply-end* pour le Phala Network, qui définit comment les workers obtiennent leurs récompenses en partageant la puissance de calcul.

> Après l'approbation du référendum démocratique "Gemini Tokenomics upgrade" sur la hauteur de bloc #1,467,069, nous avons mis à jour le contenu de la Tokenomics Supply-end de la manière suivante :

## Objectifs de la conception

La conception économique globale est conçue pour répondre à ces points :

1. Soutenir l'architecture du cloud computing sans tiers de confiance (trustless) de Phala Network.
   - Séparation par consensus et calcul
   - Les workers informatiques linéairement extensibles (nombre de workers d'un ordre de grandeur de 100k)
2. Inciter les workers à rejoindre le réseau
   - Garantir le paiement de l'électricité fournie indépendamment de la demande, notamment au démarrage du réseau.
   - Subventionner le pool minier avec 70% de l'offre initiale au fil du temps
   - Calendrier de réduction de moitié du budget similaire à celui du bitcoin (halving)
   - Alimenter le Phala et le Khala en même temps
3. Tarifs spécifiques pour les applications
4. Performance On-chain

Les paragraphes suivants détaillent certains éléments clés du modèle économique.

## Conception générale

### Workers associés

Phala Supply-end Tokenomics s'applique à tous les workers fonctionnant sur Phala ou Khala.

### Promesse de valeur ($V$)

- Un score virtuel pour un worker individuel représentant la valeur gagnée qui est payable dans le futur, pour motiver les workers à se comporter de manière honnête et fiable
- Égal à la valeur attendue des revenus gagnés par le worker pour fournir de l'énergie à la plate-forme
- Change de façon dynamique en fonction des comportements des workers et du remboursement des récompenses
  - Exploitation honnête : $V$ croît progressivement au fil du temps
  - Comportement nuisible : puni par une réduction de $V$.

### $V$ Initial

Un Worker exécutera un **_Test de performance_** et mettra en staking quelques jetons pour obtenir le $V$ initial :

$$V^e = f(R^e, \text{ConfidenceScore}) \times (S + C)$$

- $R^e > 1$ est un **_Multiplicateur de staking** fixé par le réseau (Khala ou Phala).
- $S$ est le staking du worker; un **_Minimum Stake_** est requis pour commencer le minage. Le staking ne peut pas être augmentée ou diminuée pendant le minage, mais peut être fixé plus haut que le minimum.
- $C$ est le coût estimé des Rigs des workers, déduit du **_Test de performance_**.
- $\text{ConfidenceScore}$ est basé sur les capacités Intel© SGX du worker's.
- $f(R^e, \text{ConfidenceScore}) = 1 + (\text{ConfidenceScore} \cdot (R^e - 1))$
- $V$ est toujours inférieur ou égal à $V_{max}$.

Paramètres utilisés dans la simulation :

- $R^e_{\text{Phala}} =R^e_{\text{Khala}} = 1.5$
- $\text{ConfidenceScore}$ pour différents [niveaux de confiance](/en-us/mine/solo/1-2-confidential-level-evaluation/#confidence-level-of-a-worker)
  - $\text{ConfidenceScore}_{1,2,3} = 1$
  - $\text{ConfidenceScore}_{4} = 0.8$
  - $\text{ConfidenceScore}_{5} = 0.7$
- $V_{max} = 30000$

### Test de performance

Un test de performance mesure la quantité de calculs pouvant être effectués dans une unité de temps :

$$P = \frac{\text{Iterations}}{\Delta t}$$

Pour référence,

| Plate-forme               | Cores | Score | Prix approximatif |
| ---------------------- | ----- | ----- | ----------------- |
| Low-End Celeron        | 4     | 450   | $150              |
| Intel Xeon E Processor | 6     | 1900  | $500              |
| Mid-End i5 10-Gen      | 8     | 2000  | $500              |
| High-End i9 9-Gen      | 10    | 2800  | $790              |

> Le tableau est basé sur la version en cours de rédaction de cette documentation et est sujet à des changements.

Le test de performance sera effectué :

1. **Avant le minage** pour déterminer le **_Minimum Stake_**.
2. **Pendant le minage** pour mesurer la performance actuelle, et pour ajuster l'incrément de $V$ dynamiquement.

### Minimum Stake

$$S_{min}=k \sqrt{P}$$

- $P$ - **_Test de performance_** score
- $k$ - facteur multiplicateur ajustable

Paramètre proposé :

- $k_{\text{Phala}} =k_{\text{Khala}} = 50$

> L'état verrouillé (Locked) du jeton $PHA peut également être utilisé pour le staking du minage, par exemple, la récompense Khala Crowdloan.

### Coût

$$C = \frac{0.3 P}{\phi}$$

- $\phi$ est la cotation actuelle du PHA/USD, mise à jour dynamiquement sur la chaîne par les Oracles.
- $P$ est le score initial du **_Test de performance_**.
- Dans les premières étapes, nous compensons le coût de l'équipement $C$ par une promesse de valeur plus élevée.
- A l'avenir, nous prévoyons de compenser les coûts d'amortissement plus élevés (en ajoutant le coût d'amortissement de l'équipement aux coûts de fonctionnement $c^i$ et $c^a$), augmentant ainsi la vitesse de croissance du $V$ du Worker's.

### Processus général du minage

![](https://i.imgur.com/IpEnlGR.png)

![](https://i.imgur.com/zKWAI1S.png)

Les $V$ de chaque personne sont mis à jour à chaque bloc :

- Augmenté de $\Delta V_t$ si le worker continue de miner.
- Diminué de $w(V_t)$ si le worker a reçu un paiement.
- Diminué selon les **_Règles de Slash_** si le worker se comporte mal.

Lorsqu'un worker reçoit un paiement $w(V_t)$, il reçoit le montant immédiatement dans son wallet Phala. Le paiement suit le **_Calendrier des paiements_** et ne peut pas dépasser le **_Budget des subventions_**.

Enfin, une fois que le worker décide d'arrêter de miner, il attendra une période de Cooling Down (refroidissement) de $\delta$. Il recevra un paiement final unique après la période de Cooling Down.

| Numéro de bloc  |     $t$      |    $t+1$     | $\dots$ |     $T$      |             $\dots$             |       $T+\delta$        |
| :------------ | :----------: | :----------: | :-----: | :----------: | :-----------------------------: | :---------------------: |
| Promesse de valeur |    $V_t$     |  $V_{t+1}$   | $\dots$ |    $V_T$     |             $\dots$             |         $\dots$         |
| Paiement       |   $w(V_t)$   | $w(V_{t+1})$ | $\dots$ |   $w(V_T)$   |               $0$               | $\kappa \min(V_T, V^e)$ |
|               | Récompense de bloc |     ...      |   ...   | Récompense de bloc | Cooling off pour les blocs $\delta$ |      Paiement final       |

Paramètre proposé :

- $\delta = \text{blocs équivalents à 7 jours}$

### Mise à jour de $V

Quand il n'y a pas d'événement de paiement ou de slashing:

$$\Delta V_t = k_p \cdot \big((\rho^m - 1) V_t + c(s_t) + \gamma(V_t)h(V_t)\big)$$

- $\rho^m$ est le facteur d'accroissement inconditionnel $V$ du worker
- $c(s_t)$ est le coût opérationnel pour faire fonctionner le worker
- $\gamma(V_t)h(V_t)$ représente un facteur de compensation pour les slashing accidentelles/non intentionnelles (ignoré dans les tableaux simulés)
- $k_p = \min(\frac{P_t}{P}, 120\\%)$, où $P_t$ est le score de performance instantané, et $P$ est le score initial
- If $V > V_{max}$ après la mise à jour, il sera plafonné à $V_{max}$

Paramètres proposés :

- $\rho^m_{\text{Phala}} =\rho^m_{\text{Khala}} = 1.00020$ (par heure)

### Événement de paiement

Afin de respecter le budget des subventions, à chaque bloc, le budget est distribué proportionnellement sur la base des **_Participations des Workers_** actuelles :

$$w(V_t) = B \frac{\text{share}}{\Sigma \text{share}}$$

où $B$ est le budget actuel de subvention du réseau pour la période de paiement donnée.

Chaque fois que $w(V_t)$ aura été payé au worker, son $V$ sera mis à jour en conséquence :

$$\Delta V = -min(w(V_t),V_t-V_\text{last}).$$

$V_\text{last}$ est la valeur promise lors du dernier paiement, ou $V^e$ si c'est le premier paiement.

> La mise à jour de $V$ est limitée pour s'assurer que le paiement n'entraîne pas une baisse de $V$ par rapport au dernier événement de paiement. La limite est nécessaire pour s'assurer que les workers sont bien incités à toujours accumuler des crédits dans le réseau. Sinon, les workers sont incités à réinitialiser constamment leur session de minage si $V$ diminue au fil du temps.

Share représente combien le worker est payé à partir de $V$. Nous nous attendons à ce qu'il se rapproche de la base de référence de la part, mais avec des ajustements mineurs pour refléter la propriété du worker :

$$\text{share}_{\text{Baseline}} = V_t.$$

$\Sigma \text{share}$ contient la part des workers qui fonctionnent sur Phala ou Khala avec le même ratio de subvention.

Algorithme proposé :

- $\text{share}_{\text{Khala}} = \sqrt{V_t^2 + (2 P_t \cdot \text{ConfidenceScore})^2}$
- $\text{share}_{\text{Phala}} = \sqrt{V_t^2 + (2 P_t \cdot \text{ConfidenceScore})^2}$
- $P_t$ est le score de performance instantané

### Budget des subventions

|                    |  Phala / Khala   |
| ------------------ | :--------------: |
| Relaychain         | Polkadot/ Kusama |
| Budget pour le Mining  |     700 mln      |
| Période de Halving     |     180 days     |
| Réduction du Halving   |       25%        |
| Partage de la trésorerie       |       20%        |
| Récompense du premier mois |     21.6 mln     |


### Heartbeat & Calendrier des paiements

Dans n'importe quel bloc $t$, si le VRF du Worker's est plus petit que son seuil actuel de Heartbeat $\gamma(V_t)$, il doit envoyer la transaction Heartbeat à la chaîne, qui mettra à jour l'enregistrement on-chain de sa promesse de valeur et enverra une récompense de minage $w(V_t)$ à son wallet de récompense :

$$\Delta V_t = - w(V_t).$$

S'ils n'envoient pas la transaction Heartbeat à la chaîne dans la fenêtre de défi, la mise à jour de leur promesse de valeur sera

$$\Delta V_t = - h(V_t),$$

et leur statut est changé en _unresponsive_, et ils seront punis de manière répétée jusqu'à ce qu'ils envoient un heartbeat, ou arrêtent de miner. Le montant du slash $h$ est défini dans la section **_Slash_**.

L'objectif est de traiter environ 20 challenges heartbeat par bloc. La probabilité de défi de heartbeat $\gamma(V_t)$ sera ajustée pour cibler ce nombre de challenges.

Paramètres potentiels :

- $\text{ChallengeWindow} = 10$ (blocs)

### Règles de Slash

Les règles relatives au slash pour les workers sont définies ci-dessous. Aucune règle de slash n'a été mise en œuvre pour le moment, mais elle le sera dans un avenir proche.

| Gravité | Défaut                               | Punition                                |
| -------- | ----------------------------------- | ----------------------------------------- |
| Niv. 1   | Worker hors ligne                      | 0.1% $V$ par heure (déduit bloc par bloc) |
| Niv. 2   | Bonne volonté avec mauvais résultat          | 1% de $V$                                  |
| Niv. 3   | Intention malveillante ou erreur collective     | 10% de $V$                                |
| Niv. 4   | Risque sérieux pour la sécurité du système | 100% de $V$                               |

### Paiement final

Lorsqu'un worker choisit de se déconnecter de la plateforme, il envoie une transaction de sortie et reçoit son indemnité de départ après $\delta$ blocs.

Après la période de cooling down, le worker reçoit son paiement final, représentant le retour de la mise initiale. Cependant, si $V_T$ est inférieur au $V^e$ initial, le worker recevra moins de récompense de staking en guise de punition :

$$w(T + \sigma) = \min(\frac{V_T}{V^e}, 100\\%) \cdot S$$

où $S$ est la mise initiale.
