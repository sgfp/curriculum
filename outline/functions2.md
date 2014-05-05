## Functions that take other functions

One of the most magical things about Clojure--and many other programming languages--is that it can have functions that take other functions as arguments. That may not make sense at first, so let's look at an example:

```clj
(defn triple
  [x]
  (+ x x x))

(map triple [1 2 3]) ;=> [3 6 9]
```

`map` is a function that takes another function, along with a collection. It calls the function provided to it on each member of the collection, then returns a new collection with the results of those function calls. This is a weird concept, but it is at the core of Clojure and functional programming in general.

Let's look at another function that takes a function. This one is `reduce`, and it is used to turn collections into a single value:

```clj
(defn add
  [x y]
  (+ x y))

(reduce add [1 2 3]) ;=> 6
```

`reduce` takes the first two members of the provided collection and calls the provided function with those members. Next, it calls the provided function again--this time, using the result of the previous function call, along with the next member of the collection. `reduce` does this over and over again until it finally reaches the end of the collection.

This process is complicated, so let's illustrate it further.

```clj
(defn join-with-space
  [string1 string2]
  (str string1 " " string2))

(reduce join-with-space ["i" "like" "peanut" "butter" "and" "jelly"])
;=> "i like peanut butter and jelly"
```

In the example above, `reduce` calls `join-with-space` with the parameters `"i"` and `"like"`, returning `"i like"`. Then, in order, it makes the following function calls:

* `(join-with-space "i like" "peanut")`
* `(join-with-space "i like peanut" "butter")`
* `(join-with-space "i like peanut butter" "and")`
* `(join-with-space "i like peanut butter and" "jelly")`

Another example of a function that uses a function is `sort-by`. It takes a function and sorts a sequence by applying that function to each element of the sequence.

```clj
(sort-by val > {:amy 3, :renee 5, :lisa 4})
;=> ([:renee 5] [:lisa 4] [:amy 3])
```
### Anonymous functions

So far, all the functions we've seen have had names, like `+` and `str` and `reduce`. However, functions don't need to have names, just like values don't need to have names. We call functions without names _anonymous functions_.

Before we go forward, you should understand that you can _always_ feel free to name your functions. There is nothing wrong at all with doing that. However, you _will_ see Clojure code with anonymous functions, so you should be able to understand it.

An anonymous function is created with `fn`, like so:

```clj
(fn [string1 string2] (str string1 " " string2))
```

You might notice that this function is the same as the function we called `join-with-space`. `fn` works a lot like `defn`; we still have arguments listed as a vector and a function body. I didn't break the line in the anonymous function above, but you can, just like you can in a named function.

Why would you ever do this? Anonymous functions can be very useful when we have functions that take other functions. Let's take each of our examples above, but use anonymous functions instead of named functions.

```clj
(map (fn [x] (+ x x x)) [1 2 3]) ;=> [3 6 9]
(reduce (fn [x y] (+ x y)) [1 2 3]) ;=> 6
(reduce
  (fn [s1 s2] (str s1 " " s2))
  ["i" "like" "peanut" "butter" "and" "jelly"])
;=> "i like peanut butter and jelly"
```

### EXERCISE: Find the average

Create a function called `average` that takes a vector of numbers and returns the average of those numbers.

Hint: You will need to use `reduce` and `count`.

### EXERCISE: Get the names of people

Create a function called `get-names` that takes a vector of maps of people and returns a vector of their names.

Here is an example of how it should work:

```clj
(get-names [{:first "Margaret" :last "Atwood"}
            {:first "Doris" :last "Lessing"}
            {:first "Ursula" :last "Le Guin"}
            {:first "Alice" :last "Munro"}])

;=> ["Margaret Atwood" "Doris Lessing" "Ursula Le Guin" "Alice Munro"]
```

## `let`

When you are creating functions, you may want to assign names to values in order to reuse those values or make your code more readable. Inside of a function, however, you should _not_ use `def`, like you would outside of a function. Instead, you should use a special form called `let`. Let's look at an example:

```clj
(defn spread
  "Given a collection of numbers, return the difference between the largest and smallest number."
  [numbers]
  (let [largest (reduce max numbers)
        smallest (reduce min numbers)]
    (- largest smallest)))

(spread [10 7 3 -3 8]) ;=> 13
```

This is the most complicated function we've seen so far, so let's go through each step. First, we have the name of the function, the documentation string, and the arguments, just as in other functions.

Next, we see `let`. `let` takes a vector of alternating names and values. `largest` is the first name, and we assign the result of `(reduce max numbers)` to it. We also assign the result of `(reduce min numbers)` to `smallest`.

After the vector of names and values, there is the body of the `let`. Just like a the body of a function, this executes and returns a value. Within the `let`, `largest` and `smallest` are defined.

Type the `spread` function into your instarepl and see how it evaluates.

### EXERCISE: Rewrite average

Go back to the `average` function you created before and use `let` to make it easier to read.
