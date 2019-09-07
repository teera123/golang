## Understanding GOPATH/GOROOT
```
bin/
    hello                           # command executable
    outyet                          # command executable
pkg/
    linux_amd64/
        github.com/golang/example/
            stringutil.a            # package object
src/
    github.com/golang/example/
        .git/                       # git repository metadata
        hello/
            hello.go                # command source
        outyet/
            main.go                 # command source
            main_test.go            # test source
        stringutil/
            reverse.go              # package source
            reverse_test.go         # test source
    golang.org/x/image/
        .git/                       # git repository metadata
        bmp/
            reader.go               # package source
            writer.go               # package source
    ...            
```

## Golang leaves out many features
- No classes
- No constructors
- No inheritance
- No exceptions

## Hello World
```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello, world!");
    }
}
```

```go
package main

import "fmt"

func main() {
	fmt.Println("Hello, world!")
}
```
https://play.golang.org/p/BYzfGNSzsU1

## Variables
```go
package main

import "fmt"

const fourth = "constant"

var third = "XXXX"

func main() {
	var first string
	first = "hello"
	
	second := "world"
	third = "change value"
	
	fmt.Printf("%s %s, %s %s\n", first, second, third, fourth)
}
```
https://play.golang.org/p/TRb_23ytw_-

## Array & Slices
```go
package main

import (
	"fmt"
)

func main() {
	var str [2]string
	str[0] = "foo"
	str[1] = "bar"

	//str := []string{"foo", "bar"}

	for i, s := range str {
		fmt.Println(i, s)
	}

	numbers := []int{10, 20, 30, 40}
	for i, n := range numbers {
		fmt.Println(i, n)
	}

	fmt.Println(numbers[1:])
	fmt.Println(numbers[3:])
	fmt.Println(numbers[:1])
	fmt.Println(numbers[:3])
	fmt.Println(numbers[2:3])
	fmt.Println(numbers[1:4])
}
```
https://play.golang.org/p/EveuAPDkQtK

### workshops
split your own citizen id to slices, print out only last 4 digits

## Type conversions
```go
package main

import (
	"encoding/json"
	"fmt"
	"reflect"
	"strconv"
	
	"github.com/shopspring/decimal"
)

func main() {
	str := "10.00"
	f, err := strconv.ParseFloat(str, 64)
	if err != nil {
		fmt.Println("parse float error:", str, err)
		return
	}
	fmt.Println("float:", f)
	
	// suggest for money conversion
	price, err := decimal.NewFromString(str)
	if err != nil {
	    fmt.Println("parse decimal error:", str, err)
	    return
	}
	fmt.Println("price:", price)
	
	var it interface{} = "15"
    data, ok := it.(string)
    
    fmt.Printf("%T: %v\n", reflect.TypeOf(it).String(), it)
    fmt.Printf("%T: %s\n", data, data)
    fmt.Printf("%T: %t\n", ok, ok)
    
    u := struct{X string `json:"data"`}{"abcd"}
    ud, err := json.Marshal(u)
    if err != nil {
        fmt.Println("json marshal:", u, err)
        return
    }
    fmt.Printf("%T: %v, %s\n", ud, ud, string(ud))
}
```
https://play.golang.org/p/6l-XE7WllCk

## Functions
```go
package main

import "fmt"

func main() {
	a, b := swap("hello", "world")
	fmt.Println(a, b)
}

func swap(x, y string) (string, string) {
	return y, x
}
```
https://play.golang.org/p/W8mzp069HS_1

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println("join string 1:", joinString1("apple", "banana", "kiwi"))
	fmt.Println("join string 2:", joinString2("apple", "banana", "kiwi"))

	fmt.Println("append array 1:", appendArray1([]string{"apple", "banana"}, "kiwi", "orange"))
	fmt.Println("append array 2:", appendArray2([]string{"apple", "banana"}, "kiwi", "orange"))
}

func joinString1(str ...string) string {
	var a []string
	for _, s := range str {
		a = append(a, s)
	}
	return strings.Join(a, ",")
}

func joinString2(str ...string) string {
	return strings.Join(str, ",")
}

func appendArray1(base []string, str ...string) []string {
	for _, s := range str {
		base = append(base, s)
	}
	return base
}

func appendArray2(base []string, str ...string) []string {
	return append(base, str...)
}
```
https://play.golang.org/p/q6hgllad7_O

```go
package main

import "fmt"

func main() {
	defer func() {
		fmt.Println("run when found return or end of function")
	}()

	i := 10
	fmt.Println("i:", i)
	
	print := func(str string) {
		fmt.Println("print from anonymous function!", str)
		fmt.Println("i can be access here:", i)
	}
	
	i++
	print("foo")
}
```
https://play.golang.org/p/RuqgmuYoeNC

```go
package main

import "fmt"

func main() {
    f := func(n int) int {
        return 2 * n
    }
    fmt.Printf("f(21) = %v\n", f(21))
}
```
https://play.golang.org/p/GQ7Fj1Zyw7v

```go
package main

import "fmt"

func applyFunction(n int, f func(int) int) {
    fmt.Printf("f(%v) = %v\n", n, f(n))
}

func main() {
    f := func(n int) int {
        return 2 * n
    }
    applyFunction(21, f)
}
```
https://play.golang.org/p/0yemqRPQYDN

## Methods
```go
package main

import "fmt"

type rectangle struct {
	height float64
	width  float64
}

func (r rectangle) area() float64 {
	return r.width * r.height
}

func main() {
	square := rectangle{
		height: 4.0,
		width:  4.0,
	}
	fmt.Printf("square has area of %v", square.area())
}
```
https://play.golang.org/p/oY7kkvv55dq

## Struct
```go
package mylib

import "fmt"

const (
	Const1 = "constant 1"
	const2 = "constant 2"
)

var (
	Var1 = 1
	var2 = 2
)

type MyStruct struct {
	X int // public
	y int // private
}

func (m MyStruct) DoAPI() { // public
    fmt.Println("DoAPI", Const1, Var1)
}

func (m MyStruct) doAPI() { // private
	fmt.Println("doAPI", const2, var2)
}
```

## Panic & Recovery
```go
package main

func main() {
	panic("PANIC")
	
	// panic: PANIC
	// 
	// goroutine 1 [running]: 
	// main.main() 
	// 	/tmp/sandbox424231028/main.go:4 +0x40
}
```
https://play.golang.org/p/jkZvxfkRkQZ

** panic also stop service

```go
package main

import "fmt"

func main() {
	defer func() {
		str := recover()
		fmt.Println("recover:", str)
	}()

	panic("PANIC")
}
```
https://play.golang.org/p/kIt9E2u8bLN

## For-loop
```go
package main

import "fmt"

func main() {
	for i := 1; i <= 10; i++ {
		fmt.Print(i) // print: 12345678910
	}
	
	arr := []string{"a", "b", "c"}
	for i, a := range arr {
		fmt.Printf("%d:%s ", i, a) // print: 0:a 1:b 2:c
	}
	fmt.Println("len:", len(arr))
	
	data := map[string]string{"a": "1", "b": "2", "c": "3"}
	for k, v := range data {
		fmt.Printf("%s:%s ", k, v) // print: a:1 b:2 c:3 
	}
	
	// while loop
	sum := 1
	for sum < 10 {
		fmt.Print(sum)
		sum++
	}
	
	for {
		fmt.Print(sum)
		if sum >= 10 {
			break
		}
		sum++
	}
	
	// forever loop
	i := 0
	for {
		fmt.Print(i)
		i++
	}
}
```
https://play.golang.org/p/LiNDLRlh6_T

### workshops
1. loop sum int by 5, print and return when hit 100 
2. create an array 1..100, print only even value

## json/xml
```go
package main

import (
	"encoding/json"
	"fmt"
)

func main() {
	user := struct{
		FirstName string
		LastName  string
	}{"foo", "bar"}
	
	data, err := json.Marshal(user)
	if err != nil {
		fmt.Println("json marshal:", user, err)
		return
	}
	fmt.Println(string(data))
	
	d := `{"email": "teerapat.sak@kbtg.tech"}`
	var u struct{Email string `json:"email"`}
	
	if err := json.Unmarshal([]byte(d), &u); err != nil {
		fmt.Println("json unmarshal error:", d, err)
		return
	}
	fmt.Println(u)
}
```
https://play.golang.org/p/tpyoRcQr2BK

### workshops
1. render json as string {"address_no": "46/6", "road": "popular", "district": "pakkret", "province": "nonthaburi", "zip": "11120"}
2. unmarshal json {"name": "john", "hobbies":["watch tv", "surf internet", "read book"]}

## If-Else/switch
```go
package main

import (
	"fmt"
	"runtime"
)

func main() {
	i := 10
	if i < 10 {
		fmt.Println("i more than 10")
	} else {
		fmt.Println("i less than 10")
	}
	
	fmt.Print("Go runs on ")
	switch os := runtime.GOOS; os {
	case "darwin":
		fmt.Println("OS X.")
	case "linux":
		fmt.Println("Linux.")
	default:
		// freebsd, openbsd,
		// plan9, windows...
		fmt.Printf("%s.", os)
	}
}
```
https://play.golang.org/p/tjvaniVzNE-

### workshops
print student grade as condition:
1. A: more than 90
2. B: more than 80
3. C: more than 70
4. D: more than 60
5. F: else

## Pointers
```go
package main

import "fmt"

func double(i int) {
    i = i * 2
}

func main() {
    i := 2
    double(i)
    fmt.Printf("i = %v\n", i)
}
```
https://play.golang.org/p/RPolUu_Mrtw

```go
package main

import "fmt"

func double(i *int) {
    *i = *i * 2
}

func main() {
    i := 2
    double(&i)
    fmt.Printf("i = %v\n", i)
}
```
https://play.golang.org/p/X3TMrjgFD7n

```go
package main

import "fmt"

func main() {
	p := P{A: 10, B: 20}
	p.setA(30)
	p.setB(40)
	
	fmt.Println(p.A, p.B)
}

type P struct {
	A int
	B int
}

func (p P) setA(v int) int {
	p.A = v
	return p.A
}

func (p *P) setB(v int) int {
	p.B = v
	return p.B 
}
```

## Interfaces
```go
package main

import (
	"fmt"
	"math"
)

func main() {
	r := rectangle{width: 3, height: 4}
	c := circle{radius: 5}
	
	measure(r)
	measure(c)
}

type geometry interface {
	area() float64
	perimeter() float64
}

type rectangle struct {
	width, height float64
}

func (r rectangle) area() float64 {
	return r.width * r.height
}

func (r rectangle) perimeter() float64 {
	return 2*r.width + 2*r.height
}

type circle struct {
	radius float64
}

func (c circle) area() float64 {
	return math.Pi * c.radius * c.radius
}

func (c circle) perimeter() float64 {
	return 2 * math.Pi * c.radius
}

func measure(g geometry) {
	fmt.Println(g)
	fmt.Println(g.area())
	fmt.Println(g.perimeter())
}
```
https://play.golang.org/p/nH2PbRTkgjT

### workshops
1. correct the code: https://play.golang.org/p/7_xjfr6S1zc
2. correct the code: https://play.golang.org/p/3KIIad2kpe8

## Routines
```go
package main

import (
	"fmt"
	"time"
)

func main() {
	print("foo")
	print("bar")
}

func print(str string) {
	for i := 0; i < 5; i++ {
		time.Sleep(100 * time.Millisecond)
		fmt.Println(str)
	}
}
```
https://play.golang.org/p/yJgczOdPOP9

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	go print("foo")
	print("bar")
}

func print(str string) {
	for i := 0; i < 5; i++ {
		time.Sleep(100 * time.Millisecond)
		fmt.Println(str)
	}
}
```
https://play.golang.org/p/mBqMFYPHyHR

## Reflect
```go
package main

import (
	"fmt"
	"reflect"
)

func main() {
	str := "string"
	i := 10

	fmt.Println(reflect.TypeOf(str))
	fmt.Println(reflect.TypeOf(i))
}
```
https://play.golang.org/p/I-mmqSHWJpa

## Channel
```go
package main

import "fmt"

func main() {
	messages := make(chan string)

	go func() { messages <- "message from channel" }()

	msg := <-messages
	fmt.Println(msg)
}
```
https://play.golang.org/p/b2kvtJaBC2z

```go
package main

import "fmt"

func main() {
	messages := make(chan string, 2)
	
	messages <- "buffered"
	messages <- "channel"
	//messages <- "foo"
	
	fmt.Println(<-messages)
	fmt.Println(<-messages)
}
```
https://play.golang.org/p/tb7ZlDK_OX5

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	c1 := make(chan string)
	c2 := make(chan string)

	go func() {
		time.Sleep(1 * time.Second)
		c1 <- "one"
	}()
	go func() {
		time.Sleep(2 * time.Second)
		c1 <- "two"
	}()

	for i := 0; i < 2; i++ {
		select {
		case msg1 := <-c1:
			fmt.Println("received:", msg1)
		case msg2 := <-c2:
			fmt.Println("received:", msg2)
		}
	}
}
```
https://play.golang.org/p/Ps41gkSgx-U

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	jobs := make(chan int, 100)
	results := make(chan int, 100)

	for w := 1; w <= 3; w++ {
		go worker(w, jobs, results)
	}

	for j := 1; j <= 5; j++ {
		jobs <- j
	}

	for a := 1; a <= 5; a++ {
		fmt.Println(<-results)
	}
}

func worker(id int, jobs <-chan int, results chan<- int) {
	for j := range jobs {
		fmt.Println("worker", id, "started  job", j)
		time.Sleep(time.Second)
		fmt.Println("worker", id, "finished job", j)
		results <- j * 2
	}
}
```
https://play.golang.org/p/U5GIbzLMzK5

## Concurrency
https://divan.github.io/posts/go_concurrency_visualize/

### workshops
write 2 goroutines to do:
1. one producer routine producing numbers
2. one consumer routine printing numbers to stdout

https://play.golang.org/p/mC-5G4g3uR2

## Unit Testing
```go
package main

func Sum(x, y int) int {
    return x + y
}

func main() {
    Sum(5, 5)
}
```

```go
package main

import "testing"

func TestSum(t *testing.T) {
    total := Sum(5, 5)
    if total != 10 {
       t.Errorf("Sum was incorrect, got: %d, want: %d.", total, 10)
    }
}
```

```go
package main

import "testing"

func TestSum(t *testing.T) {
	tables := []struct {
		x int
		y int
		n int
	}{
		{1, 1, 2},
		{1, 2, 3},
		{2, 2, 4},
		{5, 2, 7},
	}

	for _, table := range tables {
		total := Sum(table.x, table.y)
		if total != table.n {
			t.Errorf("Sum of (%d+%d) was incorrect, got: %d, want: %d.", table.x, table.y, total, table.n)
		}
	}
}
```

go test
go test -cover

```go
package main

import (
	"fmt"
	"io"
)

type FakeReader struct {
}

func (fr FakeReader) Read(p []byte) (n int, err error) {
	// return an integer and error or nil
}

func (fr FakeReader) SetFakeBytes(p []byte) {
	// set fake bytes
}

func ReadAllTheBytes(reader io.Reader) []byte {
	// read from the reader..
}

func main() {
	fakeReader := FakeReader{}
	
	// You could create a method called SetFakeBytes which initialises canned data.
	fakeReader.SetFakeBytes([]byte("when called, return this data"))
	bytes := ReadAllTheBytes(fakeReader)
	fmt.Printf("%d bytes read.\n", len(bytes))
}
```

```go
package main

import (
	"encoding/json"
	"fmt"
	"log"
)

func GetAstronauts(getWebRequest GetWebRequest) int {
	url := "http://api.open-notify.org/astros.json"
	bodyBytes := getWebRequest.FetchBytes(url)
	peopleResult := people{}
	jsonErr := json.Unmarshal(bodyBytes, &peopleResult)
	if jsonErr != nil {
		log.Fatal(jsonErr)
	}
	return peopleResult.Number
}

func main() {
	liveClient := LiveGetWebRequest{}
	number := GetAstronauts(liveClient)

	fmt.Println(number)
}
```

```go
package main

import (
	"io/ioutil"
	"log"
	"net/http"
	"time"
)

type people struct {
	Number int `json:"number"`
}

type GetWebRequest interface {
	FetchBytes(url string) []byte
}

type LiveGetWebRequest struct {
}

func (l LiveGetWebRequest) FetchBytes(url string) []byte {
	spaceClient := http.Client{
		Timeout: time.Second * 2, // Maximum of 2 secs
	}

	req, err := http.NewRequest(http.MethodGet, url, nil)
	if err != nil {
		log.Fatal(err)
	}

	req.Header.Set("User-Agent", "spacecount-tutorial")

	res, getErr := spaceClient.Do(req)
	if getErr != nil {
		log.Fatal(getErr)
	}

	body, readErr := ioutil.ReadAll(res.Body)
	if readErr != nil {
		log.Fatal(readErr)
	}
	return body
}
```

```go
package main

import "testing"

type testWebRequest struct {
}

func (t testWebRequest) FetchBytes(url string) []byte {
	return []byte(`{"number": 2}`)
}

func TestGetAstronauts(t *testing.T) {
	amount := GetAstronauts(testWebRequest{})
	if amount != 1 {
		t.Errorf("People in space, got: %d, want: %d.", amount, 1)
	}
}
```

## workshops
1. write unit testing for calculate grade
