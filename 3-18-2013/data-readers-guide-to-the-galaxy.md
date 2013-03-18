Data Reader's Guide to the Galaxy
============
by Steve Miner
--------

Took the same material and reused it in different ways.
Corresponds to how we're using data readers with tagged literals.

# The Guide #

* standard repo for all knowledge and wisdom
* basics
* new in Clojure 1.5
* EDN
* unorthodox ideas

## Tagged Literals ##

    #my.ns/tag literal-data
    #inst "2013-03-18       ;; instant literal

* self-describing data, can choose the implementation of the tagged literal
* loosely coupled to implementation
* data transfer

## Extensible Reader ##

* add new data literals
* cusomized to application
* limited form of CL reader macros
* read-time vs. compile time

## *data-readers* ##

* data_readers.clj
** literal map
** tag symbols to var symbols

    (binding [*data-readers* {...}] body)

### example ###

    (defn my-reader [[a b]]
      (+ (* 10 a) b))

    (binding [*data-readers*
               {'my.ns/tag #'my-reader}]
      (read-string "#my.ns/tag [4 2]"))

    ;=> 42

### default-data-readers ###

* pre-defined by Clojure
* #uuid
* #inst

> Time is an illusion. Lunchtime doubly so.

### #inst ###

    #inst "2013-03-18"
    #inst "2013-03-18T09:50-07:00"
    #inst "2013-03-18T16:50:00.000-00:00"

#### Date/Time ####

* java.util.Date
* java.util.Calendar
* java.sql.Timestamp
* Joda Time - clj-time
* JSR 310 - java.time in JDK 8

### #uuid ###

### Examples ###

    #x/url "http://clojurewest.org"
    #x/coords [45.323 38.2343]

### *default-data-reader-fn* ###

* new in Clojure 1.5
* defaults to nil, throws on unknown tag
* fn receives a tag (symbol) and a value
* should return a literal value

#### Example *ddrf* ####

    (defrecord TaggedValue [tag value])

* print-method TagedValue
** #tag value

### Records vs. Tags

* record: #my.ns.Rec{:a 42}
* tagged: #my.ns/Rec {:a 42}
* *default-data-reader-fn*
** factory for tag: my.ns/map->Rec
** maybe check tag ns or Capital

#### Example ####

    (defn record-tag-factory [tag]
      (resolve (symbol (str (namespace tag) "/map->" (name tag)))))

    (defn default-reader [tag value]
      (if-let [factory (and (map? value)
                         (Character/isUpperCase

### Library Authors ###

* #my.ns/tag semantics
* Base literal value
* Implementation types
* Data-reader functions
* Print methods (maybe)
* data_readers.clj (maybe)

### Printing ###

* see instant.clj
* Clojure prints using #inst for ju.Date,
  ju.Calendar and js.Timestamp
* *print-dup* ignored

### Gotchas ###

* CLJ-1138 returning nil throws.
** Returns '(quote nil) instead.
* CLJ-1100 period in tag throws.
* Java Dates are mutable.

### Example ###

    (= #inst "2013-03-18T16:50Z"
       #inst "2013-03-18T09:50-07:00")
    ...

## *read-eval* Kerfuffel ##

* Ruby on Rails vulnerability
* Clojure *read-eval* true by default
* #=(dangerously) can execute code
* not safe for threading untrusted data
* CLJ-1153 and CLJ-904

> Don't panic.

### clojure.core/read ###

* Designed by hyper intelligent, pan dimensional beings
* *read* is for trusted input
* binding *read-eval* false
** (pre 1.5) allowed Java constructors

### clojure.edn ###

* new EdnReader in Clojure 1.5
* no #=()
* no Java constructors, no records
* just safe EDN elements

#### clojure.edn/read ####

* arities = [] [stream] [opts stream]
* opts map - :eof, :readers, :default
* defaults to *in* and default-data-readers

#### clojure.tools.reader ####

* written in Clojure
* A complete Clojure reader
* an edn-only reader
* works with Clojure 1.3+

## EDN ##

* extensible data notation
* edn-format.org
* Clojure style values: {} [] () sym :kw
* #tagged literals
* implementations for other languages

## XML ##

* XML 1.0 (1998) - working gruop of 11 experts and an interest group of 150 within W3C
* <xml/> is (s-expr) with better marketing

## JSON ##

* "The Fat-Free Alternative to XML"
* "The good thing about reinventing the wheel is that you can get around one." - Crockford
* not extensible

## EDN vs. JSON ##

* extensible
* more types
* a litte more syntax
* conveyance of values, not objects
* slightly cheaper

## Spyscope ##

* more useful than prinln debugging

# Summary #

* #tagged literals are mostly harmless
* EDN

# Questions #

* Can you enclose tags?
> Yes. It might get pretty complicated, other tags have to know something about those other classes.
