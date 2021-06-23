# 2: A Simple Start

- Simple Hello World example
- Looking at each line

## 5 questions to answer:
1. How do we run the code in our project?
1. What does `package main` mean?
1. What does `import "fmt"` mean?
1. What's that `func` thing?
1. How is the main.go file organized?

### 1. How do we run the code in our project?
Go CLI:
- `go build` compiles a bunch of source code files to binary
- `go run` compiles and executes on or two files
- `go fmt` formats all the code in each file in the dir
- `go install` compiles and "installs" a package
- `go get` downloads the raw source of someone else's package
- `go test` runs any tests associated with the current project

### 2. What does `package main` mean?
- Packages == Project == Workspace
- A package can have multiple related files inside it
    - Each package must have `package name` defined at the start
- In go, there are 2 types of packages:
    - Executable - generates a file that we can run
    - Reusable - code used as 'helpers', good place to put reusable logic
- The package name `main` defines that this is an executable package, any other package name means that it will be a 
  reusable package
- An executable package must also have a function called 'main' which is the entrypoint

### 3. What does `import "fmt"` mean?
- `import` gives access to code written in another package
- 'fmt' is a shortened version of 'format', and is a standard library in Go
- Other standard library packages are `debug`, `math`, `encoding`, `crypto`, `io`
- `import` is also used for other 3rd party, or external packages
- https://golang.org/pkg/fmt/

### 4. What's that `func` thing?
- Just like functions in other languages
- Structure `func main() { ... }`
    1. `func` keyword
    1. Name of function
    1. Arg list in `()`
    1. Function body in `{ ... }`, executed on function call

### 5. How is the main.go file organized?
- This will always be the exact same pattern
    1. package declaration at the top `package main`
    1. imports of other packages `import "fmt"`
    1. declaration of functions & telling Go what to do `func main() {...`

## Appendix

### How to Access Course Diagrams
All of the diagrams in this course can be downloaded and marked up by you!  Here's how:
1. Go to https://github.com/StephenGrider/GoCasts/tree/master/diagrams
1. Open the folder containing the set of diagrams you want to edit
1. Click on the ‘.xml’ file
1. Click the ‘raw’ button
1. Copy the URL
1. Go to https://www.draw.io/
1. On the ‘Save Diagrams To…’ window click ‘Decide later’ at the bottom
1. Click ‘File’ -> ‘Import From’ -> ‘URL’
1. Paste the link to the XML file
1. Tada!