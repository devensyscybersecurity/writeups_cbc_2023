---
title: "EZCrypto"
date: 2023-12-06T09:03:20-08:00
draft: false
---

# EZCrypto - Cybersecurity Business Convention 2023 - *100 pts*

## Énoncé

Lors d'une mission d'espionnage, notre agent a intercepté un message crypté qui contient des informations vitales. Selon nos renseignements, ce message révèle le nom du prochain site de lancement de fusée.

```
0001 000 100 0010 0000 0110 010 1001 1101 011 100 0100 1101
```


## Solution

Le message chiffré que nous avons reçu est en binaire. Chaque groupe de chiffres représente un caractère en Morse, où 0 est . et 1 est -. Après avoir converti le message en Morse, nous obtenons la chaîne suivante :

```
...- ... -.. ..-. .... .--. .-. -..- --.- .-- -.. .-.. --.-
```

Ce message en Morse correspond à la chaîne "vsdfhprxqwdlq" en ASCII. Cependant, notre espion nous a informé qu'il avait dû utiliser un chiffrement de César avec un décalage de 3 pour éviter que le message ne soit intercepté.

En appliquant un décalage de ```<- 3``` à chaque lettre de la chaîne "vsdfhprxqwdlq", nous obtenons le flag "spacemountain".

*FLAG : CBC{s********}*