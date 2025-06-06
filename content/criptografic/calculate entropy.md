There's a measure of uncertain, where 0 means never and 1 means always.
To calculate the entropy, first we need to know the probabillity.

### The probabillity
A probability distribution must include all possible outcomes, if we got a cyphertext of *N* bytes we should iterate like: p1, p2, . . . , pN computing the frequency of each byte, and this outcome divided by the length  
![[Pasted image 20241230222107.png]]
Where X is the frequency of bytes and its divided by the length of the total bytes.
p(xi) = count of xi/total number of bytes in the cyphertext
### Entropy
the entropy is the negative sum of all the probabilities multiplied by its logarithm
	−p1 × log(p1) − p2 × log(p2) − ... pN × log(pN)
For example a coin with a tail and head is 1 predictable:
−(1/2) × log (1/2) − (1/2) × log (1/2) = 1/2 + 1/2 = 1 bit
For the probability: 1 is the number that the byte appears, and 2 is the length (head and tail), then is just do the sum with the logarithm
Now an uniformed distributed random key (which means the character appear with the same frequency) a random 128 bits key has the following possibilities, as each one appears once the frequency with each byte get repeated is one:
	2^-128 × log(2^-128) * (-2^-128 × log(2^-128))

Here's a script:
```go
func calculateEntropy(data []byte) float64 {
	// frequency for each byte
    frequency := make(map[byte]float64)
    for _, b := range data {
        frequency[b]++
    }

    var entropy float64
  
    for _, count := range frequency {
        probability := count / float64(len(data))
        entropy -= probability * math.Log2(probability)
    }

    return entropy
}
```


Hello. I'm improving my authentication token, and reading about authentication, and its related OWASP top ten topic. One of the first thing i read about in the conhague book from Pilcrow (A developer kwown by have been developed Oslo and another projects related to the authentication topic and criptography), know i'm reading about tokens, and Pilcrow says "Tokens should have at least 112 bits of entropy (120-256 is a good range)". I remember to read a little bit about criptography before, so i search "entropy" in my notes, i find a note titled "calculate entrophy", which goes as follows:

"""

To calculate the entropy, first we need to know the probabillity.

### The probabillity

A probability distribution must include all possible outcomes, if we got a cyphertext of *N* bytes we should iterate like: p1, p2, . . . , pN computing the frequency of each byte, and this outcome divided by the length

"""

To calculate the probability i need to numerate the frequency of bytes in the cyphertext.

"""

Where X is the frequency of bytes and its divided by the length of the total bytes.

p(xi) = count of xi/total number of bytes in the cyphertext

  

### Entropy

the entropy is the negative sum of all the probabilities multiplied by its logarithm

    −p1 × log(p1) − p2 × log(p2) − ... pN × log(pN)

"""

Now i wonder how to calculate the entropy. They mention that is the negative sum of all the probabilites multiplied by its logarithm, so if i need to sum all the probabillities, as all the posibilities represents a byte, i need to calculate 256 differents logarithms, because all the probabilities are 256 for each byte, this are the ASCII ones, which is a single byte, am i right?

How would you calculate the entropy?