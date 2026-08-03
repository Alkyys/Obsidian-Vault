tout d'abord une fonction est déclere come ci dessous :
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