`go vet` est le linter officiel intégré à la chaîne d'outils Go. Il ne vérifie pas le style (c'est le rôle de `gofmt`), mais examine le code source à la recherche de **bugs potentiels ou de constructions louches** qui compilent mais risquent de provoquer des erreurs à l'exécution.

### Ce que détecte `go vet` :

- Mauvais arguments de formatage dans `fmt.Printf` (ex: `fmt.Printf("%d", "chaine")`).
    
- Variables non utilisées ou masquées (_variable shadowing_).
    
- Verrous (`sync.Mutex`) passés par valeur (copie) au lieu d'être passés par pointeur.
    
- Code inatteignable (_unreachable code_) après un `return` ou `panic`.
    
- Conflits de Goroutines sur des variables de boucle.
    

### Exécution

Bash

```
go vet ./...
```