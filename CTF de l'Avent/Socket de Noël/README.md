# Socket de Noël
**Points : 350**

**Difficulté : Facile**

**Catégorie : Programmation**

## Sources

Ta famille t'a fait une liste de choses qu'elle voudrait pour Noël, tu as tout réuni et as noté les objets avec leur prix sur des bouts de papier.

Parce que tu n'as pas un budget extensible et pour ne pas faire de jaloux tu mets ces bouts de papier dans un chapeau et tu tires au sort différents objets que tu devras acheter.

Comme tu es très fort en calcul mental tu dois donner ce que coutera cette liste en moins de 2 s.

Bon courage.

## Résolution

Dans ce challenge il nous est fourni plusieurs instances auxquelles nous pouvons nous connecter. Dans un premier temps on va simplement chercher à afficher la liste qui nous est envoyé. Pour cela nous utilisons ce script python :

```
from pwn import *
import re

ip = '0.cloud.chals.io'
port = 22243
 
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((ip, port))

total = 0

while True:
    data = s.recv(1024).decode()
    print(data)
```

On observe un message du père noël ainsi qu'une liste de commandes, on remarque également que cette liste de commandes nous est envoyé en deux fois, il faudra donc prévoir un système permettant d'additionner les montants de ces deux parties de liste.

Pour trouver tout les montants, on va utiliser une regex dans laquelle on va spécifier que l'on ne désire uniquement prendre les nombre avant le symbole '€'.

Puis nous allons renvoyer le total des prix. Nous obtenons donc le script python suivant :
```
from pwn import *
import re

ip = '0.cloud.chals.io'
port = 22243
 
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((ip, port))

total = 0

while True:
    data = s.recv(1024).decode()
    print(data)


    prices = re.findall(r'\d+€', data)
    prices = [price.replace('€', '') for price in prices]
    print(prices)
    total += sum(map(int, prices))
    print(total)
    s.sendall((str(total) + '\n').encode())
```
On peut observer par la suite dans notre console :
![result](https://github.com/FastHorn/CTF-Write-Ups/blob/main/CTF%20de%20l'Avent/Socket%20de%20No%C3%ABl/result.PNG)



