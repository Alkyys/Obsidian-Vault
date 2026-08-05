les erreurs sont des variables, leur valuers par défault est `nil`
elle se déclare comme ci-dessous :
`var err error

pour utiliser cette valeur il faut importre un package lui correspondant :
`import "errors"

on a pour habitude pour les fonction de retourner régulièrement une err pour voir si la fonction s'est bien exécuté ou s'il y a une erreur de pouvoir la comprende et agir en conséquence 

**Exemple**
```
func intDivision(numerator int, denominator int) (int, int, error {
	var err error
	if denominator==01
		err = errors.New("Cannot Divide by Zero")
		return 0, o, err
	}
	var result int = numerator/denominator
	var remainder int = numerator%denominator
	return result, remainder, err
}
```
