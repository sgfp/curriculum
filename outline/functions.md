Functions
=========

## What are functions?

You have already seen some functions, such as `count`, `conj`, `first`, and `rest`. All the arithmetic we did had functions, as well: `+`, `-`, `*`, and `/`. What does it mean to be a function, though?

A _function_ is an independent, discrete piece of code that takes in some values (called _arguments_) and returns other values. Let's see an example:

```clj
(defn triple
  "Given a number, return 3 times that number."
  [x]
  (+ x x x))
```

In this code:

* `defn` specifies that we are defining a function.
* `triple` is the name of this function.
* The string on the next line is the documentation for the function, which explains what the function does. This is optional.
* `[x]` is the list of arguments. Here, we have one argument called `x`.
* `(+ x x x)` is the _body_ of the function. This is what executes when we use the function.

To use `triple`, we _call_ the function, just like we've done with all the functions we've already used.

```clj
(triple 2)    ;=> 6
(triple 3/2)  ;=> 9/2
(triple 30.3) ;=> 90.9
```

Functions can also take more than one argument. Let's make an `average` function that takes two numbers and gives us the average of those two numbers:

```clj
(defn average
  [x y]
  (/ (+ x y) 2))

(average 2 3) ;=> 5/2
```

### EXERCISE: Make a function to format names

The `str` function can take any number of arguments, and it concatenates them together to make a string. Write a function called `format-name` that takes two arguments, `first-name` and `last-name`. This function should output the name like so: `Last, First`.

## Naming functions

Function names are symbols, just like the symbols we used with `def` when assigning names to values.

Symbols have to begin with a non-numeric character, and they can contain alphanumeric characters, along with *, +, !, -, _, and ?. This flexibility is important with functions, as there are certain idioms we use.

Functions that return true or false--called _predicates_--usually end in `?`:

* `zero?`
* `vector?`
* `empty?`

## Important functions

There are some functions that are essential when using Clojure. The arithmetic functions and `str` have already been covered, and you need to know them. Let's look at some others.

### Comparison functions

You can use the function `=` to test the equality of two things. For example, here is a function called `vegetarian?` that determines whether a person is vegetarian or not:

```clj
(defn vegetarian?
  [person]
  (= :vegetarian (get person :dietary-restrictions)))
```

The other comparison functions are `>`, `>=`, `<`, `<=`, and `not=`, and all but the last of these are used exclusively with numbers. Like all Clojure functions, the comparison functions are used as prefixes, so they can be a little tricky. Here's some examples:

```clj
(> 4 3)    ;=> true
(>= 4 5)   ;=> false
(< -1 1)   ;=> true
(<= -1 -2) ;=> false
```

### String functions

A large part of programming is manipulating strings. The most important string function in Clojure to remember is `str`, which concatenates all of its arguments into one string:

```clj
(str "Chocolate" ", " "strawberry" ", and " "vanilla")
;=> "Chocolate, strawberry, and vanilla"
```

### Collection functions

When we learned about data structures, we saw many functions that operated on those data structures, including:

* `count`
* `conj`
* `first`
* `rest`
* `nth`
* `assoc`
* `dissoc`
* `merge`

Some of the most powerful functions you can use with collections can take other functions as arguments. That's a complicated idea, so we'll learn more about that next.
