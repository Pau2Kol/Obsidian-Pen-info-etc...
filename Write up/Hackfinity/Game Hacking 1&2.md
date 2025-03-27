![[Pasted image 20250323140242.png]]
Bon, première chose : on installe le .exe.

Lorsqu'on le lance, on se rend compte que c’est une refonte de Tetris avec un score à atteindre.

![[Pasted image 20250323140307.png]]999999, ce serait assez long. Du coup, pour changer la valeur de notre score, on a plusieurs méthodes disponibles. Moi, j’ai choisi d’utiliser Cheat Engine.

Après avoir scanné l’ordinateur et choisi **Tetris (Debug)**,
![[Pasted image 20250323140355.png]]On insère notre score comme valeur, puis on fait un First Scan. Cheat Engine va chercher toutes les valeurs égales à Value, ici 0 pour nous.
![[Pasted image 20250323140407.png]]Ensuite, on retourne sur Tetris, on augmente notre score, puis on fait un **Next Scan** avec notre nouveau score. Ici, Cheat Engine va chercher dans les valeurs déjà scannées celles qui sont passées à **100**.

On a encore trop de résultats, donc on va refaire la même chose pour **200**.

On a maintenant **2 valeurs**.
![[Pasted image 20250323140420.png]]Il y a forcément celle qui nous intéresse dedans. On a juste à changer les deux valeurs par un nombre plus grand que **999999**.

**Clic droit** sur la valeur → **Change the value of the selected address**, puis on met, par exemple, **1000000000**.
![[Pasted image 20250323140438.png]]Maintenant, il faut juste faire en sorte d’actualiser notre score en augmentant notre nombre de points (je ne peux rien faire pour les skill issues).
![[Pasted image 20250323140552.png]]🙂

Bon pour le game hacking 2 c’est la même chose sauf qu’il faut changer le score à atteindre, par chance il n’y a pas beaucoup de valeurs égal à 9999999

On obtient:
![[Pasted image 20250323140613.png]]👍

#reverse_engineering