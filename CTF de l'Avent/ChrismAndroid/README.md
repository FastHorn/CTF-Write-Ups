# ChrismAndroid
**Points : 750**

**Difficulté : Facile**

**Catégorie : Android**

## Sources

Sur la demande du Père Noël, l'équipe MAK'HACK a dévelopé une application mobile pour voir les photos de son animal de compagnie. Les lutins ont remarqué que des secrets ont été glissé dans l'application. Pouvez-vous les retrouver ?

Le format du flag est CATF{...}

## Résolution

Dans ce challenge on nous fournis une apk. La première étape va donc être de la décompresser afin de l'examiner de plus près.

A l'intérieur on observe 3 fichiers classes.dex, on va donc chercher à les décompresser afin de savoir si on peut y trouver des choses intéressantes. Dans le troisième fichier, nous pouvons apercevoir des fichiers java qui me semblent intéressants, notemment ceux nommés 'MainActivity' et 'Decryptor'.

Le fichier 'Decryptor' nous fournis un string et une clé ainsi qu'une fonction de décryption, n'étant pas très a l'aise avec java, j'ai réécris la fonction en python :
```
import base64
import hashlib
from cryptography.hazmat.primitives.ciphers import Cipher, algorithms, modes
from cryptography.hazmat.backends import default_backend

def set_key(key: str) -> bytes:
  key_bytes = key.encode("utf-8")
  digest = hashlib.sha1(key_bytes).digest()
  key_bytes = digest[:16]
  return key_bytes

def decrypt(my_string: str, key_bytes: bytes) -> str:
  cipher = Cipher(algorithms.AES(key_bytes), modes.ECB(), backend=default_backend())
  decryptor = cipher.decryptor()
  decoded_string = base64.b64decode(my_string)
  decrypted_bytes = decryptor.update(decoded_string) + decryptor.finalize()
  return decrypted_bytes.decode("utf-8")

key_bytes = set_key("s4nT4XM4Kh4Ck")
decrypted_string = decrypt("0ORLPmCXZPjyPm3jMAbQ4q0Wvfb2nGdHzis/kLjNU4g=", key_bytes)
print(decrypted_string)
```
Apres éxécution, nous obtenons :
>Part 2  : _AnDr01d_r3V3Rs3}♣♣♣♣♣

On en déduit qu'il nous manque une partie.

On va donc aller chercher dans notre fichier 'MainActivity', on observe quelque chose d'étrange 'monkey_secret', on peut en déduire que parmis les images de singe que cette application affiche, il doit y en avoir une secrète. On va donc simplement faire une recherche dans nos fichier et bingo, on trouve une image nommé 'monkey_secret.png'.

En l'ouvrant il nous est affiché la première partie de notre flag :
>CATF{S4nt4_l0v3S

On peut ainsi écrire notre flag complet
>CATF{S4nt4_l0v3S_AnDr01d_r3V3Rs3}
