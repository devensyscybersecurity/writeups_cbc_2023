---
title: "QR or not QR"
date: 2023-12-06T09:03:20-08:00
draft: false
---

# Cybersecurity Business Convention 2023 - QR or not QR - *200 pts*

## Énoncé

QR or not QR... that is the question.

*Fichier attaché : `QR.png`*

## Solution

![Fichier original](/images/002/01.png)

Le fichier fourni ressemble beaucoup à un code QR, à quelques détails près : 
 - Il n'y a pas les 3 détrompeurs aux 3 coins de l'image
 - L'image semble scindée au milieu
 - Il y a des motifs répétés (deux carrés allignés verticalement)
 - Et surtout, **il n'est pas carré !**

Avec quelques recherches autour des spécificités de ce format un peu particulier, on réussit à trouver un nouveau standard qui est plutot ressemblant : `rMQR`

`rMQR` est une alternative au code QR développée par **DENSO** (les mêmes qui ont fait QR) destinée à être utilisée sur des objets oblongs ou verticaux, là ou un code QR serait inadapté en raison de sa forme carrée. Il fait "concurrence" aux codes barres classiques.

![Standard rMQR](/images/002/02.jpg)

Cependant, si l'on compare un rMQR classique avec le fichier fourni, on peut constater quelques différences : 
 - Le pattern d'alignement (3) n'est pas au bon endroit, et s'arête au milieu de l'image
 - Le pattern de timing s'arrête également au milieu de l'image, alors qu'il devrait en faire le tour.

Grâce à ces indices, on peut en déduire que le `rMQR` est simplement coupé en deux et que les deux morceaux sont collés verticalement.

![Deux parties](/images/002/03.png)

Il suffit alors de prendre ces deux morceaux et de les coller horizontalement pour retrouver le `rMQR` original.

![Standard rMQR](/images/002/04.png)

Ensuite, on peut lire le contenu avec l'application `QRQR Reader` sur Android.

![Standard rMQR](/images/002/05.png)


*FLAG : CBC{I\*\*\*\*\*\*\*\*\*\*\*\*}*
