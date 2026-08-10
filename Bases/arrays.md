En Go, un **Array** a une **taille fixe** définie lors de sa déclaration. Sa taille fait partie intégrante de son type (ex: `[3]int32` et `[5]int32` sont deux types complètement différents).

Déclaration et Accès
```
func main(){
	var intArr [3]int32
	intArr[1] = 123
}
```
- On crée un tableau de 3 entiers `int32`.
    
- Par défaut, tous les éléments sont initialisés à leur _Zero Value_ (`0`). Le tableau contient donc `[0, 0, 0]`.
    
- L'indexation commence à **`0`**. Pour accéder au 2ᵉ élément, on utilise `intArr[1]`.
    
- Après l'assignation `intArr[1] = 123`, le tableau contient `[0, 123, 0]`.
    
> **Attention :** Tenter d'accéder à un index en dehors des limites (ex: `intArr[5]`) génère une erreur de compilation ou un `panic: runtime error: index out of range` à l'exécution.





Ci dessus on  a crée un tabeau de int32 avec 3 elements

pour acceder au element du tableau on va utiliser cette syntaxe, attention les index commence a 0 et non pas a 1, par exemple si on veux avoir le second element on va l'apeller `intArr[1]
lorsqu'on demande un element avec l'index hors du tableau on obtient 0, en suivant l'exemple au dessus on aurais `intArr[1:3]` on aurais comme résultat `[123 0]`

sachant que un int32 prend 4 bytes de mémoire, notre tableau prend 12 bytes

on peut aussi déclarer et assigné un tableau comme ceci `var intArr [3]int32 = [3]int32{1,2,3}` ou encore `intArr := [3]int32{1,2,3}` ou encore ``intArr := [...]int32{1,2,3}``





