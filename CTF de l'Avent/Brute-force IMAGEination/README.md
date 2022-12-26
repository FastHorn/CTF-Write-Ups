# Brute-force IMAGEination
**Points : 750**

**Difficulté : Moyen**

**Catégorie : Programmation**

## Sources

Le lutin de la dernière fois vous a transmis un nouveau message ainsi que la notice suivante :
> Le message décrit une image au format RGB mais je ne connais pas sa largeur, je pense toutefois qu'elle doit être comprise entre 10 et 1000 pixels

Arriverez-vous à retrouver l'image originale ?

## Résolution

Dans ce challenge nous avons un fichier texte qui nous est fourni dans lequel on peut retrouver une liste de nombres allant de 0 à 255.
> 255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,...

On en déduit donc qu'ils correspondent aux couleurs RGB des pixels d'une image.

Nous allons donc utiliser ces pixels pour reconstituer une image, tout en faisant varier la largeur de l'image entre 10 et 1000 pixels.

Nous allons dans un premier temps regrouper les nombres 3 par 3 pour recréer les pixels.
```
RGB = []
for i,_ in enumerate(color.split(",")):
    if i%3 == 0:
        R = _
    if i%3 == 1:
        G = _
    if i%3 == 2:
        B = _
        RGB.append([R + "," + G + "," + B])
```

Puis il faut vérifier que les dimensions sont possibles, en effet, on dénombre 3132 pixels, donc il faut que la largeur fois la hauteur soit égale à ce nombre.
```
largeur = []
hauteur = []

#Determination des largeur/hauteur possible
largeur_possible = [i for i in range(10,1001)]
hauteur_possible = [i for i in range(0,314)]
for i in largeur_possible:
    for j in hauteur_possible:
        if i*j == 3132: #Nombre de pixel
            largeur.append(i)
            hauteur.append(j)
```
Pour finir on teste de recréer chaque image avec les dimensions que nous avons trouvées. Ainsi nous obtenons ce script final :
```
import os
from PIL import Image

color = open("image.txt","rt").read()

RGB = []
for i,_ in enumerate(color.split(",")):
    if i%3 == 0:
        R = _
    if i%3 == 1:
        G = _
    if i%3 == 2:
        B = _
        RGB.append([R + "," + G + "," + B])


largeur = []
hauteur = []

#Determination des largeur/hauteur possible
largeur_possible = [i for i in range(10,1001)]
hauteur_possible = [i for i in range(0,314)]
for i in largeur_possible:
    for j in hauteur_possible:
        if i*j == 3132: #Nombre de pixel
            largeur.append(i)
            hauteur.append(j)

#Création des images
for i,element in enumerate(hauteur):
    im = Image.new('RGB',(largeur[i],hauteur[i]))

    pixel = 0
    for x in range(largeur[i]):
        for y in range(hauteur[i]):
            im.putpixel((x,y),(int(str(RGB[pixel][0]).split(",")[0]),int(str(RGB[pixel][0]).split(",")[1]),int(str(RGB[pixel][0]).split(",")[2])))
            pixel += 1

    im.save(f'flag{element}.png')
```

En regardant les images que nous avons recrée, on ramarque un texte sur l'une d'entre-elle : 

