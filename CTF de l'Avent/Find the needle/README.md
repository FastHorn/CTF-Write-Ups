# Find the needle
**Points : 1500**

**Difficulté : Difficile**

**Catégorie : Programmation**

## Sources

Un de vos amis qui est très doué de ses mains a caché un message dans une aiguille du sapin de Noël. A vous de retrouver ce massage.

Les caractères du flag ont été chiffré une première fois en vigenère avec la clé CATF puis chaque caractère de ce hash a été transformé en morse.

Des threads et une optimisation du code vous seront utiles...

Le flag est de la forme CATF{message}.

La taille de "message" est de 17 caractères.

Le décalage entre chaque caractère est régulier.

Ex : Si le premier caractère du flag est dans la branche1, leaf1, needle1 et que le deuxième caractère est dans la branche2,leaf2, needle2 alors le troisième caractère sera dans la branche3, leaf3, needle3.

## Résolution

On nous fournis un dossier 'tree', contenant 50 dossier 'branch', contenant eux-même chacun 50 dossier 'leaf', contenant eux aussi 50 dossier 'needle'. Dans chaque dossier 'needle', nous pouvons retrouver un ficher contenant des caractères.
Grâce au format du flag, on sait que les caractères CATF{} devraient apparaitre en clair après être décodé du morse, comme l'on sait que le message est compris entre les crochets et que ces derniers n'ont pas d'encodage en morse, on va les utiliser pour trouver l'emplacement de notre message. On va donc créer un premier script qui va nous permettre de trouver ces positions, pour cela, on va lire chaque fichier et regarder si l'on y trouve '{' ou '}'.

```
for b,branch in enumerate(range(1,51),1):
    for l,leaf in enumerate(range(1,51),1):
        for n,needle in enumerate(range(1,51),1):
            data = open(f"tree/branch_{b}/leaf_{l}/needle_{n}/needle","r",encoding='cp1252').read()
            if (data == "{") or (data == "}"):
                print(f"branche {b}, leaf {l},needle {n}")
```

On obtient en sortie :
>branche 29, leaf 30,needle 31
>branche 47, leaf 48,needle 49

On sait donc maintenant que notre message ce trouve dans cet intervalle, de plus comme on sait que l'intervalle est régulier, il est facile de deviner où sont chaque caractères.

On va donc chercher chaqu'un de ces caractères et les décoder :
```
MORSE_DICT = {".-": "A",
          "-...": "B",
          "-.-.": "C",
          "-..": "D",
          ".": "E",
          "..-.": "F",
          "--.": "G",
          "....": "H",
          "..": "I",
          ".---": "J",
          "-.-": "K",
          ".-..": "L",
          "--": "M",
          "-.": "N",
          "---": "O",
          ".--.": "P",
          "--.-": "Q",
          ".-.": "R",
          "...": "S",
          "-": "T",
          "..-": "U",
          "...-": "V",
          ".--": "W",
          "-..-": "X",
          "-.--": "Y",
          "--..": "Z",
          "-----": "0",
          ".----": "1",
          "..---": "2",
          "...--": "3",
          "....-": "4",
          ".....": "5",
          "-....": "6",
          "--...": "7",
          "---..": "8",
          "----.": "9"}

def morsedecode(message):
    decoded = [MORSE_DICT[message]]
    return "".join(decoded)

branch = [_ for _ in range(30,47)]
leaf = [_ for _ in range(31,48)]
needle = [_ for _ in range(32,49)]

flag = ""

for i in range(17):
    data = open(f"tree/branch_{branch[i]}/leaf_{leaf[i]}/needle_{needle[i]}/needle","r",encoding='cp1252').read()
    print(f"branch {branch[i]}, leaf {leaf[i]},needle {needle[i]}")
    flag += morsedecode(data)

print(flag)
```
On obtient donc le flag suivant :
>4N0T0YB0TKY0WVH4M

Il faut maintenant le décoder du vigenère en utilisant la clé CATF

On a donc comme flag final :
>4L0T0FW0RKF0RTH4T
