---
title: "SSAT"
date: 2023-12-06T09:03:20-08:00
draft: false
---

# SSAT - Cybersecurity Business Convention 2023 - *XXX pts*

## Énoncé

Nous suspectons que cette "entreprise" dissimule un lourd secret...

## Solution

![Page d'index](/images/005/01.png)

Le site web présente l'entreprise Satellite Security Corp.

Nous remarquons la présence d'une page de connexion dans la barre de navigation.

La page de connexion expose un script JavaScript obfusqué :

![Code JavaScript obfusqué](/images/005/02.png)

Il est possible de le déobfusquer de différentes manières, la plus simple étant de se rendre sur le site : [https://obf-io.deobfuscate.io/](https://obf-io.deobfuscate.io/)


![Déobfuscation du code JavaScript](/images/005/03.png)

Une fois le code JavaScript déobfusqué, il suffit d'exécuter la fonction `decodeString`.

![Déobfuscation du code JavaScript](/images/005/04.png)

La fonction nous retourne les identifiants de l'interface de connexion `admin:s&tellite`.

![Page d'administration](/images/005/05.png)

Lorsque nous saisissons une commande, nous remarquons que l'input est directement retourné dans la réponse. En essayant une charge utile de SSTI, nous constatons que le template `{{7+7}}` est bien exécuté :

![SSTI Test Heuristique](/images/005/06.png)

Nous pouvons donc essayer d'exécuter des commandes à l'aide du payload suivant :
```python
{{ self._TemplateReference__context.cycler.__init__.__globals__.os.popen('id').read() }}
```

![SSTI Exécution de code](/images/005/07.png)

Le fichier flag se trouve à la racine du serveur `/flag.txt` :

![Flag](/images/005/08.png)

*FLAG : CBC{f********}*