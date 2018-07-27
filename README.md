# Go - Golang quick intro study notes. 📝💨

This is my study notes for Golang. It is a quick intro/guide to start with golang if you have prior programming experience.

- [🏓Go Fundmentals](#--go-fundmentals)
  * [Notes](#notes)
  * [Variables Declaration](#variables-declaration)
  * [Go Primitive Types](#go-primitive-types)
  * [Visibility](#visibility)
  * [Take Input from console](#take-input-from-console)
  * [Iota](#iota)
  * [Go Pointers](#go-pointers)
  * [Go If-Conditions](#go-if-conditions)
  * [Go For-Loops](#go-for-loops)
  * [Go For-Each loop](#go-for-each-loop)
  * [Go Switch Statement](#go-switch-statement)
  * [Type Function and Returning Functions](#type-function-and-returning-functions)
- [✨Go functions and return types.](#-go-functions-and-return-types)
  * [Notes](#notes-1)
  * [Typical Function](#typical-function)
  * [Multiple Returns](#multiple-returns)
  * [Named Returns](#named-returns)
  * [Variadic Functions / Variable arguments list](#variadic-functions---variable-arguments-list)
  * [Type Function and Returning Functions](#type-function-and-returning-functions-1)
  * [Callbacks - Passing functions as argument](#callbacks---passing-functions-as-argument)
  * [Defer Keyword](#defer-keyword)
  * [Receivers](#receivers)
  * [Overriding Receivers](#overriding-receivers)
- [🏗Go Data-Structures](#--go-data-structures)
  * [Arrays](#arrays)
  * [Slices](#slices)
  * [Iterating over a Slice](#iterating-over-a-slice)
  * [Appending to Slice](#appending-to-slice)
  * [Common Slice Functions](#common-slice-functions)
  * [Slices Tricks](#slices-tricks)
  * [Maps](#maps)
- [🏢Go Structs / OOP](#--go-structs---oop)
  * [Notes](#notes-2)
  * [Go supports](#go-supports)
    + [Encapsulation](#encapsulation)
    + [Inheritance and Reusability](#inheritance-and-reusability)
    + [Polymorphism and Interfaces](#polymorphism-and-interfaces)
    + [Overriding](#overriding)
- [🥂Go Concurrency](#--go-concurrency)
  * [Intro](#intro)
  * [Notes](#notes-3)
  * [mutex Locks, WaitGroups, and Atomic operations](#mutex-locks--waitgroups--and-atomic-operations)
  * [Go Channels](#go-channels)
    + [Example 1](#example-1)
    + [Example 2](#example-2)
    + [Example 3](#example-3)
    + [Example 4](#example-4)
    + [Example 5 - Semaphores](#example-5---semaphores)
    + [Example 6 - Using channels as arguments/returns](#example-6---using-channels-as-arguments-returns)
- [🐞Go Error Handling](#--go-error-handling)
  * [Notes](#notes-4)
  * [Example](#example)


--------------------------------------------------------------------------


# 🏓Go Fundmentals

## Notes

1. Program Excuetable is at `package main/func main()`
``` Go

        package main        //Excuetables must be of package main
        import "fmt"
        
        func main() {
        	var g string = "Hello golang"
        	println(g)
        }
        
        func function() string {
        	return "Five of Diamonds"
        }
```

2. **Blank Identifier**: Use `_` to replace unused variables.
3. there are two primitive ways to allocate to a pointer, `new()` and `make()`, they differ though, we will discuss that later, briefly new returns pointer, make return value. read [Doc](https://golang.org/doc/effective_go.html#allocation_new).
4. Every thing is `passed by value` except **arrays, slices, maps and channels** which some calls r**eference types**, these types are passed by reference ( they internally have pointers, so no copying of the actual data happens when passing them) . 
5. Unlike in C, it's perfectly OK to return the address of a local variable; the storage associated with the variable survives after the function returns.

## Variables Declaration

``` Go

      //Declration
      var g string
      
      //Assignment
      g = "golang"
      
      //Declration & Assignment
      var g = "golang"
      var g string = "golang"
      
      //Shorthand - Declration & Assignmnet
      a := 10
      b := "golang"
```

*Uninitialized variables are given its zero value *(e.g int = 0, string = "", bool = false)* 

## Go Primitive Types

The possible values for `bool` are `true` and `false`.

- **uint8** : unsigned 8-bit integers `(0 to 255)`
- **uint16** : unsigned 16-bit integers `(0 to 65535)`
- **uint32** : unsigned 32-bit integers `(0 to 4294967295)`
- **uint64** : unsigned 64-bit integers `(0 to 18446744073709551615)`
- **int8** : signed 8-bit integers (`-128 to 127)`
- **int16** : signed 16-bit integers `(-32768 to 32767)`
- **int32** : signed 32-bit integers `(-2147483648 to 2147483647)`
- **int64** : signed 64-bit integers `(-9223372036854775808 to 9223372036854775807)`
- **int** is either **int64** or **int32** depends on the implementation.
- **float32** : set of `all IEEE-754 32-bit` floating-point numbers
- **float64** : set of `all IEEE-754 64-bit` floating-point numbers
- **complex64** the set of all complex numbers with float32 real and imaginary parts
- **complex128** the set of all complex numbers with float64 real and imaginary parts
- byte alias for `uint8` rune alias for `int32`

## Visibility

If `variables`/`functions` starts with Uppercase character, it is accessible outside the scope of its package, if lowercase then it is only accessible inside its package.

``` Go
package myPkg

var Uppercase = "This is accessible outside the pkg"
var lowercase = "This is not accessible outside the pkg"
func UppercaseFunc() string  {	return "This is accessible outside the pkg"}
func lowercaseFunc() string  {	return "This is accessible outside the pkg"}

// Another file:
package main
import "myPkg"

func main() {
  //Accessible
  println(myPkg.Uppercase)
  println(myPkg.UppercaseFunc())

  //Not Accessible
  println(myPkg.lowercase)
  println(myPkg.lowercaseFunc())
}
```
## Take Input from console
``` Go
//take input like cin >> in c++
var x int = 1337
var y string = "string value"

_, err := fmt.Scan(&x, &y)
fmt.Println("You Entered x:", x, " and y: ", y, " Error: ", err)

//take input like scanf in C
_, err = fmt.Scanf("%d %s", &x, &y)
fmt.Println("You Entered x:", x, " and y: ", y, " Error: ", err)

//take input with white spaces
var z string = "string"

scanner := bufio.NewScanner(os.Stdin)

scanner.Scan()

z = scanner.Text()

fmt.Println("You Entered z:", z)
```
## Iota

iota in Go, is a value used within the **const** block, its value starts at 0 per block, and increment each time it is used again

``` Go
const (
  c0 = iota  // c0 == 0
  c1 = iota  // c1 == 1
  c2 = iota  // c2 == 2
)
```

## Go Pointers

Pointers syntax is essentially like C/C++
``` Go
var value int = 1000
var pointer *int = &value
println(value)                //1000
println(pointer)              //0xfffffffff
println(*pointer)             //1000
(*pointer)++		  			      //1001
*pointer = *pointer + 10	    //1011
println(*pointer)			        //1011
println(*pointer + *pointer)  //1011 + 1011 = 2022
```

## Go If-Conditions

- Braces must open in the same line of the if/else ( Aghh 😕 )
- in Go's if-statements **parentheses( )** around conditions **are optional**.  but the **braces { } are required** even for oneliners.
``` Go
  value := 10
  if value < 10 {
    println("Less Than 10")
  } else if value > 10 {
    println("Greater Than 10")
  } else {
    println("Equals 10")
  }

  //if conditions with statment
  //note that value is inscope of all if/else's 
  if 	value := 10; value < 10 {
    println(value, "Less Than 10")
  } else if value > 10{
    println(value, "Greater Than 10")
  }else{
    println(value, "Equals 10")
  }
```

Go doesn't have Ternary Operator ( x < 0 ? A : B ) 🤷

## Go For-Loops

There are **3 forms** of for loops, also **there is no a while loop syntax in GO** (instead it is a form of for loops), also there is no do-while at all 
``` Go
  //For loop
  for j := 7; j <= 9; j++ { /*stuff*/ }

  //While like for loop
  i := 1
  for i <= 3 { /*stuff*/ i++ }

  //Infinite Loop : While(true)
  for { /*stuff*/ if (/*stuff*/) break }
```
## Go For-Each loop
``` Go
for i, v := range arr {	//do stuff }
for _, v := range arr {	//do stuff }
for i, _ := range arr {	//do stuff }
```
## Go Switch Statement

Switch statements in GO doesn't require `break`; they will break by default, `fallthrough` keyword used to go to NEXT statement even if condition doesn't match, `fallthrough` is like a break so no code can be after it. however a workaround is to use `labels` and `goto`
```Go
i := 2
fmt.Println("Switch for i = ", i, " goes to: ")
switch i {
  case 1:
    fmt.Println("one")
  case 2:
    fmt.Println("two")
    i = 4
    fallthrough //goes to NEXT case even if doesn't match.
  case 3:
    fmt.Println("three")
  case 4:
    fmt.Println("four")
  case 5,6:
    fmt.Println("five or six")
  default:
    fmt.Println("default")
}
```
## Type Function and Returning Functions

1. Functions can be assigned to variables `func0 := func() int {x++; return x}`
2. Functions that are returned from another functions has its own scope per returned function (yea ikr ? 🤷).
``` Go

package main

var x = 0

func main() {
  //local x
  x := 0

  func0 := func() int {x++; return x}
  func1 := incrementGlobalX //without ()
  func2 := wrapper()
  func3 := wrapper()

  println(func0(), " : func0 (local x)")
  println(func1(), " : func1 (global x)")
  println(func2(), " : func2 (per func scope x1)")
  println(func3(), " : func3 (per func scope x2)")
  println("Second Increment")
  println(func0(), " : func0 (local x)")
  println(func1(), " : func1 (global x)")
  println(func2(), " : func2 (per func scope x1)")
  println(func3(), " : func3 (per func scope x2)")
}

func incrementGlobalX() int  {
  x++
  return x
}

func wrapper() func() int {
  x := 0
  return func() int {
    x++
    return x
  }
}
```  
      

# ✨Go functions and return types.

## Notes

- Again Everything is `passed by value` except **arrays, slices, maps and channels** which some calls r**eference types**, these types are passed by reference.
- unlike in C, it's perfectly OK to return the address of a local variable; the storage associated with the variable survives after the function returns.

## Typical Function
``` Go
  // return void
  func add(x int, y int)  {
    fmt.Println("Hello, World!")
  }

  //-------arguments------return------
  func add(x int, y int)  int {
    return x + y
  }

  //-----same type arguments-----------
  func add(x, y int)  int {
    return x + y
  }
```    

## Multiple Returns
```Go
  func swap(x, y string) (string, string) {
    return y, x
  }

  //in main
  a, b := swap("hello", "world")
  fmt.Println(a, b) //prints "world hello"
```
## Named Returns

You can declare return variables and name them at the beginning, they are returned in the end.

*you can override the returns and return whatever you want at the return statement.
```Go
  //Returns x,y at the end.
  func split(sum int) (x, y int) {
    x = sum * 4 / 9
    y = sum - x
    return
    //return a,b  <- u can override default return of x,y.
  }
```
## Variadic Functions / Variable arguments list

The rightmost argument can be a list of variable size (*slice*) of data.
```Go
  // x value here has no use for average. just illustrates the idea of having arguments
  // then a variable number of arguments
  func average(x int, values ...int) float64{
    //print values
    fmt.Println("Single argument value: ", x)
    fmt.Println("Variable argument values: ", values)

    //calculate average
    total := 0
    for _, value := range values {
      total += value
    }

    return float64(total) / float64(len(values))
  }

  func main() {
    avg := average(10,20,30,40,50)
    println("Average:", avg)
  }
```
## Type Function and Returning Functions

1. Functions can be assigned to variables `func0 := func() int {x++; return x}` as anonymous functions or to another declared functions
2. Functions that are returned from another functions has its own scope per returned function ( Closures yea ikr ? 🤷).
``` Go

package main

var x = 0

func main() {
  //local x
  x := 0

  func0 := func() int {x++; return x}
  func1 := incrementGlobalX //without ()
  func2 := wrapper()
  func3 := wrapper()

  println(func0(), " : func0 (local x)")
  println(func1(), " : func1 (global x)")
  println(func2(), " : func2 (per func scope x1)")
  println(func3(), " : func3 (per func scope x2)")
  println("Second Increment")
  println(func0(), " : func0 (local x)")
  println(func1(), " : func1 (global x)")
  println(func2(), " : func2 (per func scope x1)")
  println(func3(), " : func3 (per func scope x2)")
}

func incrementGlobalX() int  {
  x++
  return x
}

func wrapper() func() int {
  x := 0
  return func() int {
    x++
    return x
  }
}
```      
      

## Callbacks - Passing functions as argument
```Go
func visit(numbers []int, callback func(int2 int)){
  for _, n := range numbers {
    callback(n*2)
  }
}
func main() {
  visit([]int{1,2,3,4}, func(n int){
    fmt.Println(n, "- Printed withing the callback function.")
  })
}
```
## Defer Keyword

`Defer` used before a functions executes the function at the end of the scope of it, think of it as a destructor for the scope. usually used to close opened files/buffers so u open the file and closes it using defer in the next line to keep things clean. they're executed as a **stack**.
```Go
fmt.Println("One")
defer fmt.Println("Four")
defer fmt.Println("Three")
fmt.Println("Two")

//Prints One Two Three Four
```
## Receivers

Receiver are the way you create a method for a specific type/struct 
``` Go
type rect struct {
    width, height int
}

func (r *rect) area() int {
    return r.width * r.height
}

//used as 
r := rect{2,3}
areaX := r.area()
fmt.Println(areaX)
```
## Overriding Receivers
```Go
 type Person struct {
    First string
    Last string
    Age int
   }

   type Employee struct {
    Person
    ID string
    Salary int
   }

   func (p Person) FullName() string{
    return p.First + " " + p.Last
   }

   //Override
   func (p Employee) FullName() string{
    return p.ID + " " + p.First + " " + p.Last
   }


   func main() {
    x := Employee{
      Person{
        "Sherif",
        "Abdel-Naby",
        12},
      "0ID12000ID",
      9999,
    }

fmt.Println(x)
fmt.Println(x.Person.FullName()) //Sherif Abdel-Naby
fmt.Println(x.FullName()) 		   //0ID12000ID Sherif Abdel-Naby
```
# 🏗Go Data-Structures

Arrays, Slices, Maps, and Structs

## Arrays

- Arrays are of **static size**, size can't be changed in arrays.
- Arrays elements do not need to be initialized explicitly; the zero value of the type is the initial value.
``` Go
    var x[15] int
    var twoD [2][3] int
```
## Slices

Slices are of dynamic size.
``` GO
    letters := []string{"a", "b", "c", "d"}
    
    /*  using make -> make([]T, len, cap) */
    var s []byte
    s = make([]byte, 5, 5)
    //OR
    s := make([]byte, 5)
    
    // both equiavlent to: s == []byte{0, 0, 0, 0, 0}
```
- A slice does not store any data, it just describes a section of an underlying array. Changing the elements of a slice modifies the corresponding elements of its underlying array. **Other slices that share the same underlying array will see those changes**.
- Slicing a slice changes pointers of the underlying array, so it is as efficient as manipulating array indices, size and capacity of the new slice are changed too, capacity is equal `old capacity - sliced part from the beginning only`
```gO
    	names := [4]string{"John","Paul","George","Ringo",}
    	fmt.Println(names)   //[John Paul George Ringo]
    	a := names[0:2]
    	b := names[1:3]
    	fmt.Println(a, b)    //[John Paul] [Paul George]
    	b[0] = "XXX"
    	fmt.Println(a, b)    //[John XXX] [XXX George] 
    	fmt.Println(names)   //[John XXX George Ringo]
    
    	//ALSO
    
    	//This is an array literal:
    	[3]bool{true, true, false}
    
    	//And this creates the same array as above, then builds a slice that references it:
    	[]bool{true, true, false}
```
## Iterating over a Slice
``` Go
    for i, v := range arr {	//do stuff }
    for _, v := range arr {	//do stuff }
    for i, _ := range arr {	//do stuff }
```

## Appending to Slice

Append return a whole new array (not reference).
``` Go
      var s []int
    	// append works on nil slices.
    	s = append(s, 0)
    	// The slice grows as needed.
    	s = append(s, 1)
```
Append add element at the end of the slice **if there is enough capacity** and r**eturn a reference type**!, if **not enough capacity** it allocate and copy to a new array and **return it as a new value**! and the **old array points to the old data**.

If Append had enough capacity (didn't allocate new array) then ***changing a value in the new returned array changes the value in the old***! but if it allocated a new array to expand capacity, then **changing a value at an index of the newly returned array *DOESN'T* change the old array!**

consider only using append where the left hand side is the same variable in the append first argument **(S = append(S, .....) )** to avoid any unexpected results

``` Go
  //Allocate new capacity  
  var s []int
  s = make([]int, 5, 5)
  x := append(s, 1, 2 ,3)
  x[0] = 1337
  s[0] = 6800
  fmt.Println(s,x) //[6800 0 0 0 0] [1337 0 0 0 0 1 2 3]

  //Doesn't allocat new capacity and return reference
  var s []int
  s = make([]int, 5, 150)
  x := append(s, 1, 2 ,3)
  x[0] = 1337
  s[0] = 6800
  fmt.Println(s,x) //[6800 0 0 0 0] [6800 0 0 0 0 1 2 3]
                   //notice that 1337 is overwritten
```
## Common Slice Functions

**Append Another Slice**
``` Go
  a = append(a, b...)
```
**Copy**

Copy only copy elements of size = min(len(a), len(b)). so, the new slice to which a copy is to be made must have a size == size of the original array to have all elements copied.
```Go
    b = make([]T, len(a))
    copy(b, a)
    // or
    b = append([]T(nil), a...)
```
**Cut**
```Go
    a = append(a[:i], a[j:]...)
```
**Delete**
```Go
    a = append(a[:i], a[i+1:]...)
    // or
    a = a[:i+copy(a[i:], a[i+1:])]
```
## Slices Tricks

[golang/go](https://github.com/golang/go/wiki/SliceTricks)

## Maps

From [https://play.golang.org/p/U67R66Oab8r](https://play.golang.org/p/U67R66Oab8r)
```Go
// To create an empty map, use the builtin `make`:
// `make(map[key-type]val-type)`.
m := make(map[string]int)

// Set key/value pairs using typical `name[key] = val`
// syntax.
m["k1"] = 7
m["k2"] = 13

// Printing a map with e.g. `fmt.Println` will show all of
// its key/value pairs.
fmt.Println("map:", m)

// Get a value for a key with `name[key]`.
v1 := m["k1"]
fmt.Println("v1: ", v1)

// The builtin `len` returns the number of key/value
// pairs when called on a map.
fmt.Println("len:", len(m))

// The builtin `delete` removes key/value pairs from
// a map.
delete(m, "k2")
fmt.Println("map:", m)

// The optional second return value when getting a
// value from a map indicates if the key was present
// in the map. This can be used to disambiguate
// between missing keys and keys with zero values
// like `0` or `""`. Here we didn't need the value
// itself, so we ignored it with the _blank identifier_
// `_`.
_, prs := m["k2"]
fmt.Println("prs:", prs)

//You can use zero value to check if value exist. use second return instead.
fmt.Println("key not found (gets zero value):", m["notFoundKey"])


// You can also declare and initialize a new map in
// the same line with this syntax.
n := map[string]int{"foo": 1, "bar": 2}
fmt.Println("map:", n)
```
# 🏢Go Structs / OOP

## Notes

- in Go you don't create classes, but create types
- in Go you don't do inheritance, you create a value of the type (sort of delegation or embedding a type inside the other and use its methods with some syntactic sugar.
- in Go structs fields can have a tag, it is written between `' '` after field type, tags can be used to e.g exclude or rename a field when Encoding/Decoding it to/from JSON
- Fields and Methods in GO that start with **Uppercase** are exported, hence **they are not seen outside the package and in another packages**, lowercase are unexported fields which are only seen inside their package. e,g Json Encode won't encode unexported fields as Json Encoder package won't be able to access it.

## Go supports

### Encapsulation

- State/Fields
- Methods
- Public/Private → Exported/unexported

### Inheritance and Reusability

``` Go

package main
import "fmt"

type Parent struct {
  First string
  Last string
  Age int
}

type Child struct {
  Parent
  First string
  Middle string
}


func main() {

  x := Child{
    Parent{
      "First",
      "Last",
      12},
    "Child's First",
    "Middle",
  }

  fmt.Println(x)

  fmt.Println(x.First)
  fmt.Println(x.Parent.First)
  fmt.Println(x.Middle)

  fmt.Println(x.Last)
  fmt.Println(x.Parent.Last)

  fmt.Println(x.Age)
  fmt.Println(x.Parent.Age)

}
```
Packages might need your type to implement its interface to work, for example the `sort` package requires you to implement `swap`, `less`, `equal` methods in order to work. also `fmt.Println()` requires you to implement `func (t T) String() string`

### Polymorphism and Interfaces

A type implements an interface by implementing its methods. There is no explicit declaration of intent, no "implements" keyword.

``` Go

// Here's a basic interface for geometric shapes.
type geometry interface {
  area() float64
  perim() float64
  //extraFunc() string	//if we uncomment this, so rect and circle won't
              // be implementing the geometry interface
}

// For our example we'll implement this interface on
// `rect` and `circle` types.
type rect struct {
  width, height float64

}
type circle struct {
  radius float64
}

// To implement an interface in Go, we just need to
// implement all the methods in the interface. Here we
// implement `geometry` on `rect`s.
func (r rect) area() float64 {
  return r.width * r.height
}
func (r rect) perim() float64 {
  return 2*r.width + 2*r.height
}

// The implementation for `circle`s.
func (c circle) area() float64 {
  return math.Pi * c.radius * c.radius
}
func (c circle) perim() float64 {
  return 2 * math.Pi * c.radius
}

// If a variable has an interface type, then we can call
// methods that are in the named interface. Here's a
// generic `measure` function taking advantage of this
// to work on any `geometry`.
func measure(g geometry) {
  fmt.Println(g)
  fmt.Println(g.area())
  fmt.Println(g.perim())
}

func main() {
  r := rect{width: 3, height: 4}
  c := circle{radius: 5}

  // The `circle` and `rect` struct types both
  // implement the `geometry` interface so we can use
  // instances of
  // these structs as arguments to `measure`.
  measure(r)
  measure(c)
}
```
### Overriding

``` Go

  type Person struct {
    First string
    Last string
    Age int
  }

  type Employee struct {
    Person
    ID string
    Salary int
  }

  func (p Person) FullName() string{
    return p.First + " " + p.Last
  }

  //Override
  func (p Employee) FullName() string{
    return p.ID + " " + p.First + " " + p.Last
  }


  func main() {

    x := Employee{
      Person{
        "Sherif",
        "Abdel-Naby",
        12},
      "0ID12000ID",
      9999,
    }

    fmt.Println(x)
    fmt.Println(x.Person.FullName()) //Sherif Abdel-Naby
    fmt.Println(x.FullName()) 		 //0ID12000ID Sherif Abdel-Naby
```
# 🥂Go Concurrency

## Intro

Go Concurrency is made available by what's called `go-routines` , basically when a function is preceded with the `go` keyword, it runs in a `go-routine`, think of go-routine as a thread (***though they're different...**)**.*** go-routines is one of the most important features of Go that makes it and its concurrency model special.

For data synchronization you can use mutex Locks, WaitGroups, and Atomic operations, however..
**It's recommended** to use Go Channels for data synchronization, though using the sync package (using mutex, locks, atomics, and WaitGroups) is also usable if it make sense for your use case.

---

## Notes

1. GO executable exits with active go routines running.

## mutex Locks, WaitGroups, and Atomic operations

[//TODO](//todo) Example on using synchronization by mutex Locks, WaitGroups, and Atomic operations

## Go Channels

- channels in layman terms are like a synchronized bucket that contains data, a go-routine can add data to the channel, or extract data from the channel. There are unbuffered channels, and buffered channels. for unbuffered channels if you're adding data to the channel, adding another data will be blocking until another go-routine extract such data. on the other hand receiving is also blocking until data is put in the channel.
GO Buffered channel add a buffer to the go channel to avoid stalls, however it is not recommended to use it as a beginner, uses it only when it makes sense.
- Channels can be `bidirectional (chan)`, `receive (<-chan)` only, or `send only(chan <-)` , send/receive only channels are useful when channels are passed as arguments, this indicates(and rather enforces) that the passed channel can only be received from (and you can send to), so this introduces some sort of control over how channels are used. think of pkgs where I don't want users to send anything to my channel.

### Example 1

Note that I am using time.Sleep at the end to wait for the code to execute as the program will instantly close after running the two go routines.
```Go
c := make(chan int)

go func() {
  for i := 0; i < 9; i++ {
    time.Sleep(time.Second)
    c <- i
  }
}()

go func() {
  for{
    fmt.Println( <- c )
  }
}()

time.Sleep(time.Second * 15)
```
### Example 2

Using Range on a channel, it will iterate over values added on the channel until the channel is closed.
No need to use time.sleep as the for-range is a blocking function.
```Go
c := make(chan int)

go func() {
  for i := 0; i <= 10; i++ {
    time.Sleep(time.Second)
    c <- i
  }
  close(c)
}()

for n := range c{
  fmt.Println(n)
}
```
### Example 3

Using wait-group to use more than 1 function to write to the same channel.
Using a `waitGroup` to close the channel once the two writing functions signal `wg.Done()`
```Go
c := make(chan int)

var wg sync.WaitGroup

wg.Add(2)

go func() {
  for i := 0; i <= 10; i++ {
    time.Sleep(time.Millisecond * 350)
    c <- i
  }
  wg.Done()
}()

go func() {
  for i := 1000; i <= 1010; i++ {
    time.Sleep(time.Millisecond * 350)
    c <- i
  }
  wg.Done()
}()

go func() {
  wg.Wait()
  close(c)
}()

for n := range c{
  fmt.Println(n)
}
```
### Example 4

Using dynamic number of function calls.

Also notice passing i inside the go func, this is because the value outside is in a for-loop, hence it is changing, so using it inside the the go-routine will lead to unexpected results.
```Go
c := make(chan string)

var wg sync.WaitGroup

n := 10

wg.Add(n)

for i := 0; i < n; i++  {
  go func(i int) {
    for t := i*10; t < i*10 + 10; t++ {
      c <- "From " + 	strconv.Itoa(i) + " : " + strconv.Itoa(t)
    }
    wg.Done()
  }(i)
}

go func() {
  wg.Wait()
  close(c)
}()

for x := range c{
  fmt.Println(x)
}
```
### Example 5 - Semaphores

Using only channels without waitGroup. 
This is done using a channel that store bool (or anything really), and use a function to receive n-done signals then close the main channel.
```Go
c := make(chan string)
done := make(chan bool)

n := 2


for i := 0; i < n; i++  {
  go func(i int) {
    for t := i*10; t < i*10 + 10; t++ {
      c <- "From " + 	strconv.Itoa(i) + " : " + strconv.Itoa(t)
    }
    done <- true
  }(i)
}

go func() {
  //receive the n-dones from the go-routines
  for i := 0; i < n; i++{
    <- done
  }
  close(c)
}()

for x := range c{
  fmt.Println(x)
}
```

### Example 6 - Using channels as arguments/returns

In this example we sum values from 0 to i. e.g( i = 3 → 0+1+2+3 = 6)

---

This code we create a go routine that feeds the **increment channels values** 1,2,3
another Channel called the **sum channel** will take the **increment channel and processes its values ( so the sum channel will run until the increment channel closes )**, then the sum channel will **put its sum value for the main to pick**. the point here that main can do other stuff while sum channel finish processing. also we can pass any type of channel to sum channel to sum not necessary an incrementing values in a decoupled way.
```Go
func main() {

  i := 10

  //Return A Channel that produces 1, 2, 3...n
  c := incrementer(i)

  // Take a channel that produces 1,2,3...n and sum these numbers
  // returns a channel that have the data in it after summation (so it is not blocking the main itself)
  cSum := puller(c)

  /* DO STUFF WHILE Puller is working (that's why it is returning a channel */

  //Pull from the puller when we want the result (This is blocking now)
  //Result for i := 10 should be : 10 + 9 + 8 + 7 + 6 + 5 + 4 + 3 + 2 + 1 + 0 = 55
  fmt.Println("Final Sum", <- cSum)

}

//returns an ACTIVE go routine that produces 1,2,3..n
func incrementer(n int) chan int  {
  out := make(chan int, 10)

  //no need to pass n as parameter as it is a non-changing variable in this context
  go func() {
    for i := 0; i <= n; i++ {
      fmt.Println("From incrementer: Produced i = ", i )
      out <- i
      //just to illustrate it is blocking in main.
      time.Sleep(time.Millisecond * 100)
    }
    close(out)
  }()
  return out
}

//takes a channel that produces numbers that are to be summed together.
func puller(c chan int) chan int  {
  out := make (chan int)
  go func() {
    var sum int
    for n := range c{
      fmt.Println("From Puller go-routine: Sum + i ->", sum, "+", n, "=", sum + n)
      sum += n
    }
    fmt.Println("Summation Finished -> Outputing SUM")
    out <- sum
    // also we can output each sum stage for whoever uses the channel and close when finish.
    //close(out)
  }()
  return out
}
```
```
Output:

    From incrementer: Produced i =  0
    From Puller go-routine: Sum + i -> 0 + 0 = 0
    From incrementer: Produced i =  1
    From Puller go-routine: Sum + i -> 0 + 1 = 1
    From incrementer: Produced i =  2
    From Puller go-routine: Sum + i -> 1 + 2 = 3
    From incrementer: Produced i =  3
    From Puller go-routine: Sum + i -> 3 + 3 = 6
    From incrementer: Produced i =  4
    From Puller go-routine: Sum + i -> 6 + 4 = 10
    From incrementer: Produced i =  5
    From Puller go-routine: Sum + i -> 10 + 5 = 15
    Summation Finished -> Outputing SUM
    Final Sum 15
```
# 🐞Go Error Handling

## Notes
Go doesn't use try/catch and exceptions to handle errors, instead, functions also returns an error along with its own return. programmer should then check this error by an if-condition and deiced what to do accordingly

## Example

from "Go by Example"...
```Go
// In Go it's idiomatic to communicate errors via an
// explicit, separate return value. This contrasts with
// the exceptions used in languages like Java and Ruby and
// the overloaded single result / error value sometimes
// used in C. Go's approach makes it easy to see which
// functions return errors and to handle them using the
// same language constructs employed for any other,
// non-error tasks.

package main

import "errors"
import "fmt"

// By convention, errors are the last return value and
// have type `error`, a built-in interface.
func f1(arg int) (int, error) {
  if arg == 42 {

      // `errors.New` constructs a basic `error` value
      // with the given error message.
      return -1, errors.New("can't work with 42")

  }

  // A `nil` value in the error position indicates that
  // there was no error.
  return arg + 3, nil
}

// It's possible to use custom types as `error`s by
// implementing the `Error()` method on them. Here's a
// variant on the example above that uses a custom type
// to explicitly represent an argument error.
type argError struct {
  arg  int
  prob string
}

func (e *argError) Error() string {
  return fmt.Sprintf("%d - %s", e.arg, e.prob)
}

func f2(arg int) (int, error) {
  if arg == 42 {

      // In this case we use `&argError` syntax to build
      // a new struct, supplying values for the two
      // fields `arg` and `prob`.
      return -1, &argError{arg, "can't work with it"}
  }
  return arg + 3, nil
}

func main() {

  // The two loops below test out each of our
  // error-returning functions. Note that the use of an
  // inline error check on the `if` line is a common
  // idiom in Go code.
  for _, i := range []int{7, 42} {
      if r, e := f1(i); e != nil {
          fmt.Println("f1 failed:", e)
      } else {
          fmt.Println("f1 worked:", r)
      }
  }
  for _, i := range []int{7, 42} {
      if r, e := f2(i); e != nil {
          fmt.Println("f2 failed:", e)
      } else {
          fmt.Println("f2 worked:", r)
      }
  }

  // If you want to programmatically use the data in
  // a custom error, you'll need to get the error as an
  // instance of the custom error type via type
  // assertion.
  _, e := f2(42)
  if ae, ok := e.(*argError); ok {
      fmt.Println(ae.arg)
      fmt.Println(ae.prob)
  }
```
