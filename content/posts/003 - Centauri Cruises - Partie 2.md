---
title: "Centauri Cruises (2)"
date: 2023-10-24T09:03:20-08:00
draft: false
---

# Cybersecurity Business Convention 2023 - Centauri Cruises (2) - *300 pts*

## Énoncé

Cool, on dirait que je vais pouvoir partir en orbite gratos !

C'est même possible qu'avec un peu de chance tu puisses venir avec moi, leur système de vérification des tickets me semble un peu bancal... ;)

## Solution

![Ticket PDF](/images/003/11.png)

On reprend à partir du ticket qu'on a réussi à générer, et on commence par observer ce que contient le QRCode qui doit sans doute servir à la vérification du billet par la compagnie.

![QR Decode](/images/003/12.png)

Le QRCode semble rediriger vers un nouvel endpoint API `/api/verifyTicket`. Visiter le lien avec les paramètre fournis nous permet de valider directement notre ticket.

![Ticket is valid](/images/003/13.png)

On constate que deux paramètres sont envoyés dans cette requête : `id` qui semble être le même identifiant que celui qui est demandé par l'endpoint `getTicket`, ainsi que `server`, qui semble donner à l'application l'adresse du serveur de vérification.

Si l'on joue avec le paramètre `id`, on constate que une valeur différente de l'identifiant original est toujours refusée.

![Invalid Ticket](/images/003/14.png)

En revanche, le paramètre `server` est potentiellement beaucoup plus intéressant, car il pourrait potentiellement nous permettre de réaliser une attaque SSRF, et ainsi de récupérer la requête envoyée par l'application au serveur de vérification.

Cependant, une whitelist de noms est en place, ce qui nous empêche de contacter n'importe quel domaine.

![Unauthorized server](/images/003/15.png)

Le serveur contacté par défaut est `verif:51001`, donc on peut tenter d'insérer le nom du serveur quelque part dans l'URL que l'on veut atteindre, mais tout en gardant le contrôle sur le domaine qui est requêté.

Par exemple, on peut essayer de le mettre après le premier `/` qui suit le nom de domaine.

```
google.com/verif
```

![Request Error](/images/003/16.png)

Le message a changé ! Ici `Request error` veut potentiellement dire que le serveur a tenté de joindre notre URL, mais qu'il n'a pas réussi, ou que l'endpoint en question a répondu quelque chose d'inatendu.

On peut set up un Beeceptor / RequestBin / serveur web python pour voir si la requête est bien envoyée et voir son contenu.

```
xxx.free.beeceptor.com/verif
```

![Ticket is valid](/images/003/17.png)

Et bingo, on a bien une requête qui est reçue.

![Beeceptor](/images/003/18.png)

Au premier abord, on ne voit pas grand chose, simplement une requête GET avec l'identifiant du ticket.

Mais si on regarde les headers, on peut récupérer l'authentification qui passe entre l'application et le serveur de vérification !

![Flag in header](/images/003/19.png)

*FLAG : CBC{S\*\*\*\*\*\*\*\*\*\*\*\*}*
