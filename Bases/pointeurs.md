les pointeur sont des variable qui stocke des enplacement memoire de stockage et non des valeur en elle meme. elle sont sous cette forme `var p = 0x165476576b7`
pour les utiliser et les déclarer dans le code on utilise cette syntaxe `var p *int32` 
ici on réserve un epace memoir de 32 bits ou 4 bytes et p est le pointeur (adresse mémoire). 
pour initialiser on va utiliser la fonction new() comme ceci `new(int32)`une fois ceci fait notre pointeur a une valeur autre que nil (qui est la valeur de base) ici ca sera une adresse mémoire. quand on a un pointeur et qu'on veux pas l'adresse mais les element qui pointe on va dereferencing le pointeur en utilisant cette syntax `*p` ici il répondra 0 car c'est la valeur de base d'un int32
la si on veux changer de int on utilisera `*p=10` 
attention dans cette démarche il me faut pas oublier de initilaiser l'adresse du pointeur sinon a l'éxecution on va avoir un soucis et non a la compilation.

Un **pointeur** stocke l'**adresse mémoire** d'une variable au lieu de sa valeur directe.

- `&` permet d'obtenir l'adresse d'une variable.
    
- `*` permet d'accéder à la valeur située à cette adresse (ou de définir un type pointeur).

Ils permettent de modifier une variable originale à l'intérieur d'une fonction sans en faire une copie.

Exemple 
```
x := 10
p := &x // p contient l'adresse mémoire de x

*p = 20 // Modifie directement la valeur de x
fmt.Println(x) // Affiche 20
```



(mettre un explication des pointeur dans l'optimisation mémoir et des exemples)
