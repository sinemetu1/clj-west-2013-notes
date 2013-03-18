Clojure in the Large
===================
by Stuart Sierra
----------------

# History #

> APL is like a beautiful diamond - flawless, beautifully symmetrical. But you can't add anything to it.
> Lisp is like a ball of mud. Add more and it's still a ball of mud - it still looks like lisp.

* unregulated growth
* expedient repair
* shared promiscuously
* nearly all the important information becomes global or duplicated

    ;; not a lot of structure

    (ns com.example.app)         ;; not first-class modules

    (def state (ref {}))         ;; global by default

    (defn- private-op [arg] ...)

    (defn op1 [arg] ...)         ;; global by default

    (defn op2 [arg] ....)

# Larger ... *

* Programs: 10,000s of lines
* Systems: 10s-100s of machines
* Teams: 10s of developers

* Components serve as the building blocks for the structure of a system. - Pattern Oriented Software Architecture

## Componenent ##

* Encapsulated
* Interface
* Lifecycle

### Encapsulation ###

* Encapsulation deals with grouping the elements of an abstraction ... and with separating different...

#### Global State ####

    ;; Global state
    (def state-a (ref {}))    ;; effectively global mutable variables
                              ;; hidden dependencies

    (def state-b (atom 0))    ;; temptation to share promiscuously

    (defn op1 []
      (dosync
        (alter state-a ...))) ;; cannot ever have more than one
                              ;; hidden dependencies
    
    (defn op2 []
      (swap! state-b ...))

    ;; harder to test
    ;; Reloading code destroys state
    ;; using defonce - cannot recover initial state
    ;; let with defn - hides the global state inside

#### Local State ####

    ;; Local state

    (defn constructor []
      {:a (ref {})               ;; well defined initial state
       :b (atom 0)})             ;; encapsulate related state

    (defn op1 [state]            ;; makes function dependencies clear
      (dosync
        (alter (:a state) ...)))
    
    (defn op2 [state]
      (swap! (:b state) ...))

    ;; testing is easy
    ;; safe to reload

#### Thread bound state ####

    ;; Thread-bound state

    (def ^:dynamic *resource*)

    (defn- internal-op1 []
      ... *resource* ...) ;; hidden dependancy

    (defn op1 []
      ... *resource* ...)

    ;; used in macros
    ;; assumes body starts & ends on a single thread
    ;; assumes body does not return a Lazy sequence
    ;; limits caller to one resource at a time!

    ;; local state with dynamic context
    ;; not limited to one resource
    ;; still assumes body completes on one thread

#### Request scope ####

    (defn op1 [context]                    ;; encapsulates all that fn needs
      (let [resource (::resource context)] ;; acquire resources
        (assoc context ::result1 ...)))    ;; namespace-qualified keys for isolation

    ;; not confined to single thread

    ;; still need to manage resource lifecycle
    (defn finish [context]
      (update-in context [::resource] dispose))

#### Safe global vars ####

    ;; True constants
    (def ^:const gravity 6.67e-11)

    ;; True singletons (rare)
    (def runtime (Runtime/getRuntime))

    ;; For interactive REPL use

#### Private dynamic vars ####

    (def ^:private ^:dynamic *state*)

    ;; binding is confined to a single operation

    ;; operation is single-threaded by definition

    (defn- internal-op1 [arg]
      ... *state* ...)

### Interafaces ###

    (defprotocol KeyValueStore
      (get [store key])
      (put [store key value])
      (set [store key value]))

    ;; Public API
    ;; built on primitive operations

#### Implementation ####

    ;; create state in constructor

    (defrecord SQLStore [url conn]
    
    ;; Another implementation

    ;; create state in constructor
    ;; operations using component
    ;; depend only on public api

    ;; easy to test

    ;; Kingdom of Nouns

### Lifecycle ###

    (defn init! [] ;; rally the troops
      (connect-to-database!)
      (create-thread-pools!)
      (start-background-processes!)
      ...)                           ;; all global side-effects

#### System Object ####
      ;; System object
      (defrecord System
        [storage config web-service])

      ;; encapsulate the whole application

      ;; dev-system
      (defn dev-system []
      ;; different conrstuctors for different stages

      (defn prod-system []
        (let [config (zookeeper-config)
              storage (sql-store config)
              service (web-serv config storage)]
          (->System storage config service)))

#### System Lifecycle ####

    (defrecord System
      [storage config web-service]
      Lifecycle
      (start [_]  ;; uniform lifecycle protocol
        (start storage)
        (start web-service))
      (stop [_] ;; still ordered manually
        (stop web-service)
        (stop storage)))


    ;; global var that holds the application reference
    ;; create the system
    ;; reset fn - stop, reload, restart

    ;; REPL introspection
    (-> system :component1 :state deref) ;; everything is encapsulated but still accessible


# Summary #

* encapsulate components
* encapsulate local state
* decouple by injected dependencies
* manage lifecycle of components
* compose components into systems
