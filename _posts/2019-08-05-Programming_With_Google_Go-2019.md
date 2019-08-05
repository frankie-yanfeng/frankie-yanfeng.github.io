---
layout:     post   				    # 使用的布局（不需要改）
title:      Programming With Google Go 				# 标题
subtitle:   Basics #副标题
date:       2019-08-05 				# 时间
author:     frankie 						# 作者
header-img: img/2.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - Go
---

# Course links:
[https://www.coursera.org/specializations/google-golang](https://www.coursera.org/specializations/google-golang)

## Personal repo links:
[https://github.com/frankie-yanfeng/Programming-with-Google-Go](https://github.com/frankie-yanfeng/Programming-with-Google-Go)

## Gist

## Assignment
### Course 1 - Getting Started with Go
#### week 1
hello.go
```
package main

import (
	"fmt"
	"github.com/frankie-yanfeng/stringutil"
)

func main() {
	//fmt.Println("Hello, world.")
	fmt.Println(stringutil.Reverse("!oG ,olleH"))
}

```
#### week 2
trunc.go
```
package main

import (
	"fmt"
)

func main() {
	fmt.Print("Please, enter a float number: ")

	var e float32

	_, err :=fmt.Scanf("%f", &e)

	if err != nil {
		fmt.Println("Error: ", err)
	} else {
		fmt.Printf("%f converted to int is %d\n", e, int(e))
	}
}
```

findian.go
```
package main

import (
	"bufio"
	"fmt"
	"os"
	"strings"
)

func main() {
	fmt.Print("Please, enter a string: ")

	var e string

	scanner := bufio.NewScanner(os.Stdin)
	scanner.Scan() // use `for scanner.Scan()` to keep reading
	e = scanner.Text()

	//if err != nil {
	//fmt.Println("Error: ", err)
	//} else {
	fmt.Printf("Input is %s\n", e)
	a := strings.ToLower(e)
	b := strings.TrimSpace(a)
	if strings.HasPrefix(b, "i") && strings.Contains(b[1:len(b)-1], "a") && strings.HasSuffix(b, "n") {
		fmt.Println("Found!")
	} else {
		fmt.Println("Not Found!")
	}
	//}
}
```

#### Week 3

slice.go
```
package main

import (
	"bufio"
	"fmt"
	"os"
	"sort"
	"strconv"
	"strings"
)

func main() {
	var input string
	var s []int

	s = make([]int, 0, 3)

	fmt.Printf("s len:%d, s cap:%d, s: %v\n", len(s), cap(s), s)

	for {
		fmt.Print("Please, enter an input: ")

		scanner := bufio.NewScanner(os.Stdin)
		scanner.Scan() // use `for scanner.Scan()` to keep reading
		input = scanner.Text()

		fmt.Printf("Input is %s\n", input)

		if strings.ToUpper(input)[0] == 'X' {
			fmt.Printf("X is inputed, over\n")
			fmt.Printf("sorted slice %v\n", s)
			fmt.Printf("s len:%d, s cap:%d, s: %v\n", len(s), cap(s), s)
			return
		}

		//num, _ := strconv.ParseInt(input, 0, 32)
		num, _ := strconv.Atoi(input)

		s = append(s, num)
		sort.Ints(s)
		//fmt.Printf("sorted slice %v\n", s)
	}
}
```

#### week 4
makejson.go
```
package main

import (
	"bufio"
	"encoding/json"
	"fmt"
	"os"
)

func main() {
	var name string
	var address string

	person := make(map[string]string)

	//_, err :=fmt.Scanf("%s", &e)
	fmt.Print("Please, enter a name: ")
	scanner := bufio.NewScanner(os.Stdin)
	scanner.Scan() // use `for scanner.Scan()` to keep reading
	name = scanner.Text()

	//if err != nil {
	//fmt.Println("Error: ", err)
	//} else {
	fmt.Printf("name is %s\n", name)

	person["name"] = name

	fmt.Print("Please, enter an address: ")
	scanner.Scan() // use `for scanner.Scan()` to keep reading
	address = scanner.Text()

	fmt.Printf("address is %s\n", address)

	person["address"] = address

	m, err := json.Marshal(person)
	if err != nil {
		fmt.Println("error:", err)
	}

	os.Stdout.Write(m)

}
```

### Course 2 - Functions, Methods, and Interfaces in Go
#### week 1
bubble_sort.go
```
package main

import (
	"bufio"
	"encoding/json"
	"fmt"
	"os"
)

func main() {
	var name string
	var address string

	person := make(map[string]string)

	//_, err :=fmt.Scanf("%s", &e)
	fmt.Print("Please, enter a name: ")
	scanner := bufio.NewScanner(os.Stdin)
	scanner.Scan() // use `for scanner.Scan()` to keep reading
	name = scanner.Text()

	//if err != nil {
	//fmt.Println("Error: ", err)
	//} else {
	fmt.Printf("name is %s\n", name)

	person["name"] = name

	fmt.Print("Please, enter an address: ")
	scanner.Scan() // use `for scanner.Scan()` to keep reading
	address = scanner.Text()

	fmt.Printf("address is %s\n", address)

	person["address"] = address

	m, err := json.Marshal(person)
	if err != nil {
		fmt.Println("error:", err)
	}

	os.Stdout.Write(m)

}
```
#### week 2
function_returning.go
```
package main

import (
	"bufio"
	"fmt"
	"math"
	"os"
	"strconv"
	"strings"
)

//GenDisplaceFn definition
func GenDisplaceFn(acceleration, initialVelocity, initialDisplacement float64) func(float64) float64 {
	fn := func(time float64) float64 {
		return 0.5*acceleration*math.Pow(time, 2) + initialVelocity*time + initialDisplacement
	}

	return fn
}

func main() {
	//var acceleration, initialVelocity, initialDisplacement, time float64

	for {
		fmt.Println("please input the acceleration, initialVelocity, initialDisplacement, time separated by space")

		scanner := bufio.NewScanner(os.Stdin)
		scanner.Scan()
		input := scanner.Text()
		input = strings.Trim(input, " ")

		fmt.Printf("input is: [%s]\n", input)

		acceleration, _ := strconv.ParseFloat(strings.Split(input, " ")[0], 64)
		initialVelocity, _ := strconv.ParseFloat(strings.Split(input, " ")[1], 64)
		initialDisplacement, _ := strconv.ParseFloat(strings.Split(input, " ")[2], 64)
		time, _ := strconv.ParseFloat(strings.Split(input, " ")[3], 64)

		fmt.Printf("acceleration: %f\n", acceleration)
		fmt.Printf("initialVelocity: %f\n", initialVelocity)
		fmt.Printf("initialDisplacement: %f\n", initialDisplacement)
		fmt.Printf("time: %f\n", time)

		fn := GenDisplaceFn(acceleration, initialVelocity, initialDisplacement)

		//fmt.Println(fn(3))
		fmt.Println(fn(time))
	}
}
```
#### week 3
pointer_recevier_practice.go
```
package main

import (
	"bufio"
	"fmt"
	"os"
	"strings"
)

//Animal definition
type Animal struct {
	food, locomotion, noise string
}

//Eat definition
func (animal Animal) Eat() {
	fmt.Println(animal.food)
}

//Move definition
func (animal Animal) Move() {
	fmt.Println(animal.locomotion)
}

//Speak definition
func (animal Animal) Speak() {
	fmt.Println(animal.noise)
}

func main() {

	var animal Animal

	for {
		fmt.Printf(">please enter the request -> name & infomation: ")

		scanner := bufio.NewScanner(os.Stdin)
		scanner.Scan()
		request := scanner.Text()

		//fmt.Printf("request is: %s\n", request)

		name := strings.Split(request, " ")[0]
		info := strings.Split(request, " ")[1]

		//fmt.Printf("name: %s\n", name)
		//fmt.Printf("info: %s\n", info)

		switch name {
		case "cow":
			animal = Animal{"grass", "walk", "moo"}
		case "bird":
			animal = Animal{"worms", "fly", "peep"}
		case "snake":
			animal = Animal{"mice", "slither", "hsss"}
		default:
			fmt.Printf("%s is not in (cow, bird, snake)", name)
			return
		}

		switch info {
		case "eat":
			animal.Eat()
		case "move":
			animal.Move()
		case "speak":
			animal.Speak()
		default:
			fmt.Printf("%s is not in (cow, bird, snake)", info)
			return
		}
	}
}
```
#### week 4
```
package main

import (
	"bufio"
	"fmt"
	"os"
	"strings"
)

//Animal structure
type Animal interface {
	Eat()
	Move()
	Speak()
}

//Cow type
type Cow struct {
	food, locomotion, noise string
}

//Bird type
type Bird struct {
	food, locomotion, noise string
}

//Snake type
type Snake struct {
	food, locomotion, noise string
}

//Eat - Cow
func (cow Cow) Eat() {
	fmt.Println(cow.food)
}

//Eat - Bird
func (bird Bird) Eat() {
	fmt.Println(bird.food)
}

//Eat - Snake
func (snake Snake) Eat() {
	fmt.Println(snake.food)
}

//Move - Cow
func (cow Cow) Move() {
	fmt.Println(cow.locomotion)
}

//Move - Bird
func (bird Bird) Move() {
	fmt.Println(bird.locomotion)
}

//Move - Snake
func (snake Snake) Move() {
	fmt.Println(snake.locomotion)
}

//Speak - Cow
func (cow Cow) Speak() {
	fmt.Println(cow.noise)
}

//Speak - Bird
func (bird Bird) Speak() {
	fmt.Println(bird.noise)
}

//Speak - Snake
func (snake Snake) Speak() {
	fmt.Println(snake.noise)
}

func main() {

	var nameType map[string]string

	nameType = make(map[string]string)

	for {
		fmt.Printf("\n>please enter the request -> name & infomation: ")

		scanner := bufio.NewScanner(os.Stdin)
		scanner.Scan()
		request := scanner.Text()

		//fmt.Printf("request is: %s\n", request)

		if len(strings.Split(request, " ")) != 3 {
			fmt.Println("Command input number is not equal to 3")
			continue
		}

		command := strings.Split(request, " ")[0]
		fmt.Printf("command: %s\n", command)

		switch command {
		case "newanimal":

			name := strings.Split(request, " ")[1]
			animalType := strings.Split(request, " ")[2]

			fmt.Printf("name: %s\n", name)
			fmt.Printf("animalType: %s\n", animalType)

			//adding name checking
			_, ok := nameType[name]
			if ok == false {
				nameType[name] = animalType
				fmt.Println("Created it!")
			} else {
				fmt.Printf("Animal name %s is taken, please try another name\n", name)
			}
		case "query":

			name := strings.Split(request, " ")[1]
			info := strings.Split(request, " ")[2]

			fmt.Printf("name: %s\n", name)
			fmt.Printf("info: %s\n", info)

			switch nameType[name] {
			case "cow":
				cow := Cow{"grass", "walk", "moo"}
				switch info {
				case "eat":
					cow.Eat()
				case "move":
					cow.Move()
				case "speak":
					cow.Speak()
				default:
					fmt.Printf("%s is not in (eat, move, speak), please try again\n", info)
					continue
				}
			case "bird":
				bird := Bird{"worms", "fly", "peep"}
				switch info {
				case "eat":
					bird.Eat()
				case "move":
					bird.Move()
				case "speak":
					bird.Speak()
				default:
					fmt.Printf("%s is not in (eat, move, speak), please try again\n", info)
					continue
				}
			case "snake":
				snake := Snake{"mice", "slither", "hsss"}
				switch info {
				case "eat":
					snake.Eat()
				case "move":
					snake.Move()
				case "speak":
					snake.Speak()
				default:
					fmt.Printf("%s is not in (eat, move, speak), please try again\n", info)
					continue
				}
			default:
				fmt.Printf("%s is not in (cow, bird, snake), please try again\n", nameType[name])
				continue
			}
		default:
			fmt.Printf("%s is not in (newanimal, query), please try again\n", command)
			continue
		}
	}
}
```

### Course 3 - Concurrency in Go
#### week 2
raceCondition.go
```
package main

import (
	"fmt"
	"time"
)

var count int

func race() {
	fmt.Println(count)
	count++
}

func main() {

	count = 1
	go race()
	go race()
	time.Sleep(1 * time.Second)
}
```

#### week 3
goroutine_practice.go
```
package main

import (
	"bufio"
	"fmt"
	"os"
	"strings"
)

// Animal structure to hold information about an animal
type Animal struct{ food, locomotion, noise string }

// Eat prints the food for an animal instance
func (a Animal) Eat() { fmt.Println(a.food) }

// Move prints the locomotion for an animal instance
func (a Animal) Move() { fmt.Println(a.locomotion) }

// Speak prints the noise for an animal instance
func (a Animal) Speak() { fmt.Println(a.noise) }

func main() {
	fmt.Println("Welcome. This program will get information about a predefined set of animals (cow, bird, snake).")
	fmt.Println("In order to make a request you should input 'animal property'. For example 'bird move'.")

	reader := bufio.NewReader(os.Stdin)

	animals := map[string]Animal{
		"cow":   {"grass", "walk", "moo"},
		"bird":  {"worms", "fly", "peep"},
		"snake": {"mice", "slither", "hss"},
	}

	for {
		fmt.Printf("> ")
		inputStr, err := reader.ReadString('\n')

		if err != nil {
			fmt.Println("Error: %v", err)
		}

		s := strings.Split(inputStr, " ")

		if animal, ok := animals[strings.ToLower(s[0])]; ok {
			property := strings.TrimSpace(strings.ToLower(s[1]))

			switch property {
			case "eat":
				animal.Eat()
			case "move":
				animal.Move()
			case "speak":
				animal.Speak()
			default:
				fmt.Println("Error: Property not found.")
			}
		} else {
			fmt.Println("Error: Animal not found.")
		}
	}
}
```

#### week 4
diningPhilosopherPractice.go
```
package main

import (
	"fmt"
	"sync"
	"time"
)

//ChopS struct
type ChopS struct{ sync.Mutex }

//Philo struct
type Philo struct {
	leftCS, rightCS *ChopS
	number          int
}

var on sync.Once

func setup() {
	fmt.Println("Init philosopher")
}

func host(ch chan int) {
	ch <- 1
	ch <- 2
	<-ch
}

func (p Philo) eat(ch chan int) {

	on.Do(setup)

	for i := 0; i < 3; i++ {

		<-ch

		fmt.Printf("starting to eat %d\n", p.number)

		p.leftCS.Lock()
		p.rightCS.Lock()

		fmt.Printf("finishing eating %d\n", p.number)

		p.rightCS.Unlock()
		p.leftCS.Unlock()

		ch <- i
	}
	return
}

func main() {
	ch := make(chan int, 2)

	//c := make(chan int)

	CSticks := make([]*ChopS, 5)
	for i := 0; i < 5; i++ {
		CSticks[i] = new(ChopS)
	}

	philos := make([]*Philo, 5)
	for i := 0; i < 5; i++ {
		philos[i] = &Philo{CSticks[i], CSticks[(i+1)%5], i + 1}
	}

	go host(ch)

	for i := 0; i < 5; i++ {
		go philos[i].eat(ch)
	}

	time.Sleep(1000 * time.Millisecond)
}
```
