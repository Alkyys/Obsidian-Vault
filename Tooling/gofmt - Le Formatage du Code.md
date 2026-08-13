## Le Formatage du Code : `gofmt` & `goimports`

En Go, il n'y a **aucun débat sur le style de code**. Le langage impose un formatage universel via l'outil officiel `gofmt`.

> _"Gofmt's style is no one's favorite, yet gofmt is everyone's favorite."_ — Rob Pike

### Utilisation de `gofmt`

- **Formatage du projet complet :**
    
    Bash
    
    ```
    go fmt ./...
    ```
    
- **Lancer `gofmt` directement :**
    
    Bash
    
    ```
    gofmt -w . # -w écrit les modifications directement dans les fichiers
    ```
    

### L'alternative communautaire : `goimports`

`goimports` fait exactement la même chose que `gofmt`, mais il ajoute ou supprime automatiquement les déclarations `import` inutilisées ou manquantes dans tes fichiers.

Bash

```
# Installation
go install golang.org/x/tools/cmd/goimports@latest

# Exécution
goimports -w .
```