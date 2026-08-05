les goroutines sont le moyen de lancer plusieur fonction pour quelle s'execute en meme temps, physiquement sur les serveurs les routine s'exececute su rles coeur différents. tout d'abord la simultanéité et différent que la paralilisation, simultanéité revient a faire qu'on peut mettre des fonction en "pause" le temps d'avoir un résultat puis le reprendre plus tard. alors que la paralilisation on va tout faire en meme temps.

prenons un exemple pour parler de tout cela, ci dessous nous avons 5 appel de base de donnée `dbCall (i)` qui durent envison 2sec `var delay float32 = rand.Float32()*2000`

```
package main
  
import (
	"fmt"
	"math/rand"
	"time"
)

var dbData = []string{"id1", "id2", "id3", "id4", "id5"}

func main(){
	t0 := time.Now()
	for i:=0; i<len(dbData); i++{
		dbCall(i)
	}
	fmt.Printf("\nTotal execution time: %v", time.Since(t0))
}

func dbCall(i int) {
	// Simulate DB call delay
	var delay float32 = rand.Float32()*2000
	time.Sleep(time.Duration(delay)*time.Millisecond)
	fmt.Println("The result from the database is:", dbData[i])
}
```
 ici les appels sont fait a la suite, on obtient :
 ```
The result from the database is: id1
The result from the database is: id2
The result from the database is: id3
The result from the database is: id4
The result from the database is: id5

Total execution time: 4.94061975s%  
 ```

maintenant on va optimiser cela ! pour se faire nour allons utiliser go lors de l'execution de l'appel de BD cela permet de lancer les execution simultanément. c'est a dire qu'il attent pas le retour de la fon,ction pour passer a l'instrcution suivante. mainteant on doit faire en sorte que le programme attende un résultat de l'appel de donnée. 
tout d'abord nous avons besoin du package "sync" et nous allons utiliser les Wait groups `var wg = sync.WaitGroup{}`, pour résumer ce sont de simple compteur, a chaque fois qu'on va faire une go routine nous allons rajouter 1 au compteur `wg.Add(1)` et une fois terminer on appel la fonction done comme ceci : `wg.Done()` cela décrément le compteur. et enfin nous avons la fonction `wg.Wait()` qui permet d'attendre que les go routine termine.

alors c'est parti maintenant on va intégrer ces principe a notre programme, de plus nous allons rajouter une slice pour ranger nos résultats
```
package main
  
import (
	"fmt"
	"math/rand"
	"time"
	"sync"
)

var wg = sync.WaitGroup{}
var dbData = []string{"id1", "id2", "id3", "id4", "id5"}
var results = []string{}

func main(){
	t0 := time.Now()
	for i:=0; i<len(dbData); i++{
		wg.Add(1)
		dbCall(i)
	}
	wg.Wait()
	fmt.Printf("\nTotal execution time: %v", time.Since(t0))
}

func dbCall(i int) {
	// Simulate DB call delay
	var delay float32 = rand.Float32()*2000
	time.Sleep(time.Duration(delay)*time.Millisecond)
	fmt.Println("The result from the database is:", dbData[i])
	results = append(results, dbData[i])
	wg.Done()
}
```

Mais ici on va faire face a un nouveau probleme de taille, la corruption de donnée. car nous essayons de modifer les meme espace memoire en meme temps. si nous ne fesons rien nous allons avoir des resultats surprenant.
Pour eviter ces soucis nous utilisons les Mutex (abréviation de mutual exclusion). les methode principale sont Lock() et Unlock(). nous la mettrons a l'endrois ou nous voulons ranger les résultat de base de donnée 

```
package main
  
import (
	"fmt"
	"math/rand"
	"time"
	"sync"
)

var m = sync.Mutex{}
var wg = sync.WaitGroup{}
var dbData = []string{"id1", "id2", "id3", "id4", "id5"}
var results = []string{}

func main(){
	t0 := time.Now()
	for i:=0; i<len(dbData); i++{
		wg.Add(1)
		dbCall(i)
	}
	wg.Wait()
	fmt.Printf("\nTotal execution time: %v", time.Since(t0))
}

func dbCall(i int) {
	// Simulate DB call delay
	var delay float32 = rand.Float32()*2000
	time.Sleep(time.Duration(delay)*time.Millisecond)
	fmt.Println("The result from the database is:", dbData[i])
	m.Lock()
	results = append(results, dbData[i])
	m.Unlock()
	wg.Done()
}
```

ici losrqu'une go routine arrive a  Lock() elle va vérifier si une autre go routine a déja vérouiller si c'est le cas elle va attendre pour éviter de modifier le meme espace memoire au meme moment. et une fois terminer elle va faire appel a la méthode Unlock() pour laisser la place libre a d'autre go routine.

il existe aussi les methode `RLock()` et `RUnlock()` le R signifie Read, cela permet de pouvoir gerer les les fonction qui veulent lire les donnée et celles qui veulent modifier des donnée. le systeme est fait pour que plusiuer fonction puissent lire en meme temps et ne pas interférer avec celle qui modifie les data (attention a vérifier)