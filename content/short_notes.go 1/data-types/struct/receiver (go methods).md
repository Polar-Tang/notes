A receiver is a special parameter that defines a method in a TYPE!
So first let's create a type
```go
type rect struct {
	width int 
	height int
}
```
So we define a function
```GO
func () int {
	return 0
}
```
We can define a regular function that utilize this type to do a computation:
```go
func area(r rect) int { // Regular function

    return r.width * r.height

}

var r = rect{
	width: 5, 
	height: 10
}

func main() {

    fmt.Println(area(r)) // 50

}
```
Now the syntax here will be a little bit confusing, but we could provide a function to this type that we defined, Like a special parameter that syntactically goes _before_ the name of the function. And this work pretty similar to a method in the OOP of JavaScript.
We provide a function of this type:
```go
func (r rect) area() int { // Method
    return r.width * r.height
}

var r = rect{
	width: 5, 
	height: 10
}

func main() {

    fmt.Println(area(r)) // 50

}

```

#### Example of usage
```go
package main

type Message interface {
        Type() string
}

type TextMessage struct {
        Sender  string
        Content string
}

func (tm TextMessage) Type() string {
        return text
}

type MediaMessage struct {
        Sender    string
        MediaType string
        Content   string
}

func (mm MediaMessage) Type() string {
        return media
}

type LinkMessage struct {
        Sender  string
        URL     string
        Content string
}

func (lm LinkMessage) Type() string {
        return link
}

// Don't touch above this line

func filterMessages(messages []Message, filterType string) []Message {
        var filteredMessages []Message
        for _, msg := range messages {
                if msg.Type() == filterType {
                        filteredMessages = append(filteredMessages, msg)
                }
        }
        return filteredMessages
}

```
#### Receiver with args
```go
type User struct {
	Name string
	Membership
}

type Membership struct {
	Type string
	MessageCharLimit int
}

func (u User) SendMessage(msg string, msglen int) (string, bool) {
	if msglen <= u.Membership.MessageCharLimit {
		return msg, true
	}
	return "", false
}
  
func newUser(name string, membershipType string) User {
	membership := Membership{Type: membershipType}
	if membershipType == "premium" {
		membership.MessageCharLimit = 1000
	} else {
		membership.Type = "standard"
		membership.MessageCharLimit = 100
	}
	return User{Name: name, Membership: membership}
}
```
As you may see the interface is implicit, there's nothing inside the interface that specify which thing will use it. Actually is the function, when i do `(r rect)` that's what binds the interface, the `area` function, to the `rect` type.
The receiver `(r rect)` in Go plays the role of `self` in Python or `this` in Java.
```python
class Rect:
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):  # This is similar to Go's method with a receiver
        return self.width * self.height

r = Rect(5, 10)
print(r.area())  # Calls the 'area' method

```
The resiver could also get parameters:

```go
func (u User) SendMessage(message string, messageLength int) (string, bool) {
    if u.MessageCharLimit >= messageLength {
        return message, true
    }
    return "", false

}
```
