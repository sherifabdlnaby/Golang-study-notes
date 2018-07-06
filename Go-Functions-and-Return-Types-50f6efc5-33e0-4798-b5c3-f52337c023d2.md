# Go Functions and Return Types

## Notes

- Every thing is `passed by value` except **arrays, slices, maps and channels** which some calls r**eference types**, these types are passed by reference.
- unlike in C, it's perfectly OK to return the address of a local variable; the storage associated with the variable survives after the function returns.

## Typical Function

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
    

## Multiple Returns

    func swap(x, y string) (string, string) {
    	return y, x
    }
    
    //in main
    a, b := swap("hello", "world")
    fmt.Println(a, b) //prints "world hello"

## Named Returns

You can declare return variables and name them at the beginning, they are returned in the end.

*you can override the returns and return whatever you want at the return statement.

    //Returns x,y at the end.
    func split(sum int) (x, y int) {
    	x = sum * 4 / 9
    	y = sum - x
    	return
    	//return a,b  <- u can override default return of x,y.
    }

## Variadic Functions (Variable arguments list)

The rightmost argument can be a list of variable size (*slice*) of data.

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

## Type Function and Returning Functions

1. Functions can be assigned to variables `func0 := func() int {x++; return x}` as anonymous functions or to another declared functions
2. Functions that are returned from another functions has its own scope per returned function (yea ikr ? 🤷).
- Code

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
      
      

## Callbacks - Passing functions as argument

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

## Defer Keyword

`Defer` used before a functions executes the function at the end of the scope of it, think of it as a destructor for the scope. usually used to close opened files/buffers so u open the file and closes it using defer in the next line to keep things clean. they're executed as a **stack**.

    fmt.Println("One")
    defer fmt.Println("Four")
    defer fmt.Println("Three")
    fmt.Println("Two")
    
    //Prints One Two Three Four

## Receivers

Receiver are the way you create a method for a specific type/struct 

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

## Overriding Receivers

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