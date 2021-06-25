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

- Although not terms used in Go, we are _**sort of**_ extending `[]string` and using it as a sub-type/sub-class
- We can then go through and replace our usages of `[]string` with `deck`,
  e.g. `cards := deck{"Ace of Diamonds", newCard()}`
- Since we have a new file, we need to tell go to include it when we run, i.e. `go run main.go deck.go`

## 20. Receiver Functions

- When we put the deck's `print()` function together, we gave the receiver `(d deck)`

```
func (d deck) print() {
	for i, card := range d {
		fmt.Println(i, card)
	}
}
```

- Now any variable of type `deck` gets access to the `print` method
- `d` is the actual copy of the deck we're working with, and is available in the function as a variable of this name
- The type `deck` follows this, and this is how we tell Go any variable of this type can access this method
- In another programming language like Python or JavaScript, `d` is like using `this` or `self`, however in GO this is
  avoided by convention
- The receiver is usually named as a one or two letter abbreviation of the type, by convention

## 21. Creating a New Deck

- Instead of creating each card manually, we could make it smarter by having a list of suits, a list of card values, and
  then do a nested loop to add each card
    1. Create two lists, one for suits and one for values
    1. Loop through suits
    1. Nested loop through values
    1. Use `append()` to append and assign to cards

```go
for _, suit := range cardSuits {
for _, value := range cardValues {
cards = append(cards, value+" of "+suit)
}
}
```

## 22. Slice Range Syntax

- For dealing, we can take the initial deck, with a number to deal, and return a new deck (a hand) of this size
- Slices are zero indexed, like other languages
- Individual elements can be accessed just like other languages, e.g. `deck[0]` gets the first element
- To access a range of the slice, we can use the syntax `slice[startIndexIncluding:uptoNotIncluding]`

```go
fruits := []string{"apple", "banana", "grape", "orange"}
// slice[startIndexIncluding:uptoNotIncluding]
// fruits[0:2] -> ["apple", "banana"]
```

- We can also omit an index on either side, to start from the beginning, or continue to the end,
  e.g. `fruits[:2] -> ["apple", "banana"]`, or `fruits[2:] -> ["grape", "orange"]`

## 23. Multiple Return Values

- Since we will want to return the hand and the modified deck, we will need to return two values
- In Go we can do this by adding parenthesis around the return value, and adding multiple return types,
  e.g. `func deal(d deck, handSize int) (deck, deck)`
- We can then capture both returns like this: `hand, remainingDeck := deal(cards, 5)`, which are assigned in the order
  they appear in the return statement

## 24. Byte Slices

- When we want to start using our local storage, we can look at the
  standard [`ioutil`](https://golang.org/pkg/io/ioutil) library
- For saving, we can use `func WriteFile(filename string, data []byte, perm fs.FileMode) error`
    - `filename string` tells us that we need to give a string name for the file
    - `data []byte` then tells us how to provide the data, though we can see now that the type is a slice of type byte,
      commonly called a 'bite slice' in Go
    - The final one indicates what permissions we will use to save, however this is outside our focus for now
- Before saving a file, we will need to convert to a byte slice,
  e.g. `"Hi there!" -> [72 105 32 116 104 101 114 101 33]`
- Essentially, a byte slice is a computer friendly string
- Now we need to determine how to change our deck type to a byte slice

## 25. Deck to String

- To convert our to a byte slice, we can first determine how to convert it to a string to get a step closer
- We will use a process called type conversion, i.e. `[]byte("Hi there!")` where `[]byte` is the type we want,
  and `"Hi there!` is the value we have
- Our flow will be `deck` -> `[]string` -> `string` -> `[]byte`
- Rather than putting this all in one function, we can break out the conversion to the string, in case we need it again

## 26. Joining a Slice of Strings

- Since deck is essentially a string slice, we could use `[]string(d)` to complete the first step, however this is a
  redundant conversion as Go knows to do this for us in the next step
- After that, we can look to the standard packages to find the strings package, which gives us join,
  i.e. `strings.Join(dStringSlice, ",")`
- Note that to import multiple libraries (we need `fmt` and `strings`), we wrap the imports in parentheses

```
import (
"fmt"
"strings"
)
// ...
func (d deck) toString() string {
    return strings.Join(d, ",")
}
```

## 27. Saving Data to the Hard Drive

- Now we have our string, we can look at the string to byte slice needed to write the file
- We can do this simply by using the same type conversion as before, `[]byte(d.toString())`
- We could also take in a filename to allow the caller to set the file name
- When we are saving to a file, there is the chance that an error could occur, thus `ioutil.WriteFile()` returns an
  error, which we can then return from our own function
- Then when we call the function, we should have our string saved to the file at the specified filename

```
func (d deck) saveToFile(filename string) error {
	return ioutil.WriteFile(filename, []byte(d.toString()), 0666)
}
```

## 28. Reading From the Hard Drive

- To read, we can use the opposite `ioutil` function, `func ReadFile(filename string) ([]byte, error)` which returns the
  byte slice representation of the file contents, and any error if one occurred
- For the error:
    - If something goes wrong, this will be populated, otherwise it will have a value of `nil`
    - Usually to handle, we will put an if statement to check if the error is not `nil`, and handle it within the if
      statement body
    - In the handling, it is usually a common sense approach to what should happen if an action can't complete
    - For us, we could first log the error and then either return a new deck, or alternatively quit the program
    - If we decide to exit, we can use the standard [`os`](https://golang.org/pkg/os/) package
- If we get past our if, then the read will have succeeded, and we can continue with the read

```
bs, err := ioutil.ReadFile(filename)
if err != nil {
    fmt.Println("Error:", err)
    os.Exit(1)
}
```

## 29. Error Handling

- To read in the byte slice, we do the opposite conversion as before (25), Our flow will be `[]byte` -> `string`
  -> `[]string` -> `deck`
- We can convert the byte slice to string using `string(bs)`
- Then we can look to the standard [`strings`](https://golang.org/pkg/strings/) package documentation to find a suitable
  function to convert the string to a string slice, namely `func Split(s, sep string) []string` with `","` as our
  separator from before, i.e. `strings.Split(string(bs), ",")`
- We could convert from the string slice to a `deck` type using `deck(...)`, however this is redundant as Go knows to do
  this from our function return type

```
return deck(strings.Split(string(bs), ","))
```

## 30. Shuffling a Deck

- To shuffle, we essentially need to randomise the order of element in the slice, one method is to:
    - For each index, card in cards
    - Generate a random number between 0 and len(cards) - 1
    - Swap the current card and cards[randomNumber]
- So first we need to get our random index to swap values with, we can look at the standard packages, to
  find [`func Intn(n int) int`](https://golang.org/pkg/math/rand/#Intn), which we can pass our max random int into
- We can get the length of a slice using `len()`, and minus 1 to give us our zero indexed random position
- To swap, we can use interesting syntax of `d[i], d[newPosition] = d[newPosition], d[i]` to swap the two values
- However, with just this the order **will not truly be random**, they will always shuffle in the same way, it is
  pseudo-random

## 31. Random Number Generation

- The reason the shuffle will always be the same (in 30), is that `rand.Intn()` is pseudo-random, rather than random and
  because the seed is always the same, the "randomisation" will always be the same
- To truly randomise, we need to change the seed, we can do so by looking at the package docs to find the
  type [Rand](https://golang.org/pkg/math/rand/#Rand) which we can pass a seed into via `func New(src Source) *Rand`
- There are a few hoops to jump through to do this:
    - First we need to look at what the [Source type](https://golang.org/pkg/math/rand/#Source) is
    - Which brings us to [`func NewSource(seed int64) Source`](https://golang.org/pkg/math/rand/#NewSource), a function
      that we can pass in a seed to, and get a Source returned
    - Then we need to find a way of getting an int64 value to pass in, which for this we can simply get
      the [Unix nano time](https://golang.org/pkg/time/#Time.UnixNano) from the standard time package
    - Once we do this, we can set up a variable of type Rand, which we can use `Intn()` on

```
func (d deck) shuffle() {
	source := rand.NewSource(time.Now().UnixNano())
	r := rand.New(source)

	for i := range d {
		newPosition := r.Intn(len(d) - 1)

		d[i], d[newPosition] = d[newPosition], d[i]
	}
}
```

## 32. Testing With Go

- Go testing is not like RSpec, mocha, jasmine, selenium etc
- TO make a test, create a new file ending in `_test.go`, prefixed with the name of the file under test
- To run all tests in a package, run `go test`
- Even test files must contain the package name at the top

## 33. Writing Useful Tests

- To decide what is a useful test, it is important for the developer to decide what they care about to decide that
  something is working as expected, and test for this
- When we identify a function we want to test, we create a function within the test file called `Test[FunctionName]`,
  e.g. `TestNewDeck`, which we then add the tests that we want to
- If functions are closely related, we may test them together, e.g. `TestSaveToDeckAndNewDeckFromFile`
- We must include the argument `t *testing.T` within our test function, e.g. `func TestNewDeck(t *testing.T)`, `t` is
  our 'test handler'
- Then we can use our function, and make some assertions via if statements, and reporting any errors to the test handler
- We can then run our tests using `go test` which will tell us if the tests passed or give us our error that we reported
  to the test handler

```
func TestNewDeck(t *testing.T) {
	d := newDeck()

	if len(d) != 16 {
		t.Errorf("Expected deck length of 16, but got %v", len(d))
	}
}
```

## 34. Asserting Elements in a Slice

- To test fully, we will likely want to do more than simply check the length, for `newDeck()` we could also assert
  elements within the slice are correct
- To do this, we could check the first and last elements

```
func TestNewDeck(t *testing.T) {
	// ...as above

	if d[0] != "Ace of Spades" {
		t.Errorf("Expected card Ace of Spades, but got %v", d[0])
	}

	if d[len(d)-1] != "Four of Clubs" {
		t.Errorf("Expected card Four of Clubs, but got %v", d[len(d)-1])
	}
}
```

- One thing to notice about Go tests, unlike other testing frameworks, the test output doesn't show how many tests have
  passed, which is because we don't register them, so Go doesn't actually know how many tests are within the function

## 35. Testing File IO

- Testing `saveToFile()` and `newDeckFromFile()` are a lot less simple than testing `newDeck()`, due to the use of IO
- Here is an edge case that could cause issues:
    - `newDeck()` -> `saveToFile()` -> file created -> `newDeckFromFile()` -> attempt to load -> crash!
    - Due to the crash, we might not get the chance to clean up after our test
    - We will need to do this cleanup ourselves, manually
- We will need to ensure we do any cleanup after ourselves, as Go will not do it for us, so our flow becomes something
  like:
    1. Delete any files in current working dir with name "_decktesting"
    1. Create a deck
    1. Save to file "_decktesting"
    1. Load from file
    1. Assert deck length etc
    1. Delete any files in current working dir with name "_decktesting"
- We will need to look back at the standard package docs to check how to delete, to
  find [`func Remove(name string) error`](https://golang.org/pkg/os/#Remove)
    - One note is that this returns an error, we should usually handle this but won't for this example

```
func TestSaveToDeckAndNewDeckFromFile(t *testing.T) {
	_ = os.Remove("_decktesting")

	d := newDeck()
	d.saveToFile("_decktesting")

	loadedDeck := newDeckFromFile("_decktesting")

	if len(d) != 16 {
		t.Errorf("Expected 16 cards in deck, but got %v", len(loadedDeck))
	}

	os.Remove("_decktesting")
}
```

- Note we are not handling any errors, and `"_decktesting"` could probably be in a variable instead of duplicated