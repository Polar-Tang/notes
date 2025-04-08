An interface can recive arguments
```go
type Copier interface {
  Copy(string, string) int
}
```
And also we could name those arguments
```go
type Copier interface {
  Copy(sourceFile string, destinationFile string) (bytesCopied int)
}
```