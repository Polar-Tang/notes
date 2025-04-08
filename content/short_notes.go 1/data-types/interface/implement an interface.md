THe only way that a struct implements an interface is by using all the methods defined in that interface.
For example let' get this interface:
```go
type employee interface {
	getName() string
	getSalary() int
}
```
Now if we want the constructor type to apply this interfaces we don't use "implements" keyword.
```go
type contractor struct {
	name string
	hourlyPay int
	hoursPerYear int
}
```

To implement the emplye interfaces we need to define all the methods to this type.
```go
func (c contractor) getName() string {
	return c.name
}
  
func (c contractor) getSalary() int {
	return c.hourlyPay * c.hoursPerYear
}
```
