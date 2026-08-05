le but de ce document est de rédiger tout le savoir que j'apprend pour le langage Go

## Définition
le Go est un langage de programmation crée par google, c’est un langage compilé c’est a dire qu’il est d'abord réduit en binaire avant d’étre exécuté. c’est un langage orienté objet et statistiquement typé (c’est a dire que les variables ont un type définit comme un chiffre ou chaine de caratère; ce type ne peux pas etre changer). c'est un langage open source.

## Hello world 
`package main`
`import "fmt" // commentaire
`func main() {
	`fmt.Printf("Hello word")
`}

## Bases
[[types]]
[[arrays]]
[[slices]]
[[maps]]
[[functions and structs]]
[[pointeurs]]
[[comparators]]

## Gestion d'erreurs
[[Errors]]

## Concurrence — introduction
[[goroutines]]
[[channels]]
[[select]]
[[sync.WaitGroup]]
[[sync.Mutex]]