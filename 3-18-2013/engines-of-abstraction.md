Engines of Abstraction
======================
by Jim Duey
-----------

* Worked in embedded programming, capturing license plates on cars.
* EU outlawed electronics using lead-base solder, card discontinued
* Needed to use different card
* Didn't use an abstraction of a video card

* Did implementation of miniKanren
* could redo the search in a parallel way instead of linear
* the magic part is that miniKanren is build on top of an abstraction

* in OO the abstraction is the object
* in FP we have many more abstractions that we can take care of
* ie. conj is an abstraction, abstracted the idea of adding an element to a collection

# Clojure (lisp on the JVM) #

## Monoid ##

* A datatype, a function to combine values and a 'zero' value.
* list, concat, '()
* set, untion, #{}
* hash-map, merge, {}
* vector, (comp vec concat), []
* number, +, 0 or *, 1
* string, str, ""
* fn, comp, identity

## Functor ##

* A 'context' datatype and a function (fmap) to apply a function to each value inside that context.
* ie. Clojure's `map`
* A more general type, could be a function

### Applicative Functors ###

* A 'context' datatype and two functions:
  one that wraps a value in the context type
  'amap' applies fn(s) wrapped in the context
    to parameters wrapped in the context.

    (juxt :a :b :c)
     ; is equivalent to
    (amap (areader list) :a :b :c)
     ; where 'areader' is the wrapper function
     
     ; in the case :a :b :c pull values out of the hash-map (environment)
     : list aggregates the values

* A way of composing side-effecting functions

#### Monads ####

* A context datatype, a fn to wrap values, a fn (bind) to apply a fn to wrapped values.
  The applied fn takes a value and returns a wrapped value.
* List comprehentions. 'for'
** an example of monad comprehension
* Any query language. LINQ
** querying against a context and getting a set of results back, set comprehentions, set monad

##### Example #####

    SELECT name, color FROM
     JOIN people, colors
     WHERE people.gropu = colors.group

    (for [person people
          color  colors
          :when  (= (:group person)
                    (:group color))
      {:name  (:name person)
       :color (:color colors)})

* CALM Theoreom
* Bloom
** BloomL
*** Semi-Lattice - A set of values, a 'join' function and a special value that is less than all other values.
** Lattice looks a lot like a monoid.

### Comonad ###

* A context datatype which wraps values with one value having the 'focus'
  a fn (extract) to unwrap the focused value, a fn (extend) to apply a fn
  to wrapped values. The applied fn takes a wrapped value and returns a value.

* Huet's Zippers - let you work with trees in a function way
* Reactive Extensions - Netflix's reactive extensions with Java
* Cellular automata - Conway's game of life, have values in the cells, have rules to determine next generation,
  rule gets applied to current generation

### Arrows ###

* All monads are arrows, all comonads are arrows
* A context datatype which wraps a function.
** A fn (arr) to wrap any fn of one arg in the context
** A fn (seq) to compose two wrapped fns
** A fn (nth) to apply a wrapped fn to the nth element of a list/vector

* Streams
** Storm, Cascalog - stream processing
* Parsing
** recursive descent parsing, not very space efficient, implement a LL1 grammar to efficiently
   parse the text, able to discard if not needed again because of look ahead

# Questions #

* How to reactions and FRP fit into the comonadic ide?

> Say you create a future, and that future imbeds a value in it. So you imbed a computation inside of it.
  Extend the future value with a timeout checker, and you can extend that with some other functionality.
  You can pull that value out of that future and do something with it and have a future do something with it.
  Compose computations inside futures and never dereference them.

* Have you used the word 'lift' in any of these things?

> You 'lift' funtions into your abstrations. Take a fn and do some stuff to it so that it can be used
  on values inside of your abstraction

* What does the syntax for the comonad or an arrow look like in Clojure?

> 'for' and a binding vector, a symbol and an expression. That's the do notation for monads.
  In a comonad all the arrows are reversed, you'd have a codo and a binding vector and instead of having
  a symbol and an expression you'd have an expression and a symbol. The symbol would be fed into the expression,
  and fed into the next expression.
  With arrows, you have branching things. Arrows are a subset. You'd have an arrow-do, and instead of a binding vector
  you'd have a triplet, symbol expression symbol.

* History of monads?

> Mathematicians invented the vocabulary so that they could plug in the domain, ie abstraction.
  All were proving the same thing just in different domains.

* Risk
  1. a tool to help think about problem solving
  2. to avoid concrete code getting turned into a concrete abstraction

> Look at things as being a tradeoff. Don't apply these things blindly. Pick the right tool for the job.

* An datomic queries seems like a comonad. Is it?

> Not sure.
