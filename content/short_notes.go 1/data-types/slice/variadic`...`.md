it's used to accept an undefined quantity of values from an argument type.
#### For example
```go
func concat(strs ...string) string
```
This could concatenate zero or more types, so whatever quantity you pass to the function it will accept it
We could use [[append]] to dinamically add some content
```go
package main

func sum(nums ...int) int {
	total := 0
	for _, num := range nums {
		total += num
	}
	return total
}
```
### Spread operator
If i do not have a variadic i should create a for to iterate and search all over the thing.
```go
func printStrings(strings []string) {
    for i := 0; i < len(strings); i++ {
        fmt.Println(strings[i])
    }
}

printStrings([]string{"bob", "sue", "alice"}) // Always pass a slice
```
But with this i could print the things more easily:
```go
names := []string{"bob", "sue", "alice"}
printStrings(names...)
```
