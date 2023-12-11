---  
title: "Only One Time"  
date: 2023-12-06T09:03:20-08:00  
draft: false  
---  
  
# Only One Time - Cybersecurity Business Convention 2023 - *200 pts*  
  
## Énoncé du Défi  
  
Vous travaillez pour une agence spatiale et votre tâche consiste à déchiffrer une série de messages codés interceptés depuis une autre planète.  

Voici quelques mots qui ont été identifiés :  END / TO KNOW / NOBODY

Message 1 --> 170a0a28633b3a4205522c22343a2f200c5303333c345934382b2e
Message 2 --> 0d0d0134072b4935053c354733264e2c073c124127313c593c3739

## Solution

Nous utilisons le Crib Dragging, les deux messages sont chiffrés avec la même clé : https://toolbox.lotusfa.com/crib_drag/. En entrant les mots déjà identifiés, nous pouvons extraire les mots suivants :

END ---> ARS

TO KNOW ---> SSAGE F

NOBODY ---> THIS I

Nous pouvons distinguer que "SSAGE F" pourrait être "MESSAGE FROM", "ARS" pourrait etre "MARS"

T TO KNOW THE END --> MESSAGE FROM MARS  

Il manque que THIS IS. IL suffit de le rajouter au début et nous avons le message : 

"THIS IS A MESSAGE FROM MARS"

En effectuant une opération XOR du message avec le message chiffré, nous pouvons obtenir le flag :

```python
def hex_xor(hex1, hex2):  
    return ''.join([chr((int(hex1[i:i+2], 16) ^ int(hex2[i:i+2], 16))) for i in range(0, len(hex1), 2)])  
  

message = "THIS IS A MESSAGE FROM MARS"  
c_message = "170a0a28633b3a4205522c22343a2f200c5303333c345934382b2e"
key = hex_xor(message.encode('utf-8').hex(), c_message)
  
print(key)   
```

*FLAG : CBC{C********}*