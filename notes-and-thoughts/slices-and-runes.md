# Thoughts On Slices & Runes
Source here: https://blog.golang.org/slices
* Having a hard time wrapping my head around this, so going to document it as I'm going through it.
  * Could be a good candidate for blog/short video to help others hitting the same issue.  

## Arrays
* Not as common in Go because Array Size is part of it's type, which limits it's ability to be expressive.
* Ex `var buffer [256]byte`
* Data associated with array is an array of elements
  * `buffer: byte byte byte ... 256 times ... byte byte byte`
  * ! Attempting to index our array with a value outside the  range (0 - 255) will crash our program
*  len() is built in
* Arrays have their place—they are a good representation of a transformation matrix for instance—but their most common purpose in Go is to hold storage for a slice.

## Slices
* "A slice is a data structure describing a contiguous section of an array stored separately from the slice variable itself. A slice is not an array. A slice describes a piece of an array."
  * Rephrase this after some extra reading.
* 3 ways to write example of slicing the above Array example
  * `var slice []byte = buffer[100:150]`
    * Full var declaration in order to be explicit
  * `var slice = buffer[100:150]`
    * More idiomatic syntax
  * `slice := buffer[100:150]`
    * Short form that can be used inside a function
* Slice is a little data structure with two elements (length & pointer to an array's element)
  * ! A slice of a slice, will still point to the original array
* Can reslice & store under original structure ex `slice = slice[5:10]`
* While a slice containers a pointer,  it is still a value itself.
  * It is a struct value holding a pointer and a length.
  * It is not a pointer to a struct.
