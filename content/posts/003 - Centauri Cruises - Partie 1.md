---
title: "Centauri Cruises"
date: 2023-12-06T09:03:20-08:00
draft: false
---

# Cybersecurity Business Convention 2023 - Centauri Cruises - *100 pts*

## Énoncé

Cette nouvelle agence de voyage spacial propose un tarif encore jamais vu auparavant ! Mais c'est quand même toujours hors de prix... Tu pourrais pas me trouver un moyen de dégoter un ticket quand même ?

J'ai vu qu'ils faisaient une opération promo en ce moment avec le code promo "HALFOFF", mais c'est toujours trop cher pour moi..

## Solution
Le site internet de la compagnie semble plutôt classique : Une page de présentation, une page pour annoncer le tarif, et une page de checkout.

![Site Centauri 1](/images/003/01.png)

![Site Centauri 2](/images/003/02.png)

![Site Centauri 3](/images/003/03.png)

Effectivement le tarif pique un peu.

Si j'essaye de payer, le site me fait clairement comprendre que c'est hors de mes moyens.

![Broke](/images/003/04.png)

Je commence par tester le code promo fourni dans l'enoncé.

![Code promo 1](/images/003/05.png)

![Code promo 2](/images/003/06.png)

Mais il semble que je n'ai toujours pas les moyens.

En sachant que des codes promo sont disponibles, je peux peut être en essayer d'autres. 

Après plusieurs essais, j'essaye le code `TEST`, très souvent oublié sur les sites d'e-commerce peu fréquentés.

![Code test 1](/images/003/07.png)

Le code `TEST` fait tomber le prix à 0, ce qui nous permet de "payer", et d'obtenir un ticket !

![Code test 2](/images/003/08.png)
![Page de récupération du ticket](/images/003/09.png)

On télécharge le ticket en PDF, et on obtient le flag de cette première partie.

![Ticket PDF](/images/003/10.png)

*FLAG : CBC{A\*\*\*\*\*\*\*\*\*\*\*\*}*
