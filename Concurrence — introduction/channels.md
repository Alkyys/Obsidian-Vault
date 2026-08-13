Les **Channels** sont les tuyaux qui permettent à différentes Goroutines de communiquer et d'échanger des données en toute sécurité, sans utiliser de verrous (_Mutex_).

> **Philosophie de Go :** _"Do not communicate by sharing memory; instead, share memory by communicating."_ (Ne communiquez pas en partageant de la mémoire ; partagez la mémoire en communiquant).

### 1. Le Piège du Deadlock avec les Unbuffered Channels

Par défaut, un canal est **non tamponné** (_unbuffered_), c'est-à-dire qu'il a une capacité de `0`. Un envoi bloque l'émetteur jusqu'à ce que le récepteur soit prêt à lire.

```
package main

func main() {
    var c = make(chan int) // Canal non tamponné

    c <- 1 // ❌ DEADLOCK ! Le programme se bloque ici éternellement 
           // car aucune autre Goroutine n'est en train de lire 'c'.

    i := <-c // Cette ligne n'est jamais atteinte.
}
```

### 2. Exemple Pratique : Recherche Concurrentielle du Prix du Poulet

Voici le programme corrigé. Il lance 3 Goroutines parallèles pour surveiller les prix sur 3 sites web. Le premier site qui trouve un prix $\le 5$ gagne la course et envoie son nom dans le canal.

#### Code Go corrigé :

```
package main

import (
    "fmt"
    "math/rand"
    "sync"
    "time"
)

var MAX_CHICKEN_PRICE float32 = 5.0

func main() {
    var chickenChannel = make(chan string)
    var websites = []string{"walmart.com", "costco.com", "wholefoods.com"}

    // Lancement d'une Goroutine pour chaque site
    for i := range websites {
        go checkChickenPrices(websites[i], chickenChannel)
    }

    // Réception du premier résultat disponible
    sendMessage(chickenChannel)
}

func checkChickenPrices(website string, chickenChannel chan string) {
    for {
        time.Sleep(time.Second * 1)
        var chickenPrice = rand.Float32() * 20 // Génère un prix entre 0 et 20

        if chickenPrice <= MAX_CHICKEN_PRICE {
            chickenChannel <- website // Envoie le nom du site dans le canal
            break                     // Sort de la boucle
        }
    }
}

func sendMessage(chickenChannel chan string) {
    // <-chickenChannel bloque jusqu'à ce qu'un site envoie une valeur
    fmt.Printf("\nFound a deal on chicken at %s!\n", <-chickenChannel)
}
```

### 3. Explication ligne par ligne de ce qui se passe

#### Dans `main()` :

1. **`chickenChannel := make(chan string)`** : On crée un canal synchrone capable de faire circuler des chaînes de caractères.
    
2. **`for i := range websites { go checkChickenPrices(websites[i], chickenChannel) }`** :
    - On parcourt la liste des 3 sites.
        
    - On démarre **3 Goroutines distinctes** en arrière-plan. Elles vont s'exécuter en parallèle.
        
3. **`sendMessage(chickenChannel)`** : On appelle la fonction d'affichage en lui passant le canal.
    

#### Dans `checkChickenPrices(...)` :

4. Chaque Goroutine tourne dans une boucle `for` infinie et attend 1 seconde (`time.Sleep`).
    
5. Elle génère un prix aléatoire. Si ce prix est $> 5$, elle retente sa chance à la boucle suivante.
    
6. **`chickenChannel <- website`** : Dès qu'une Goroutine trouve un prix $\le 5$, elle tente d'envoyer le nom de son site dans `chickenChannel`.
    
    - Cette ligne **bloque** cette Goroutine spécifique jusqu'à ce que quelqu'un lise l'information de l'autre côté du tuyau.
        
7. **`break`** : Une fois la donnée transmise, elle quitte la boucle et sa fonction se termine.
    

#### Dans `sendMessage(...)` :

8. **`<-chickenChannel`** : C'est l'opération de lecture. La fonction `main` (qui exécute `sendMessage`) se **met en pause** et attend qu'un message arrive dans le canal.
    
9. Dès que la **première** des 3 Goroutines envoie son site web via `chickenChannel <- website`, la valeur est immédiatement lue par `<-chickenChannel`.
    
10. La fonction `main` affiche la phrase avec le nom du site gagnant puis le programme se termine.
    

### 4. Complément : Channels Tamponnés (_Buffered Channels_)

Si tu veux stocker des éléments dans un canal sans bloquer immédiatement l'émetteur, tu peux lui donner une **capacité** (un tampon) lors du `make` :


```
// Canal tamponné d'une capacité de 3 éléments
var bufferedChan = make(chan string, 3)

bufferedChan <- "Walmart"    // Ne bloque PAS
bufferedChan <- "Costco"     // Ne bloque PAS
bufferedChan <- "WholeFoods" // Ne bloque PAS

// Le 4ème envoi bloquerait car le tampon de 3 est plein !
```
