# 3: Deeper Into Go

- Simulates playing with playing card

## 13. Functions

- `newDeck` creates a list of cards, essentially an array of strings
- `print` logs the contents of a deck of cards
- `shuffle` shuffles all the cards in a deck
- `deal` creates a 'hand' of cards
- `saveToFile` saves a list of cards to a file on the local machine
- `newDeckFromFile` loads a list of cards from the local machine

## 15. Variable Declarations

- `var card string = "Ace of Spades"` declares and assigns a value to the variable called card
    - `var` says we're about to create a new variable
    - the name of the variable, `card`, follows this
    - we then tell the Go compiler that only a `string` will ever be assigned to this variable
    - after the equals, we create a string that is then assigned to this variable
- Some basic data types available include `bool`, `string`, `int` and `float63`, and many more
- We don't really need to put `string` in, as we are telling Go what the type on the right-hand side of the assignment
- Instead, we can use `card := "Ace of Spades"`
    - `:=` tells the Go compiler we are creating and assigning a variable, the colon is what indicates a declaration
    - The compiler will then work out the type from the right-hand side

## 16. Functions and Return Types

- if we want to create a card within another function, say because we don't know it's value right away, we can with
  another `func`

```
func newCard() string {
    return "Five of Diamonds"
}
```

- This defines a function with the name `newCard`, which can be called like `newCard()`
- In the function signature, `string` tells us that the function will return a value of type string
- Since we tell Go we will return a string, `card := newCard()` is still inferred to be a string variable

## 17. Slices and For Loops

### Slices

- So far, we have done a single card at a time
- For a deck, we will need a data structure to handle a list of cards
- Go has two basic data structures for handling lists of records
    - Array - Fixed length list of things
    - Slice - An array that can grow or shrink
- Both arrays and slices must be declared with a type, which every element must be of
- To define the slice, we use `[]` followed by the type, e.g. `[]string`
- To initialise, we can add a list of elements within curly braces, e.g. `[]string{"Ace of Diamonds", newCard()}`
- To add to a slice, we can use `cards = append(cards, "Six of Spades")` which takes in the slice and a new element to
  append, before assigning this to cards
    - Note the assignment is necessary as append() does not modify the first parameter

### For Loops

- We will want to iterate over a slice
- We can use for loops for this

```
for i, card := range cards {
    fmt.Println(i, card)
}
```

- where:
    - `range cards` is the most important, it takes the slice and iterates over it, it returns `index` and `card`
    - `index` is the index of the element in the array
    - `card` is the current card being iterates over
    - the body runs each time for each card in the slice
    - `:=` is used as `index` and `card` only live during the lifecycle of each iteration

## 18. OO Approach vs Go Approach

- Go is not an object orientated language, so there is nothing like classes
- In OO, we would likely create a Deck class, which is then instantiated and functions called on the instance
- We have looked at basic data types in go (`string`, `integer`, `float`, `array`, `map`)
- If we want to 'extend' a base type and add some extra functionality:
    - `type deck []string` tells go we want to create an array of strings and add a bunch of functions to work with it
    - we create functions with 'deck' as a 'receiver', where a receiver is like a method, or a function that belongs to
      an 'instance'
- No we are looking at types, we could create another file called `deck.go`, outside of `main.go`, to describe what a 
  deck is and how it works, later we could add `deck_test.go` when we figure out how we can test `deck`
  
## 19. Custom Type Declarations
- 