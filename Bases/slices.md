les slice s sont des tableau qui sont dynamique c'est a dire que leur taille peuvent varier en fonction de (a remplir)


### Exemple 
`// Déclaration d'une slice de chaînes
`fruits := []string{"pomme", "banane"}

`// Ajout d'un élément a la fin
`fruits = append(fruits, "orange")


attention il faut bien différentier capacity et Lenght `[4,5,6,7,*,*]`ici on a une lenght de 4 et une Capacity de 6

on peut crée une slice avec make() comme ceci : `var intSlice3 []int32 = make(int32[],3,8)` ici on crée un slice de int32 de lenght 3 et de capacity 8. cette pratique vide a ce que le systeme de réaloupas d'emplacement mémoir et ce la peut énormément impacter des performance.


(définir comment la copie des slices se passe, car on fait une copie d'adresse et non de valeur)