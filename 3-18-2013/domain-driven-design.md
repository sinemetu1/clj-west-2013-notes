Domain Driven Design
====================
by Amit Rathore
---------------

## How to organize larger applications ##

* modular
* maintainable
* agile

* business domain is the focus

1. Domain
2. Driven
3. Design

### Domain ###

* layers ie. Presentation <= Transport <= Domain <= Persistence <= Data storage
* other stuff ie. APIs, various clients, batch-processing, real-time processing, "offline" mode

* domain = business logic area, subject area, differntiation
* leverage expertise

#### What does it look like? ####

##### OO world = nouns #####

  processes are owned by nouns
  designing the domain model - the right nouns own the right processes
  nounes = classes
  processes = methods

> dude, not everything is a noun

##### Clojure #####

* verbs = functions
  nouns = data
  verbs transform nouns, functions transform data
  **set of functions that capture expertise
  purity matters, no side-effects
  no dependency on data storage! (not part of the domain)
  validation outside of domain layer
  domain model deals with canonical representation
  domain specific, semantic checks

#### Where do these live? ####

* zolo.domain.*
* look a lot like class names

* zolo.gateway.*
* reading from third-party apis

* others
* zolo.api.*, zolo.services.*

* service - GRASP
* fns for RESTful access
* service delegates

#### What lives here? ####

* score
* merge
* suggest

* a few public functions
* many private helper functions

#### inlinde or a function? ####

* functions names are documentation
* > I've seen that before

### Driven ###

* start with domain functions
* thin slices around domain
* puts the business in control ie. customer focused

* testing pure functions, unit-testing, TDD

#### design ####

* single responsibility
* a function should do only one thing
* unix-like philosophy to functions, ie. piping
* 8-9 lines in Java, 4-5 in Ruby, 3-4 in Clojure
* optimize for readability
* makes these optimizations *now*

* domain model needs to be collectively owned

### Design ###

* desired goal = higher flexibility
* ubuiquitous languages, common vocabulary, single document that defines terms
* basis for a DSL runtime
* script the domain model
* solve class of problems
* iterative approach, domain model evolves

## Final Thoughts ##

* manage complexity from the domain
* remove incidental complexity, long functions, badly named, etc.
* as simple as possible, no simpler

> YAGNI, you aren't going to need it

* saying "no" is valuable

* DDD = agility

# Questions #

* If a UI element changes, do you want to refactor down?
> Absolutely. You may add new people, or when you go back later, you won't remember
  that it changed.

* How well does placing validation at the boundary condition play out?
> That was the truth at one time. The persistence layer is outside the boundary.
  You shouldn't persist bad data.

* Is there anything that you would question now after working with FP?
> Transfers over really well. People coming from OO actually have a good advantage.

* If your schema has changed since the data has stored, what can you do to make that easier?
> Migration scripts. There's no way around having two different things in the db. A level of
  indirection can solve it.

> In traditional dbs where you have a schema, you have to migrate. You could do the same thing
  with datomic. The benefit is that you don't HAVE to do that. Whether you want to do it all in
  one shot or maybe there's data that you don't touch much.


* Do you always start with the domain or do you write the plumbing first?
> When creating a new product it's helpful to start with the UI. Before you implement a feature
  other than the specific UI instance, you might want to think about the domain model.

* Coming from an OO background, there seems to be more options for abstraction. On a larger team
  as you're factoring out abstracted methods, what kind of strategy have you seen to make sure
  everyone knows about these generic/abstract methods.
> Everyone talks to some DSL and the DSL talks to the domain, one layer above domain. Translates to
  domain functions. It's also a good thing to be able to share with the team.
