Les tableau ont une longeur fix, elle est déclarer comme cela :

```
finc main(){
	var intArr [3]int32
	intArr[1] = 123
}
```
Ci dessus on  a crée un tabeau de int32 avec 3 elements

pour acceder au element du tableau on va utiliser cette syntaxe, attention les index commence a 0 et non pas a 1, par exemple si on veux avoir le second element on va l'apeller `intArr[1]
lorsqu'on demande un element avec l'index hors du tableau on obtient 0, en suivant l'exemple au dessus on aurais `intArr[1:3]` on aurais comme résultat `[123 0]`

sachant que un int32 prend 4 bytes de mémoire, notre tableau prend 12 bytes

on peut aussi déclarer et assigné un tableau comme ceci `var intArr [3]int32 = [3]int32{1,2,3}` ou encore `intArr := [3]int32{1,2,3}` ou encore ``intArr := [...]int32{1,2,3}``





