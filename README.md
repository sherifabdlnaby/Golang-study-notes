package main

import "awesomeProject1/myPkg"

func main() {
	card1, card2 := newCard()

	//Accessible
	println(myPkg.Uppercase)
	println(myPkg.UppercaseFunc())

	//Not Accessible
	println(myPkg.lowercase)
	println(myPkg.lowercaseFunc())


	cards1 := deck{card1, card2}

	println(cards1.print())
}

func newCard() (string, string2 string) {
	return "Five of Diamonds", "Ace of Spades"
}



