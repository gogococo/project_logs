# Thoughts From Effective Go
Source here: https://golang.org/doc/effective_go.html
* Creating this Doc to reflect on the things I'm learning & make a reference doc to remind myself with.  

##  Suggested Prereq Readings
* https://golang.org/ref/spec
* https://tour.golang.org/
* https://golang.org/doc/code.html

## Useful Tools/Tips/Packages
* I've installed the go-plus package for Atom. Still figuring out all the great things it's doing for me.  
  * On save, it does some nifty things such as linting & important packages I need.

## Formatting
* Tabs Not Spaces (wew Go taking a stance!)
* No line length limit, but if it's long wrap & indent with extra tab
* Go needs less parentheses.
  * Control structures don't syntactically require them
  * Operator precedence hierarchy is shorter/clearer
* MixedCaps not names_with_underscores


## Comments
* `/* multiline comments */`
* `// single line comments`
* Package Comments
  * Block comment preceding package clause, starting with Package name
  * For detailed info/examples: https://golang.org/doc/effective_go.html#commentary

## Names
### Package Names
* Visibility of a name outside a package determined by first character upper case
* After important, package name becomes an accessor for it's contents  
* Package names are:
  * lower-case
  * single word
  * Good ones are: Short, concise & evocative
* Package name need not be unique, can be renamed locally if import packages causes a name collision
* Package name is base name of it's source directory - Come back to  this
  * the package in `src/encoding/base64` is imported as `"encoding/base64"` but has name base64, not `encoding_base64` and not `encodingBase64`.
  * For instance, the buffered reader type in the `bufio` package is called `Reader`, not `BufReader`, because users see it as `bufio.Reader`, which is a clear, concise name.
* Don't use `import .` notation  

### Getters & Setters 
* No automatic support for Getters/Setters
* Can implement own Getters/Setters, but is not  idiomatic or necessary to put Get in the Names
  * If you have a field called `owner` (lower case, unexported), the getter method should be called `Owner` (upper case, exported), not `GetOwner`.
  * A setter function, if needed, will likely be called `SetOwner`.
* More info/Example at https://golang.org/doc/effective_go.html#Getters

### Interface Names
* Reference https://golang.org/doc/effective_go.html#interface-names
