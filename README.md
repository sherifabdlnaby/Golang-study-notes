# Go Fundamentals

## Main Program

1. Program Excuetable is at `package main/func main()`
  - Code

        package main
        import "fmt"
        
        func main() {
        	var g string = "Hello golang"
        	println(g)
        }
        
        func function() string {
        	return "Five of Diamonds"
        }

2. Everything in GO is **passed by value**. there is no pass by reference (pass pointers for that)
3. **Blank Identifier**: Use `_` to replace unused variables.

## Variables Declaration

- Code

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

*Uninitialized variables are given its zero value *(e.g int = 0, string = "", bool = false)* 

## Visibility

If `variables`/`functions` starts with Uppercase character, it is accessible outside the scope of its package, if lowercase then it is only accessible inside its package.

- Code

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

## Type Function and Returning Functions

1. Functions can be assigned to variables `func0 := func() int {x++; return x}`
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
      
      

## Take Input from console

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

## Iota

iota in Go, is a value used within the **const** block, its value starts at 0 per block, and increment each time it is used again

- Code

      const (
      	c0 = iota  // c0 == 0
      	c1 = iota  // c1 == 1
      	c2 = iota  // c2 == 2
      )

## Go Pointers

Pointers syntax is essentially like C/C++

    var value int = 1000
    var pointer *int = &value
    println(value)                //1000
    println(pointer)              //0xfffffffff
    println(*pointer)             //1000
    *pointer++		  			        //1001
    *pointer = *pointer + 10	    //1011
    println(*pointer)			        //1011
    println(*pointer + *pointer)  //1011 + 1011 = 2022

## Go If-Conditions

- Braces must open in the same line of the if/else ( Aghh 😕 )
- in Go's if-statements **parentheses( )** around conditions **are optional**.  but the **braces { } are required** even for oneliners.

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
    

Go doesn't have Ternary Operator ( x < 0 ? A : B ) 🤷

## Go For-Loops

There are **3 forms** of for loops, also **there is no a while loop syntax in GO** (instead it is a form of for loops), also there is no do-while at all 

    //For loop
    for j := 7; j <= 9; j++ { /*stuff*/ }
    
    //While like for loop
    i := 1
    for i <= 3 { /*stuff*/ i++ }
    
    //Infinite Loop : While(true)
    for { /*stuff*/ if (/*stuff*/) break }
