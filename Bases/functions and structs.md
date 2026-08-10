
### Déclaration et Retours Multiples

Une fonction peut prendre zéro ou plusieurs paramètres et retourner zéro ou plusieurs valeurs. En Go, le retour d'une valeur d'erreur en second argument est la convention standard.

```
// Fonction simple sans retour
func myFunc() {
    // code
}

// Paramètre et retour simple
func myFunc(parameter string) int {
    return 42
}

// Retours multiples (Idiome très courant en Go pour gérer les erreurs)
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, fmt.Errorf("division par zéro impossible")
    }
    return a / b, nil
}
```

### Structures de Contrôle

#### A. Les Conditions (`if` / `else`)

Go permet d'exécuter une courte instruction d'initialisation directement dans la condition du `if` (la variable déclarée n'existe que dans le bloc du `if`).

```
// Structure classique
if x > 10 {
    fmt.Println("Grand")
} else if x == 10 {
    fmt.Println("Égal")
} else {
    fmt.Println("Petit")
}

// Initialisation courte dans le if (idiome très utilisé)
if val, err := divide(10, 2); err == nil {
    fmt.Println("Résultat :", val)
}
```

#### B. La Boucle Unique (`for`)

En Go, `for` est la **seule** boucle disponible. Elle prend 3 formes principales :

```
// 1. Forme classique (Initialisation; Condition; Post-instruction)
for i := 0; i < 10; i++ {
    fmt.Println(i)
}

// 2. Forme "while" (seulement la condition)
n := 1
for n < 5 {
    n *= 2
}

// 3. Boucle infinie
for {
    // s'arrête avec un 'break' ou un 'return'
    break 
}
```

#### C. Le `switch`

Évite d'enchaîner des `if/else`. Il s'arrête automatiquement au premier cas validé (pas besoin de `break`).

```
day := "lundi"

switch day {
case "lundi", "mardi":
    fmt.Println("Début de semaine")
case "vendredi":
    fmt.Println("Bientôt le week-end")
default:
    fmt.Println("Milieu de semaine")
}
```

## 2. Les Structs (Structures)

En Go, il n'y a pas de classes ni d'héritage. On utilise des **Structs** pour regrouper des données et créer nos propres types typés.

```
package main

import "fmt"

type owner struct {
    name string
}

type gasEngine struct {
    mpg     uint8
    gallons uint8
    owner   owner // Champ imbriqué (composition)
}

func main() {
    // Instanciation recommandée avec les noms de champs
    var myEngine = gasEngine{
        mpg:     25,
        gallons: 15,
        owner:   owner{name: "Alex"},
    }

    fmt.Println(myEngine.mpg, myEngine.gallons, myEngine.owner.name)
}
```

## 3. Les Méthodes

Une **méthode** est une fonction rattachée à un type (souvent une struct) via un **receveur** (_receiver_).

```
// Receveur par valeur (Value Receiver) : travaille sur une copie
func (e gasEngine) milesLeft() uint16 {
    return uint16(e.gallons) * uint16(e.mpg)
}

// Receveur par pointeur (Pointer Receiver) : modifie la struct d'origine
func (e *gasEngine) refuel(amount uint8) {
    e.gallons += amount
}

func main() {
    myEngine := gasEngine{mpg: 25, gallons: 10}
    
    fmt.Println("Km restants :", myEngine.milesLeft()) // 250
    
    myEngine.refuel(5) // Modifie directement le champ gallons
    fmt.Println("Nouveaux km restants :", myEngine.milesLeft()) // 375
}
```

## 4. Les Interfaces

Une **interface** définit un contrat (une liste de signatures de méthodes).

### Principes clés :

- **Implémentation implicite :** En Go, un type n'a pas besoin de déclarer `implements MaInterface`. Il suffit qu'il possède toutes les méthodes définies par l'interface.
- **Polymorphisme :** Permet à une fonction d'accepter n'importe quel type satisfaisant l'interface.

```
package main

import "fmt"

// 1. Définition du contrat
type Forme interface {
    Aire() float64
}

// 2. Types de données
type Rectangle struct {
    Largeur, Hauteur float64
}

type Cercle struct {
    Rayon float64
}

// 3. Implémentation implicite pour Rectangle
func (r Rectangle) Aire() float64 {
    return r.Largeur * r.Hauteur
}

// 4. Implémentation implicite pour Cercle
func (c Cercle) Aire() float64 {
    return 3.14 * c.Rayon * c.Rayon
}

// 5. Fonction polymorphe qui accepte toute 'Forme'
func afficherAire(f Forme) {
    fmt.Println("L'aire est de :", f.Aire())
}

func main() {
    r := Rectangle{Largeur: 10, Hauteur: 5}
    c := Cercle{Rayon: 3}

    afficherAire(r) // Output: L'aire est de : 50
    afficherAire(c) // Output: L'aire est de : 28.26
}
```