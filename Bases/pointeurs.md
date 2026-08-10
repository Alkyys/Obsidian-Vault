
Un **pointeur** est une variable qui stocke l'**adresse mémoire** d'une autre variable au lieu de stocker directement sa valeur.

- **Valeur par défaut (Zero Value) :** `nil`
- **Taille d'un pointeur en mémoire :** 8 octets (système 64 bits) quelle que soit la taille de la donnée pointée.

### 1. Les deux opérateurs essentiels

- **`&` (Address of) :** Récupère l'adresse mémoire d'une variable.
- **`*` (Dereference / Pointer Type) :**
    - Placé devant un type (`*int32`) : définit un type pointeur.
    - Placé devant un pointeur (`*p`) : permet d'accéder à la valeur située à l'adresse mémoire ou de la modifier (déférencement).

```
var x int32 = 42
var p *int32 = &x // 'p' contient l'adresse mémoire de 'x' (ex: 0xc000014088)

fmt.Println(p)  // Affiche l'adresse : 0xc000014088
fmt.Println(*p) // Affiche la valeur pointée : 42

*p = 100        // Modifie directement la valeur de 'x' via le pointeur
fmt.Println(x)  // Affiche : 100
```

### 2. Allocation mémoire avec `new()`

La fonction intégrée `new(T)` alloue de la mémoire pour un type `T`, initialise cette mémoire à sa valeur par défaut (_zero value_), et renvoie un pointeur `*T`.

```
func main() {
    // 1. Pointeur non initialisé (nil)
    var p *int32 
    // *p = 10 // PANIC! runtime error: nil pointer dereference

    // 2. Initialisation avec new()
    p = new(int32) // Reserve l'espace mémoire pour un int32 (initialisé à 0)
    fmt.Println(*p) // Affiche : 0

    *p = 10        // Fonctionne sans erreur !
    fmt.Println(*p) // Affiche : 10
}
```

### 3. Pointeur et Fonctions (Passage par Référence)

Puisque Go copie les arguments passés aux fonctions, utiliser un pointeur permet d'altérer la variable d'origine sans faire de copie.

```
package main

import "fmt"

// Sans pointeur : la variable d'origine n'est pas modifiée
func doubleValeur(val int) {
    val = val * 2 // Seule la copie locale est doublée
}

// Avec pointeur : modifie la variable à son adresse mémoire
func doublePointeur(val *int) {
    *val = *val * 2
}

func main() {
    x := 10

    doubleValeur(x)
    fmt.Println(x) // Output: 10 (inchangé)

    doublePointeur(&x)
    fmt.Println(x) // Output: 20 (modifié !)
}
```

### 4. Pointeurs et Optimisation Mémoire

#### A. Éviter la copie de structures volumineuses

Lorsqu'une `struct` contient beaucoup de champs ou de sous-structures, la passer à une fonction par valeur oblige Go à recopier l'intégralité des octets en mémoire à chaque appel. Passer un pointeur ne copie que 8 octets (l'adresse).

```
type BigData struct {
    buffer [1000000]int64 // Structure d'environ 8 Mo
}

// MAUVAIS : Copie 8 Mo de données sur la pile à chaque appel !
func processData(d BigData) {
    // ...
}

// BON : Copie seulement 8 octets (l'adresse mémoire)
func processDataOptimized(d *BigData) {
    // ...
}
```

#### B. La nuance importante : L'Analyse d'Échappement (_Escape Analysis_)

Même si les pointeurs évitent les copies, en abuser n'est pas toujours la meilleure solution :

- **La Pile (_Stack_) :** Rapide, nettoyée automatiquement dès qu'une fonction se termine. Les variables passées par valeur y résident souvent.
    
- **Le Tas (_Heap_) :** Si une variable survit à la fin d'une fonction (parce qu'on renvoie son pointeur), Go l'alloue sur le Tas via l'**_Escape Analysis_**. Le Garbage Collector (GC) doit alors la nettoyer, ce qui peut impacter les performances si vous en abusez.
    

> **Règle générale :**
> 
> - Utilisez des **pointeurs** pour modifier une variable ou éviter la copie de grandes structures (`struct`).
>     
> - Utilisez des **valeurs** pour les types primitifs simples (`int`, `bool`, `string`, `float`) ou les petites structures.
>