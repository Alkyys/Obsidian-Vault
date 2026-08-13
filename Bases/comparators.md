
## Opérateurs de Comparaison, Logiques et Assignation

### 1. Opérateurs Logiques

Permettent de combiner plusieurs conditions booléennes.

| **Opérateur** | **Nom**       | **Description**                                                        | **Exemple**      |
| ------------- | ------------- | ---------------------------------------------------------------------- | ---------------- |
| **`&&`**      | **ET (AND)**  | Renvoie `true` si les **deux** variables/conditions sont vraies.       | `var1 && var2`   |
| **\|\|**      | **OU (OR)**   | Renvoie `true` si **au moins une** des variables/conditions est vraie. | `var1 \|\| var2` |
| **!**         | **NON (NOT)** | Inverse la valeur booléenne (`!true` devient `false`).                 | `!var1`          |

```
a := true
b := false

fmt.Println(a && b) // false
fmt.Println(a || b) // true
fmt.Println(!a)     // false
```

### 2. Opérateurs de Comparaison

En Go, la comparaison nécessite que les deux opérandes soient du **même type**.

  

- **`==`** : Égal à
- **`!=`** : Différent de
- **`<`** / **`>`** : Strictement inférieur / supérieur
- **`<=`** / **`>=`** : Inférieur ou égal / Supérieur ou égal

> **Rappel :** L'opérateur `===` (égalité stricte en JS) **n'existe pas en Go**.
### 3. Pas de _Truthy / Falsy_ en Go !

Contrairement à d'autres langages, Go refuse d'évaluer directement des types non booléens dans des conditions.

```
// ❌ INCORRECT (Erreur de compilation : non-boolean condition in if statement)
var age int = 18
if age { ... } 

var str string = "hello"
if str { ... }

// ✅ CORRECT : Il faut toujours un test booléen explicite
if age != 0 { ... }
if str != "" { ... }
if myPtr != nil { ... }
```

#### Tableau des Zero Values (Équivalents des valeurs "Falsy" des autres langages)

Même si Go ne les convertit pas automatiquement en `false`, voici les valeurs par défaut de chaque type :

|**Type**|**Zero Value ("Falsy" en d'autres langages)**|
|---|---|
|`bool`|`false`|
|`int`, `float`|`0` / `0.0`|
|`string`|`""` (chaîne vide)|
|Pointeurs, Slices, Maps, Interfaces, Channels|`nil`|

### 4. Raccourcis d'Opérations et d'Assignation

Go propose des opérateurs raccourcis pour modifier la valeur d'une variable numérique :

```
i := 10

i++   // i = i + 1  (Incrémentation de 1)
i--   // i = i - 1  (Décrémentation de 1)

i += 10 // i = i + 10 (Rajoute 10)
i -= 10 // i = i - 10 (Enlève 10)
i *= 10 // i = i * 10 (Multiplie par 10)
i /= 10 // i = i / 10 (Divise par 10)
i %= 3  // i = i % 3  (Modulo : reste de la division entière)
```

> **Attention avec `i++` et `i--` :**
> 
> Ce sont des instructions isolées. On ne peut pas les écrire à l'intérieur d'une affectation ou d'une condition.
> 
> ```
> // ❌ Impossible en Go :
> y := i++ 
> if i++ > 5 { ... }
> ```
