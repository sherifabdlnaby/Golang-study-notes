# Go Structs / Obj-OOP

- in Go you don't create classes, but create types
- in Go you don't do inheritance, you create a value of the type (sort of delegation or embedding a type inside the other and use its methods with some syntactic sugar.
- in Go structs fields can have a tag, it is written between `' '` after field type, tags can be used to e.g exclude or rename a field when Encoding/Decoding it to/from JSON
- Fields and Methods in GO that start with **Uppercase** are exported, hence **they are not seen outside the package and in another packages**, lowercase are unexported fields which are only seen inside their package. e,g Json Encode won't encode unexported fields as Json Encoder package won't be able to access it.

# Go supports:

## Encapsulation

- State/Fields
- Methods
- Public/Private → Exported/unexported

## Inheritance and Reusability

- Code

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

Packages might need your type to implement its interface to work, for example the `sort` package requires you to implement `swap`, `less`, `equal` methods in order to work. also `fmt.Println()` requires you to implement `func (t T) String() string`

## Polymorphism and Interfaces

A type implements an interface by implementing its methods. There is no explicit declaration of intent, no "implements" keyword.

- Code

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

## Overriding

- Code

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