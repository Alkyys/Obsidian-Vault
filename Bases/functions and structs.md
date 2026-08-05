# Functions

tout d'abord une fonction est déclarer comme ci dessous :
`func myFunc() {
`// random stuff
`}

et elle est utiliser comme ci dessous :
`myFunc()

dans les fonctions on peux lui donner des paramères en entré et avoir des valeur en retour comme ci dessous :
`func myFunc(myPrameter string) int { // ici on attend un string en paramètre 
`// random stuff
`return myReturn
`}
ci dessous notre retour sera un int 

on pourra définir aussi plusieur retour de ma fonction
`func myFunc(myPrameter string) (int,int) { // ici on attend un string en paramètre 
`// random stuff
`return myReturn1, myReturn2
`}

il existe la bloucle for suivant cette syntax :
```
for i:=0; i<10; i++ {
	fmt.Println(i)
}
```

(mettre définition des fonctions, switch, if else)

# Structs

les struc serve,t a définit un type a nous par exemple de type de moteur comme ceci :
```
package main
import "fmt"

type gasEngine struct{
		mpg uint8
		gallons uint8 
		owner
}
	
type owner struct{
	name string
} 

func main(){
	var myEngine gasEngine = gasEngine{25, 15, owner{"Alex"}
	fmt. Println(myEngine.mpg, myEngine.gallons, myEngine.owner.name)
}

```

## Méthodes
En Go, il n'y a pas de classes. Une **méthode** est simplement une fonction rattachée à un type spécifique (généralement une `struct`) grâce à un **receveur** (_receiver_).
nous pouvons mettre des méthode au sein de struct, ce sont des fonction propre au struct, dans notre exemple plus haut nous voulons crée un méthode pour calculer le nombre de kilometre restant :
```
func (e gasEengine) mileLeft() uint8{
	return e.gallons*e.mpg
}

```

# Interface 
les interfaces sont (a remplir)
Une **interface** définit un ensemble de signatures de méthodes. En Go, le polymorphisme s'applique **implicitement** : si un type possède toutes les méthodes définies par l'interface, il implémente automatiquement cette interface (pas besoin de mot-clé `implements`).

```
// Définition de l'interface
type Forme interface {
    Aire() float64
}

// Tout type possédant une méthode Aire() float64 sera considéré comme une Forme
```
