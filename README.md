# Go Fundamentals

## Main Program

1. Program Excuetable is at `package main/func main()`

    package main
    import "fmt"
    
    func main() {
    	var g string = "Hello golang"
    	println(g)
    }
    
    func function() string {
    	return "Five of Diamonds"
    }

## Variables Declaration

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
