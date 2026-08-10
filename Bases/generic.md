ici on va expliquer la gestion des génériques

```
package main

import "fmt"

func main(){
	var intSlice = []int{1, 2, 3}
	fmt.Println(sumSlice[int](intSlice))
	
	var float32Slice = []float32{1, 2, 3}
	fmt.Println(sumSlice[float32](float32Slice))
}


func sumSlice[T int | float32 | float64](slice []T) T{
	var sum T
	for _,v := range slice{
		sum += v
	}
	return sum
}
```

explique ligne par ligne l'utilisation des géneriques, (rajoute l'explication any dans go)
