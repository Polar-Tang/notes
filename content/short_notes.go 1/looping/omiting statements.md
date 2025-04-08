We could omit statements in the [[for(c-like)]] and this will work, for example, here we got an infinite loop:
```go
for INITIAL; ; AFTER {
  // do something forever
}
```
### Example case
Suppossing we are building an app that should calculate the total messages for a given saldo
```go
package main
  
func maxMessages(balance int) int {
  
	var cents int
	for msgNum := 0; ; msgNum++ {
		cents += 100 + msgNum
		if cents >= balance {
			return msgNum
		}
	}
}
```
