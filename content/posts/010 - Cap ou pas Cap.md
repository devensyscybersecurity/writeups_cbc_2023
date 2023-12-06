---
title: "Cap ou pas Cap"
date: 2023-12-06T09:03:20-08:00
draft: false
---

# Cap ou pas Cap - Cybersecurity Business Conference 2023 - *XXX pts*

## Énoncé

Récupérez les plans de la fusée dans le dossier /root! 


## Solution

Le but de ce challenge est d'exploiter les capabilities de Linux afin de récupérer le flag se trouvant dans le dossier root.

La première étape consiste à lancer la commande `getcap -r / 2> /dev/null` pour obtenir les capabilities de manière récursive.

On constate que l'exécutable "tar" possède la capability `cap_dac_search_read`.

![getcap](/images/010/01.png)

Lorsqu'un programme détient la capability `cap_dac_search_read`, cela signifie qu'il peut lire n'importe quel fichier et exécuter des permissions sur les répertoires.

Cependant, le programme ne peut pas créer de fichier dans le répertoire ni modifier un fichier existant, car cela nécessite des permissions d'écriture qui ne sont pas incluses dans cette capability.

Le flag se trouvant dans le dossier root, il est donc possible de compresser le dossier root avec la commande `tar cf root.tar /root`, puis de le décompresser avec tar xf root.tar et de lire le fichier flag.txt !

![flag](/images/010/02.png)