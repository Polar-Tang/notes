A byte are the bytes of whatever strings. You will usually a `[]byte`
```go
simpleString := "Hello, World!"
data := []byte("Hello, World!")
fmt.Println(len(data))         //13
fmt.Println(len(simpleString)) //13
```