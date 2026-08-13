Go intègre son propre framework de test unitaire via le package standard `testing`.

### Les Règles d'un Fichier de Test

1. Le fichier doit se terminer par **`_test.go`** (ex: `math_test.go`).
    
2. Le fichier est placé dans le **même dossier/package** que le code qu'il teste.
    
3. La fonction de test doit commencer par **`Test`** et accepter un pointeur `*testing.T` (ex: `func TestAddition(t *testing.T)`).
    

### Pattern Idiomatique : Les Table-Driven Tests

En Go, la façon idiomatique d'écrire des tests unitaires est d'utiliser des **tests pilotés par les données** (_Table-Driven Tests_).

Go

```
package math

import "testing"

func TestSum(t *testing.T) {
    // 1. Définition de la table de cas de test
    tests := []struct {
        name     string
        a, b     int
        expected int
    }{
        {name: "nombres positifs", a: 2, b: 3, expected: 5},
        {name: "avec zéro", a: 5, b: 0, expected: 5},
        {name: "nombres négatifs", a: -1, b: -2, expected: -3},
    }

    // 2. Iteration sur les cas de test
    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            result := Sum(tt.a, tt.b)
            if result != tt.expected {
                t.Errorf("Sum(%d, %d) = %d; attendu %d", tt.a, tt.b, result, tt.expected)
            }
        })
    }
}
```

### Commandes Utiles pour `go test`

Bash

```
go test ./...              # Lance tous les tests du projet
go test -v ./...           # Mode verbeux (affiche le détail de chaque cas de test)
go test -race ./...        # Lance les tests avec le détecteur de Race Conditions
go test -cover ./...       # Calcule le pourcentage de couverture de code (Code Coverage)
go test -run TestAddition  # Lance uniquement une fonction de test spécifique
```