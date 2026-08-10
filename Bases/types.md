comme dit précédemment les types sont définit et ne peuvent pas etre changer, les variable doivent etre obligatoirement utiliser sinon on se retrouve avec une erreur  

exemple de type de declaration de variable
`var x int`
`x = 16 // ici on déclare seulement la variable `
`y:= 17 // ici on déclare et on assigne la varible`

on peut aussi déclarer un type assigner une variable comme ceci aussi :
`var myVar = "text" // Forme explicite (le type string est inféré)
`myVar := "text"    // Forme courte (réservée à l'intérieur des fonctions) `
on peut aussi faire cela de manière multiple comme ceci :
`var var1, var2 int = 1,2`
ou 
`var1, var2 := 1,2`

`const myConst = 10 // Valeur fixée à la compilation, obligatoire dès la déclaration
`var myVar // comme dit dans son type elle peut varier mais pas changer de type

attention on peux pas forcément mélanger les différent type, par exemple je ne peux pas faire des opérations avec un int et un float, (biensur il existe une solution c'est de transformer le int en float )


### INT
pour la déclaration de int par défaut il sera de 32 ou 64 bits en fonction du systme,
pour un int16 on peut aller jusqu'a 32767, si on lui rajoute un on aura un overflow a l'éxécution mais pas a la complilation (cela se traduit pas par une erreur).
`var x int16 = 32767
`x = x + 1 // Pas d'erreur à l'exécution ! x vaut maintenant -32768

int8 peut aller de -128 a 127 et 
Les uint cest la meme que des int sauf que c'est que les nombre positof qui peuvent etre stocker
uint8 ce n'est que des chiffres positifs qui peux etre assigés (0,255)

la valeur par défault (c'est a dire qu'on ne l'assigne pas ) est a 0
### float 
Pour stocker les chiffres a virgule on utilise les float, il existe deux type float32 et float64

la valeur par défault (c'est a dire qu'on ne l'assigne pas ) est a 0
#### bool
c'est une variable qui peut que etre true ou false 

la valeur par défault (c'est a dire qu'on ne l'assigne pas ) est a false
### string
les string sont utiliser pour stoker des chaines de caratères, petite particularité, si on venais a utiliser la fonction len() sur une straing on aura en retour la place qu'elle prend dans la mémoire. si on est dans un cas ou on a besoin de savoir le nombre de caratère dans notre string, on va utiliser la packaging unicode/utf8 et on va utiliser la fonction RuneCountInString() 

la valeur par défault (c'est a dire qu'on ne l'assigne pas ) est a "" (chaine de caratère vide)

alors pour la suite nous uliseron une chaine de caractère suivante `var myString = "résumé"` pour savoir le nombre de caratère si nous utilisons cette technique suivant nous obtenons une résultat surprenant. 
```
func main(){
	var myString = "résumé"
	var indexed = myString[0]
	fmt.Printf("%v, %T\n", indexed, indexed)
	
	for i, v := range myString{
		fmt. Println(i, V)
	}
}
```
ici on obtient :
0 114
1 233
3 115
4 117
5 109
6 233

ici on remarque que le 2 nexiste pas, ici le é prend plus de 7 car byts (ici on compte de nombre de byte et non le nombre de caratère d'une string)

Ici si nous voulont compter le nombre de caratère, il faut utiliser un package particulier 
`` (a completer)

pour la gestion de string Go a prévu un package "string" qui nous ervira a optimiser les performance car le type string est in modifiable, pour e faire on est oblider concat des strings pour en recrée une. comme ci dessous :
```
package main

import (
	"fmt"
	"strings"
	"unicode/utf8"
)

func main() {
	myString := "résumé"

	// Nombre d'octets vs Nombre de caractères
	fmt.Println(len(myString))                    // Output: 8 (octets)
	fmt.Println(utf8.RuneCountInString(myString)) // Output: 6 (caractères)

	// Optimisation de concaténation (les strings étant immuables)
	var strSlice = []string{"s", "u", "b", "s", "c", "r", "i", "b", "e"}
	var strBuilder strings.Builder

	for _, char := range strSlice {
		strBuilder.WriteString(char)
	}

	catStr := strBuilder.String()
	fmt.Println(catStr) // Output: subscribe
}
```


### rune
Une `rune` n'est **pas du tout un nombre à virgule** (float). C'est un **point de code Unicode (un entier)**. De plus, c'est un alias pour **`int32`** (entier signé 32 bits),
- La `rune` est un entier (`int32`) représentant un caractère Unicode (ex: `'a'`, `'é'`, `'😀'`).

la valeur par défault (c'est a dire qu'on ne l'assigne pas ) est a 0


