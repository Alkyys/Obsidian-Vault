### 1. Concurrence vs Parallélisme

- **Concurrence :** Capacité du programme à gérer plusieurs tâches en chevauchant leur exécution.
- **Parallélisme :** Exécution physique simultanée de plusieurs instructions sur plusieurs cœurs de processeur.

En Go, les **Goroutines** sont des "threads virtuels" extrêmement légers (quelques kilo-octets) gérés par le Go Runtime et non directement par l'OS.

### 2. Le problème du code synchrone (Séquentiel)

Chaque appel réseau ou base de données bloque l'exécution en attendant la réponse.

```
package main

import (
    "fmt"
    "math/rand"
    "time"
)

var dbData = []string{"id1", "id2", "id3", "id4", "id5"}

func main() {
    t0 := time.Now()
    for i := 0; i < len(dbData); i++ {
        dbCall(i)
    }
    fmt.Printf("\nTemps total d'exécution : %v\n", time.Since(t0))
}

func dbCall(i int) {
    var delay float32 = rand.Float32() * 2000
    time.Sleep(time.Duration(delay) * time.Millisecond)
    fmt.Println("Résultat de la BDD :", dbData[i])
}
```

**Résultat :** Environ **5 secondes** d'exécution (la somme de tous les delais).

### 3. Lancer des Goroutines avec `sync.WaitGroup`

Le mot-clé `go` permet de lancer l'exécution d'une fonction de manière asynchrone. Le `sync.WaitGroup` sert de compteur pour attendre que toutes les Goroutines aient terminé avant d'arrêter le programme.

```
package main

import (
    "fmt"
    "math/rand"
    "sync"
    "time"
)

var wg = sync.WaitGroup{}
var dbData = []string{"id1", "id2", "id3", "id4", "id5"}
var results = []string{}

func main() {
    t0 := time.Now()
    for i := 0; i < len(dbData); i++ {
        wg.Add(1)    // +1 au compteur du WaitGroup
        go dbCall(i) // ⚠️ Le mot-clé 'go' est indispensable !
    }
    wg.Wait() // Attend que le compteur retombe à 0
    fmt.Printf("\nTemps total d'exécution : %v\n", time.Since(t0))
}

func dbCall(i int) {
    defer wg.Done() // -1 au compteur lors de la sortie de la fonction

    var delay float32 = rand.Float32() * 2000
    time.Sleep(time.Duration(delay) * time.Millisecond)
    
    fmt.Println("Résultat de la BDD :", dbData[i])
    
    // 🛑 ATTENTION : Race Condition ici !
    results = append(results, dbData[i])
}
```

**Problème :** Plusieurs Goroutines tentent de modifier la slice `results` au même moment en mémoire. Cela provoque une **Data Race** (corruption de données ou crash).

### 4. Proteger la mémoire partagée avec `sync.Mutex`

Un **`sync.Mutex`** (Mutual Exclusion) garantit qu'une seule Goroutine à la fois peut modifier une ressource partagée.

```
package main

import (
    "fmt"
    "math/rand"
    "sync"
    "time"
)

var m = sync.Mutex{}
var wg = sync.WaitGroup{}
var dbData = []string{"id1", "id2", "id3", "id4", "id5"}
var results = []string{}

func main() {
    t0 := time.Now()
    for i := 0; i < len(dbData); i++ {
        wg.Add(1)
        go dbCall(i)
    }
    wg.Wait()
    fmt.Printf("\nTemps total d'exécution : %v\n", time.Since(t0))
    fmt.Println("Résultats enregistrés :", results)
}

func dbCall(i int) {
    defer wg.Done()

    var delay float32 = rand.Float32() * 2000
    time.Sleep(time.Duration(delay) * time.Millisecond)
    
    fmt.Println("Résultat de la BDD :", dbData[i])

    // Section critique protégée
    m.Lock()
    results = append(results, dbData[i])
    m.Unlock()
}
```

**Résultat :** Le temps total passe de **5 secondes à ~2 secondes** (le délai de la tâche la plus longue).

### 5. Optimisation avec `sync.RWMutex` (Lecteurs / Écrivains)

Lorsque les opérations de **lecture** sont beaucoup plus fréquentes que les **écritures**, on utilise `sync.RWMutex` :

```
var rwMutex = sync.RWMutex{}

// Pour LIRE la donnée (plusieurs Goroutines peuvent lire en parallèle)
func readResults() {
    rwMutex.RLock()
    defer rwMutex.RUnlock()
    fmt.Println("Lecture des résultats :", results)
}

// Pour ÉCRIRE la donnée (accès exclusif à une seule Goroutine)
func writeResult(val string) {
    rwMutex.Lock()
    defer rwMutex.Unlock()
    results = append(results, val)
}
```

#### Règles du `sync.RWMutex` :

- **`RLock()` / `RUnlock()` :** Autorise **plusieurs lecteurs** simultanés.
- **`Lock()` / `Unlock()` :** Bloque **tous** les lecteurs et écrivains pour donner un accès exclusif à **un seul écrivain**.


