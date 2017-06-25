# Channels and synchronization
## Doesn't work, unbuffered channel will block on write if no one is availble to read the value (a required handshake).
```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var wg sync.WaitGroup
	xc := make(chan int)
	xc <- 0
	wg.Add(2)
	go func(){
		val := <-xc
		val++
		xc <- val
		wg.Done()
	}()
	go func(){
		val := <-xc
		val++
		xc <- val // last function trying to write will block
		wg.Done()
	}()
	wg.Wait()
	fmt.Println(<-xc)
	
}
```

# How to solve this?

### Use a buffered channel of size 1 allowing to write a value and continue until someone reads
```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var wg sync.WaitGroup
	xc := make(chan int, 1)
	xc <- 0
	wg.Add(2)
	go func(){
		val := <-xc
		val++
		xc <- val
		wg.Done()
	}()
	go func(){
		val := <-xc
		val++
		xc <- val
		wg.Done()
	}()
	wg.Wait()
	fmt.Println(<-xc)
	
}
```

## Solution using only channel no waitgroup to synchronize
```go
package main

import "fmt"

func main() {
	in := initialize()
	done := make(chan bool)
	go incrementor("1", in, done)
	go incrementor("2", in, done)
	wait(2, done)
	close(in)
	fmt.Println("Final Counter:", <-in)
}

func initialize() chan int64 {
	in := make(chan int64, 1)
	go func() {
		in <- 0
	}()
	return in
}

func incrementor(s string, in chan int64, done chan bool) {
	for i := 0; i < 20; i++ {
		val := <-in
		val++
		in <- val
	}
	done <- true
}

func wait(n int, done chan bool) {
	for i := 0; i < n; i++ {
		<-done
	}
}
```
### the last time a go func writes to xc it will block since no read can be executed because waitGroup is waiting for the go func to mark as done and will wait forever cause no one will read the value ever before wg.Done() is called and we never get to that point.
```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var wg sync.WaitGroup
	xc := make(chan int)
	xc <- 0
	wg.Add(2)
	go func(){
  xc <- 1 //WRITE NO READ (deadlock)
		wg.Done()
	}()
	go func(){
		xc <- 2 //WRITE NO READ (deadlock)
		wg.Done()
	}()
	wg.Wait()
	fmt.Println(<-xc)
		
}
```