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

### Le Slicing (Découpage de tableau)

La syntaxe `intArr[début:fin]` permet d'extraire une sous-partie d'un tableau (l'index de `fin` est exclus) :

```
intArr := [3]int32{0, 123, 0}
fmt.Println(intArr[1:3]) // Renvoie [123 0] (prend l'index 1 et l'index 2)
```

### Empreinte en mémoire

Les éléments d'un tableau sont stockés de façon contiguë (les uns à la suite des autres) en mémoire :

- 1 élément `int32` = 4 octets (bytes)
    
- Un tableau `[3]int32` occupe exactement **12 octets** (`3 * 4 octets`).


### Syntaxes de déclaration et d'initialisation

Il existe plusieurs façons de déclarer et d'initialiser un Array :

```
// Forme explicite complète
var intArr [3]int32 = [3]int32{1, 2, 3}

// Forme courte avec initialisation
intArr := [3]int32{1, 2, 3}

// Inférence de la taille avec [...] (Go compte le nombre d'éléments pour nous)
intArr := [...]int32{1, 2, 3} // Crée un tableau de type [3]int32

// Initialisation d'index spécifiques
intArr := [5]int32{0: 10, 4: 50} // Crée [10, 0, 0, 0, 50]
```






