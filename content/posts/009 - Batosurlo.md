---
title: "Batosurlo"
date: 2023-12-06T09:03:20-08:00
draft: false
---

# Cybersecurity Business Convention 2023 - Batosurlo - *XXX pts*

## Énoncé

Le bateau chargé de récupérer le booster de notre nouvelle fusée s'est perdu en chemin, et je n'ai plus l'accès à l'interface qui permet de modifier la trajectoire, car elle n'est disponible qu'a bord.

J'ai besoin de toi pour que tu essayes de sauver ce booster, il faut absolument que le navire soit aux coordonées `6.026964, -52.133399` avant ce soir !

## Solution

L'interface web représente simplement une carte avec la position actuelle du bateau.

![Interface carte](/images/009/01.png)

Lorsqu'on regarde les calls effectués par le client pour récupérer les données de positionnement, on peut observer qu'il s'agit d'une reuqête GraphQL.

![GraphQL](/images/009/02.png)

La première étape lorsqu'on est face à un endpoint GraphQL est de vérifier que les query d'introspection sont aurorisées par le serveur. Pour cela, on envoie une requête d'introspection simple.

```js
{__schema{types{name,fields{name}}}}
```

![Test Introspection](/images/009/03.png)

L'endpoint retourne correctement les différentes query possibles, et on peut remarquer qu'une mutation `updateCoordinates` existe. Ca tombe bien, c'est exactement ce qu'on veut.

On étoffe un peu la requête d'introspection pour pouvoir récupérer les arguments requis pour l'appel de cette mutation.

```js
{__schema{types{name,fields{name,args{name,description,type{name,kind,ofType{name, kind}}}}}}}
```

![Test Introspection 2](/images/009/04.png)

On voit que la mutation `updateCoordinates` prend les arguments `lat` et `lng` qui sont tous les deux des nombres à virgule.

On peut maintenant essayer d'appeler cette mutation avec les bons paramètres.

```js
mutation updateCoordinates($lat: Float, $lng: Float){
    updateCoordinates(lat: $lat,lng: $lng)
}
```

![Test Mutation](/images/009/05.png)

Il semble que notre adresse IP n'est pas autorisée à faire cette requête depuis l'extérieur du bateau.

Lorsqu'un tel filtrage IP est en place, il peut être possible de le contourner en utilisant le header `X-Forwarded-For` souvent cru par les applications normalement déployées derrière un reverse proxy.

```
X-Forwarded-For: 127.0.0.1
```

L'application croit qu'on est connectés localement, et nous autorise l'accès !

![Test Mutation](/images/009/06.png)

*FLAG : CBC{G\*\*\*\*\*\*\*\*\*\*\*\*}*
