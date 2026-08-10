a canal (channel) est un endroit ou ils garder les infos, on evite la couses de donnée entre 

ok c'est parti pour un exemple :
```
pacakge main

func main(){
	var c = make(chan int) // ici dans le channel on sticketa qu'un int
	c <- 1 // ici on ajoute 1 
}
```
 ici nous avons un channel sans tampon c'est a dire qu'il y a une place que pour une valeur.
 `var i = <- c // et on transfert le contenant du channel `
 ici on va crée un variable et on transfert le contenant du channel. apres cette ligne le channel c est vide avec un espace de mémoire libre.
ici on se retrouver aavec une erreur (expliquer le probleme), pour bien utiliser les channel il faut les combiner au go routine 

```
package main

import (
	"fmt"
	"math/rand"
	"time"
)

var MAX_CHICKEN_PRICE float32 = 5

func main(){
	var chickenChannel = make(chan string)
	var websites = []string("walmart.com", "costco.com", "wholefoods.com"}
	for i:= range websitest{
		go checkChickenPrices(websites[1], chickenChannel)
	｝
	sendMessage(chickenChannel)
}

func checkChickenPrices(website string, chickenChannel chan string){
	for {
		time.Sleep(time.Second*1)
		var chickenPrice = rand.Float32()*20
		if chickenPrice<=MAX_CHICKEN_PRICE{
			chickenChannel| <- website 
			break
		}
	}	
}

func sendMessage(chickenChannel chan string){
	fmt.Printf("\nFound a deal on chicken at %s", <-chickenChannel)
｝
```

(explique ligne par lice qui se passe)



