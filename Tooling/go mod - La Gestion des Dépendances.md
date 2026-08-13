Depuis Go 1.11 (et par défaut depuis Go 1.13), la gestion des paquets repose sur les **Go Modules**. Un module est une collection de packages Go versionnés ensemble.

### Les Fichiers Clés

- **`go.mod`** : Le fichier manifeste à la racine du projet. Il définit le nom du module (son chemin d'importation), la version minimale de Go requise, et les dépendances directes et indirectes avec leurs versions.
    
- **`go.sum`** : Le fichier de contrôle de sécurité (_checksums_). Il garantit que les dépendances téléchargées n'ont pas été altérées par rapport au hachage officiel. **Ce fichier doit impérativement être commité sur Git.**
    

### Commandes Essentielles

|Commande|Description|
|---|---|
|`go mod init <module-path>`|Initialise un nouveau module Go (ex: `go mod init [github.com/user/monprojet](https://github.com/user/monprojet)`).|
|`go get <package>@<version>`|Télécharge et ajoute une dépendance dans `go.mod` (ex: `@v1.2.0`, `@latest`).|
|`go mod tidy`|**Incontournable.** Nettoie le fichier `go.mod` en supprimant les dépendances inutilisées et en ajoutant les manquantes.|
|`go mod vendor`|Recopie localement toutes les dépendances dans un dossier `vendor/` à la racine (utile pour des builds hors-ligne ou isolés).|
|`go mod verify`|Vérifie que le contenu des dépendances locales correspond aux hachages du `go.sum`.|