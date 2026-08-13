
Les génériques permettent d'écrire des fonctions et des structures de données réutilisables avec différents types tout en conservant le **typage fort** et la **sécurité à la compilation** (sans utiliser le tricheur `interface{}` qui supprime le contrôle des types).

## 1. Explication ligne par ligne du code

### La fonction générique

```
func sumSlice[T int | float32 | float64](slice []T) T {
```

- **`[T ...]`** : Les crochets `[...]` définissent les **paramètres de type** (_type parameters_). C'est ce qui indique à Go qu'il s'agit d'une fonction générique.
    
- **`T`** : C'est le nom de notre paramètre de type (par convention, une lettre majuscule comme `T`, `K`, `V`).
    
- **`int | float32 | float64`** : C'est la **contrainte de type** (_type constraint_). On utilise le symbole pipe `|` pour créer une union de types. On indique à Go que `T` ne pourra être **que** soit un `int`, soit un `float32`, soit un `float64`.
    
- **`(slice []T)`** : Le paramètre `slice` est une tranche dont les éléments sont obligatoirement du type `T`.
    
- **`) T`** : La fonction retourne une valeur du type `T`.
    

```
    var sum T
```

- Déclare une variable `sum` de type `T`. Elle est automatiquement initialisée à la **zero value** du type `T` (`0` si `T` est un `int`, `0.0` si `T` est un `float`).
    

```
    for _, v := range slice {
        sum += v
    }
    return sum
}
```

- Parcourt la slice et additionne les éléments. L'opérateur `+=` est autorisé par le compilateur parce que tous les types autorisés par la contrainte (`int`, `float32`, `float64`) supportent l'addition.

### L'appel dans la fonction `main()`

```
func main() {
    var intSlice = []int{1, 2, 3}
    fmt.Println(sumSlice[int](intSlice))
```

- **`sumSlice[int](intSlice)`** : On passe le type concret `[int]` entre crochets pour instancier la fonction pour les entiers.

```
    var float32Slice = []float32{1, 2, 3}
    fmt.Println(sumSlice[float32](float32Slice))
}
```

- **`sumSlice[float32](float32Slice)`** : On instancie la fonction générique pour le type `float32`.

> **Astuce (Inférence de type) :** Go est capable de deviner (_inférer_) le type automatiquement ! Tu peux donc écrire plus simplement :
> 
> ```
> fmt.Println(sumSlice(intSlice))      // Go comprend tout seul que T = int
> fmt.Println(sumSlice(float32Slice))  // Go comprend tout seul que T = float32
> ```

## 2. Le mot-clé `any` dans Go

Introduit en même temps que les génériques dans Go 1.18, **`any` est un alias strict pour `interface{}`**.

```
type any = interface{}
```

### Quand utiliser `any` ?

On utilise `any` lorsqu'un paramètre de type ou une fonction accepte **absolument n'importe quel type** sans aucune restriction.

```
// Cette fonction accepte une slice de N'IMPORTE QUEL type (int, string, struct, etc.)
func printSlice[T any](slice []T) {
    for _, v := range slice {
        fmt.Println(v)
    }
}
```

### Pourquoi n'a-t-on pas utilisé `any` dans `sumSlice` ?

Si nous avions écrit :

```
// ❌ ERREUR DE COMPILATION
func sumSlice[T any](slice []T) T {
    var sum T
    for _, v := range slice {
        sum += v // Erreur: invalid operation: operator + not defined for T
    }
    return sum
}
```

**Pourquoi cette erreur ?** `any` accepte aussi des `string`, des `struct`, des `bool` ou des `map`. Comme on ne peut pas faire `+` sur une `map` ou un `bool`, le compilateur Go rejette le code pour des raisons de sécurité.

## 3. Aller plus loin : Définir des contraintes personnalisées

Pour rendre tes signatures de fonctions plus propres, tu peux regrouper les contraintes dans des interfaces ou utiliser le package officiel `golang.org/x/exp/constraints` (ou `cmp` dans la bibliothèque standard).

### A. Interface de contrainte

```
// Définition d'une contrainte réutilisable
type Number interface {
    int | float32 | float64
}

// Utilisation dans la fonction générique
func sumSlice[T Number](slice []T) T {
    var sum T
    for _, v := range slice {
        sum += v
    }
    return sum
}
```

### B. L'opérateur tilde `~` (Types sous-jacents)

Si quelqu'un crée un type personnalisé basé sur `int` :

```
type MyInt int
```

La contrainte `T int` va refuser `MyInt`. Pour autoriser les types personnalisés basés sur un type primitif, on ajoute le symbole `~` :

```
type Number interface {
    ~int | ~float32 | ~float64 // Prend en compte int ET les alias/types personnalisés basés sur int
}
```

### C. La contrainte prédéfinie `comparable`

Go fournit une contrainte intégrée nommée `comparable`. Elle regroupe tous les types qui supportent les opérateurs `==` et `!=` (pratique pour les clés de maps ou la recherche dans des slices) :

```
// Vérifie si un élément existe dans une slice
func Contains[T comparable](slice []T, element T) bool {
    for _, v := range slice {
        if v == element {
            return true
        }
    }
    return false
}
```

