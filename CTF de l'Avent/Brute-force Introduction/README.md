# Brute-force Introduction
**Points : 350**

**Difficulté : Facile**

**Catégorie : Programmation**

## Sources

Un lutin vous a transmis un message cryptique ainsi qu'une notice, celle-ci indique :
> Chaque lettre du message est cachée parmi des caractères aléatoires suivant un décalage fixe entre 500 et 5000 caractères par lettre du message. Par exemple pour la lettre K avec un décalage de 10 cela donnerai : "alsmdieotpKxksldmqlop"

Arriverez-vous à retrouver le message du lutin ?

Le format du flag est CATF{...}

## Résolution

Grâce au format du flag qui nous est fournis, on peut rapidement en déduire que la première lettre du flag est un 'C' et qu'il se trouve entre le caractère 500 et 5000. C'est pourqui nous allons chercher chaque 'C' dans cet intervalle et en prenant son emplacement comme décalage, nous pourrons reconstituer chaque message possible.
Nous avons donc le script suivant :
```
f = open("message.txt","rt")
data = f.read()

decalage = []

for i,c in enumerate(data,1):
    if c == 'C' and i <= 5000 and i >= 500:
        decalage.append(i)

for element in decalage:
    flag = ''
    nb_char = len(data)//element
    for i in range(1,nb_char):
        flag += data[(element*i)-1]
    print(flag)
```
On peut ainsi observer en sortie le flag CATF{brut3_forc3_th3_h1dd3n_fl4g}
