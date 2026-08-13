En Go, les fonctions qui peuvent échouer retournent leur résultat **plus** une valeur de type `error` (souvent comme dernier argument). Le code appelant doit immédiatement tester si l'erreur n'est pas `nil`.

```
package main

import (
    "errors"
    "fmt"
)

// Correction de la fonction intDivision
func intDivision(numerator int, denominator int) (int, int, error) {
    if denominator == 0 {
        // errors.New() crée une erreur simple à partir d'un texte
        return 0, 0, errors.New("cannot divide by zero")
    }
    
    result := numerator / denominator
    remainder := numerator % denominator
    
    return result, remainder, nil // nil signifie que tout s'est bien passé
}

func main() {
    res, rem, err := intDivision(10, 0)
    if err != nil {
        // Traitement explicite de l'erreur
        fmt.Println("Erreur détectée :", err)
        return
    }
    
    fmt.Printf("Résultat: %d, Reste: %d\n", res, rem)
}
```

## 2. Erreurs Sentinel & Formatage (`fmt.Errorf`)

### A. Erreurs Sentinel

Pour pouvoir comparer des erreurs spécifiques plus tard, on déclare souvent des variables d'erreur globales (nommées par convention `ErrSomething`) :

```
var ErrDivideByZero = errors.New("cannot divide by zero")
var ErrNotFound     = errors.New("item not found")
```

### B. Formater une erreur avec des variables

Pour inclure des détails dynamiques dans une erreur, on utilise `fmt.Errorf()` :

```
func checkAge(age int) error {
    if age < 18 {
        return fmt.Errorf("âge invalide: %d (doit être au moins 18)", age)
    }
    return nil
}
```

## 3. Empaquetage d'Erreurs (Error Wrapping)

Lorsqu'une erreur remonte la chaîne d'appels de fonctions, il est très utile de lui ajouter du contexte sans perdre l'erreur d'origine.

En Go, on empaquète (_wrap_) une erreur en utilisant le verbe **`%w`** dans `fmt.Errorf()` :

```
func processData() error {
    err := readConfig()
    if err != nil {
        // On enveloppe l'erreur d'origine dans un nouveau message
        return fmt.Errorf("échec du traitement des données: %w", err)
    }
    return nil
}
```

## 4. Inspecter les Erreurs : `errors.Is` vs `errors.As`

Quand une erreur a été empaquetée (_wrapped_), un simple `==` ou une assertion de type classique ne fonctionne plus. C'est là qu'interviennent `errors.Is` et `errors.As`.

### A. `errors.Is` (Pour comparer une valeur d'erreur)

**`errors.Is(err, target)`** vérifie si une erreur spécifique (comme une erreur Sentinel) se trouve n'importe où dans la chaîne d'empaquetage de `err`.

```
package main

import (
    "errors"
    "fmt"
)

var ErrNotFound = errors.New("ressource introuvable")

func fetchUser(id int) error {
    // Imaginons que la BDD renvoie ErrNotFound et qu'on l'enveloppe
    return fmt.Errorf("erreur service utilisateur (id %d): %w", id, ErrNotFound)
}

func main() {
    err := fetchUser(42)

    // ❌ Mauvaise pratique (échoue à cause du wrapping fmt.Errorf)
    // if err == ErrNotFound { ... }

    // ✅ Bonne pratique : errors.Is parcourt toute la chaîne d'erreurs
    if errors.Is(err, ErrNotFound) {
        fmt.Println("L'utilisateur n'existe pas en BDD !")
    }
}
```

### B. `errors.As` (Pour extraire un type d'erreur personnalisé)

Parfois, une erreur est représentée par une `struct` personnalisée contenant des champs d'information supplémentaires. **`errors.As(err, &target)`** cherche si un type d'erreur spécifique existe dans la chaîne et **l'extrait** dans une variable.

```
package main

import (
    "errors"
    "fmt"
)

// 1. Structure d'erreur personnalisée
type QueryError struct {
    Code    int
    Message string
}

// 2. Implémentation de l'interface 'error'
func (e *QueryError) Error() string {
    return fmt.Sprintf("Code %d: %s", e.Code, e.Message)
}

func executeQuery() error {
    customErr := &QueryError{Code: 404, Message: "table non trouvée"}
    return fmt.Errorf("échec de la requête SQL: %w", customErr) // Wrapped!
}

func main() {
    err := executeQuery()

    // Variable qui va recevoir l'erreur personnalisée si elle existe
    var qErr *QueryError

    // errors.As vérifie si 'err' contient un *QueryError et le copie dans 'qErr'
    if errors.As(err, &qErr) {
        fmt.Println("Erreur personnalisée extraite !")
        fmt.Println("Code d'erreur :", qErr.Code)       // Output: 404
        fmt.Println("Détail d'origine :", qErr.Message) // Output: table non trouvée
    }
}
```
### Résumé comparatif

|**Fonction**|**Utilité**|**Quand l'utiliser ?**|
|---|---|---|
|**`errors.Is(err, target)`**|Compare les **valeurs** d'erreurs|Pour tester si l'erreur est une erreur Sentinel spécifique (`ErrNotFound`, `io.EOF`, etc.).|
|**`errors.As(err, &target)`**|Vérifie et extrait un **type** d'erreur|Pour convertir l'erreur vers une `struct` personnalisée et lire ses champs.|
