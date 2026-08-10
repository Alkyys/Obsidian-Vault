À la différence des **Arrays**, les **Slices** ont une **taille dynamique** qui peut évoluer selon les éléments ajoutés ou supprimés. En coulisses, une slice s'appuie toujours sur un tableau fixe masqué (le _tableau sous-jacent_).

### Déclaration et utilisation de base

```
// Déclaration d'une slice de strings (pas de taille entre les crochets)
fruits := []string{"pomme", "banane"}

// Ajout d'un élément à la fin avec append()
fruits = append(fruits, "orange")
```

### Longueur (`len`) vs Capacité (`cap`)

Une slice possède deux propriétés clés :

- **`len` (Longueur) :** Le nombre d'éléments actuellement visibles et stockés dans la slice (`len(slice)`).
- **`cap` (Capacité) :** Le nombre maximum d'éléments que le tableau sous-jacent peut contenir sans réallocation mémoire (`cap(slice)`).

**Exemple conceptuel :**

```
Slice: [4, 5, 6, 7, *, *]
Length   (len) = 4  (éléments initialisés : 4, 5, 6, 7)
Capacity (cap) = 6  (2 emplacements encore réservés en mémoire)
```

### Création optimisée avec `make()`

Créer une slice avec la fonction intégrée `make()` permet de prédéfinir sa capacité pour éviter les réallocations mémoire coûteuses :

```
var intSlice []int32 = make([]int32, 3, 8) 
// Crée une slice de int32 : len = 3 (remplie de 0 par défaut), cap = 8
```

#### Pourquoi la capacité est-elle cruciale pour les performances ?

Lorsque tu fais `append()` sur une slice dont la longueur atteint la capacité (`len == cap`), Go effectue les étapes suivantes à la volée :

1. Il alloue un **nouveau tableau sous-jacent** en mémoire (en général **2 fois plus grand**).
2. Il **copie** tous les anciens éléments vers ce nouveau tableau.
3. Il ajoute le nouvel élément.

Utiliser `make([]T, len, cap)` quand tu connais à l'avance la taille approximative de tes données permet d'éviter ces étapes de réallocation répétées.

### Copie de Slices et Gestion de la mémoire

Une slice est un **type référence** : elle contient un pointeur vers un tableau sous-jacent.

#### Piège de l'assignation directe (`:=`)
Si tu égalises deux variables de type slice, elles partagent le **même tableau sous-jacent** :

```
a := []int{1, 2, 3}
b := a // b pointe vers le même tableau que a !

b[0] = 99
fmt.Println(a) // Output: [99, 2, 3] -> 'a' a aussi été modifié !
```

#### Copie indépendante avec `copy()`
Pour dupliquer physiquement les données dans un nouvel espace mémoire, il faut utiliser la fonction `copy()` :

```
a := []int{1, 2, 3}
b := make([]int, len(a)) // 1. Créer une slice cible de même longueur

copy(b, a) // 2. Copier les éléments de 'a' dans 'b'

b[0] = 99
fmt.Println(a) // Output: [1, 2, 3] -> 'a' reste intact
fmt.Println(b) // Output: [99, 2, 3]
```