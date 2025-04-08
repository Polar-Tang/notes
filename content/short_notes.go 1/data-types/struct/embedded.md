A struct is embedded when is reference in another kind of object:
```go
type car struct {
  brand string
  model string
}

type truck struct {
  // "car" is embedded, so the definition of a
  // "truck" now also additionally contains all
  // of the fields of the car struct
  car
  bedSize int
}
```
Now truck has the same car properties, but the bedSize additionally. just like the extends keyword in the most of POO languages
### Nested struct (composite literal)
```go
lanesTruck := truck{
  bedSize: 10,
  car: car{
    brand: "Toyota",
    model: "Camry",
  },
}

fmt.Println(lanesTruck.brand) // Toyota
fmt.Println(lanesTruck.model) // Camry
```