Une **Map** est une structure de données qui associe des clés uniques à des valeurs (`map[TypeClé]TypeValeur`).

### 1. Déclaration et Initialisation

```
// 1. Déclaration avec initialisation immédiate (Litéral)
var myMap = map[string]uint8{
    "Adam": 23,
    "Moi":  34,
}

// 2. Déclaration avec make() (recommandé pour créer une map vide)
myMap2 := make(map[string]uint8)
myMap2["Sam"] = 18

// 3. PIÈGE : Map 'nil' (déclarée mais non initialisée)
var nilMap map[string]uint8 // nilMap vaut nil
// fmt.Println(nilMap["Sam"]) // OK -> Renvoie 0
// nilMap["Sam"] = 25       // PANIC! Écriture impossible sur une map nil
```

### 2. Accès aux éléments et le Comma-OK Idiom

Lorsqu'on cherche une clé qui n'existe pas dans la map, Go renvoie la **Zero Value** du type de la valeur (ex: `0` pour un nombre, `""` pour une string, `false` pour un booléen).

Pour distinguer une clé absente d'une clé dont la valeur est réellement `0`, on utilise la syntaxe **Comma-OK** qui renvoie deux valeurs : la valeur et un booléen (`true` si la clé existe, `false` sinon).

```
func main() {
    myMap := map[string]uint8{"Adam": 23}

    // Clé existante
    age, ok := myMap["Adam"]
    fmt.Println(age, ok) // Output: 23 true

    // Clé inexistante
    ageSam, okSam := myMap["Sam"]
    fmt.Println(ageSam, okSam) // Output: 0 false
}
```

### 3. Suppression d'éléments avec `delete()`

La fonction intégrée `delete(map, clé)` permet de retirer une clé et sa valeur associée. Elle modifie la map directement en mémoire et ne renvoie aucune valeur.

```
delete(myMap, "Adam") // Supprime la clé "Adam"
delete(myMap, "Inexistant") // Ne fait rien et ne plante pas si la clé n'existe pas
```

### 4. Parcours d'une Map (`for range`)

On peut parcourir une map avec une boucle `for ... range`.

> **Important :** L'ordre de parcours des éléments **n'est pas garanti** et change d'une exécution à l'autre.

```
myMap := map[string]uint8{"Adam": 23, "Moi": 34, "Sam": 18}

// Parcours Clé et Valeur
for name, age := range myMap {
    fmt.Printf("Nom: %v, Âge: %v\n", name, age)
}

// Parcours des Clés uniquement
for name := range myMap {
    fmt.Printf("Nom: %v\n", name)
}

// Parcours des Valeurs uniquement (en ignorant la clé avec '_')
for _, age := range myMap {
    fmt.Printf("Âge: %v\n", age)
}
```