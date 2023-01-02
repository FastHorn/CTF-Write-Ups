# Starry Sky
**Points : 750**

**Difficulté : Moyen**

**Catégorie : Programmation**

## Sources

Afin de décompresser de cette journée de rush, mini-monkey décide de passer une nuit à la belle étoile.

Il se pose dans l'herbe, regarde le ciel et contemple les étoiles.


Vert = 0

Rouge = 1

Rose = 2 (Fin du GIF)


Essayez de trouver le message transmis par ce GIF !

Le FLAG est sous la forme CATF{message}

## Résolution

Dans ce challenge il nous est donné un GIF, celui-ci étant un format assez connu composé de plusieurs images, nous allons le décompresser afin de récupérer chaque image. Pour faciliter la suite de la résolution du challenge, nous allons utiliser 'graphicsmagick' pour convertir les images obtenues du format png au format jpg, sans quoi il sera plus difficile de déceler la couleur du pixel avec python.

Nous avons désormais toutes les images que nous devons analyser, on va donc créer un script python qui va lire chaque pixel de chaque image et écire '0' si il y a un pixel vert et '1' si il y a un pixel rouge.

On obtient donc le script python suivant :

```
from PIL import Image

f = ''

for i in range(256):

    if i < 10:
        image = Image.open(f"images/GIF_Frame  {i}.jpg")
    elif i < 100:
        image = Image.open(f"images/GIF_Frame {i}.jpg")
    elif i < 1000:
        image = Image.open(f"images/GIF_Frame{i}.jpg")

    width, height = image.size

    # Parcourir chaque pixel de l'image
    for x in range(width):
        for y in range(height):
            # Obtenir la couleur du pixel
            pixel = image.getpixel((x, y))
            # Vérifier si le pixel est vert
            if pixel[1] > 150:
                f += '0'
            # Vérifier si le pixel est rouge
            elif pixel[0] > 150:
                f += '1'

print(f)
```

Apres exécution du script nous obtenons :
>0100100101010111011011110111010101101100011001000100110001101001011010110110010101000001010001000110000101110100011001010101011101101001011101000110100001001101011010010110111001101001010011010110111101101110011010110110010101111001010011100100011101001100

On va donc mainteant décoder le binaire, on se rend sur un décodeur en ligne : https://www.dcode.fr/ascii-code

On obtient ainsi le flag :
>CATF{IWouldLikeADateWithMiniMonkeyNGL}
