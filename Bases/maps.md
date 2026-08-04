il s'agit uen structuer de stakage de donné se basent sur le notion de key value `{"key":"value"}`.Les clés doivent être uniques. on peut déclarer cette structure comme ceci `map[string]uint8` ici on a des key en string et des value en unit8. oon peut initialiser directement les avec des valeur comme ceci `var myMaps2 = map[string]uint8{"Adam":23, "Moi":34}`. 

remarque si on essaye de récupérer une donnée qui n'existe pas, par exemple ici `"Sam"` on obtiendra la valeur par défauld du type, dans ce ca 0. 
attention un map renvoira toujours quelque chose.
mais go a prévu ce cas et on peux verifier si il y a bien un élément. go renvoir en plus du résultat un boolean, celui ci sert a vérifier si lélement existe voici un exemple : 
`var age,ok - muMap2["Sam"] // result : 0,false`
`var age,ok - muMap2["Adam"] // result : 23,true`
au passage on crée implicitement un uint8 age

on peut supp des element avec la fonction delete(), comme par exemple `delete(myMap2,"Adam")` cela supprimera par reference donc aucune valeur de retour sera donné. 

On peut aussi parcourir une map avec une boucle for :
```
for name:+ rage myMap2{
	fmt.Printf("Name: %v", name)
}
```

attention l'ordre des elements peuvent changer il est pas constant 
