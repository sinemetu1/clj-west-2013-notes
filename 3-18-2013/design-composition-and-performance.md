Design, Composition, and Performance
==============
by Rich Hickey
--------------

# Design #

* to prepare the plans for (a work to be executed), especially to plan the form and structure of
* designare - to mark out
* make a plan, write it down

# But... #

We already write down code
* do we still need designs?

Generate docs from implementation?
* not a plan

Monolithic designs, ugh, been there
* plans, not good ones

# Good Design #

* Separating into things that can be composed
* Each component should be 'about' one or a few things
* Composing them to solve a problem
* Iterative

## Design is taking things apart ##

### Take apart requirements ###

* Move from want/need
** to problems
* knowns/unkowns
* domain-side/solution-side
* cause/symptom
* unstated

### Take apart time/order/flow ###

* queues
* idempotency
* commutation
* transactions

### Take apart place, participants ###

* Also authors
** independent development etc

### Information vs Mechanism ###

* Set of logged-in users
* Set class/construct

### Take apart solutions ###

* Benefits
* Tradeoffs
* Costs
* Problem fit

### Why Design? ###

* Comprehension
* Coordination
* Extension
* Reuse
* Testing
* Efficiency

# Composition and Performance #

* Bartok
* Coltrane

## Composition ##

* Self-imposed problems/constraints
** like other art forms
* Design for performers
** ditto screenwriting, choreography...
* Organization challenge
** a plan or design addressing those challenges

## Specificity and Scale ##

* Fully orchestrated/arranged
** typical at larger scales
* Melody + changes
** increased latitude for performers
** increased responsibility

## Constraints ##

* Most compositions are 'about' one or a few things
** melodic, harmonic, rhythmic, timbral ideas
* motif or theme
** variations
** resolution
* Larger works, more structural components

### Improvisation ###

* improvisus - not foreseen/provided
* Melody + changes provide constraints
** Peformer provides variations
* Tremendous preparation, practice, study!
* Deep musical knowledge and vocabulary

### Harmony ###

* 'accord, congruity'
* 'simultaneous combination (of tones)'
* 'the art or science concerned with the structure and combinations (of chords)'
* Harmonic sensibility is a key design skill

### Bartok and Coltrane ###

* Masters of harmony
* Students of harmoniousness
** Beyond the rules
* New systems that preserve/explore harmonic essence
* Towering intellectual effort, while totally rocking

### What does it have to do with Clojure? ###

* Is Clojure like a song?
* Is it like a syphony?
* An instrument.

#### Excitation ####

* Most instruments are 'about' one thing
* Pluck, vibrate, strike...

#### Control/Interface ####

#### Projection ####

#### Resonance ####

#### Instruments are limited ####

* Piano
** no in-between notes
* Sax et al
** one note at a time
* Minimal, yet sufficient
** no missing notes

#### Constraint Network ####

#### No one wants to play a choose-o-phone ####

#### No one wants to compose for choose-o-phone ####

* Complex target
* Nested design problem

#### Instruments are for players ####

#### Beginners are not yet players ####

* Should cellos...
** auto-tune?
** have green/red lights when you are in/out of tune?
** not make any sound until you get it right?

#### Players wanted ####

<rant></rant>

#### Humans are incredible ####

* Learners
* Teachers
* Neither are effort free

#### We are novices ####

* Briefly
* Permanently

#### Effort ####

#### Instruments (and tools) are usually for one user ####

#### Planning/Performance ####

* What ratio of time spend?
** compose/study/practice
** vs perform/record
* Why do we think we can just show up?
** unlike other creative people

#### But... ####

* Coltrane couldn't build a web site in a day
<rant></rant>

* Software isn't made of wood or metal

#### Theremin ####

#### Modularity ####

#### Design Stack ####

#### Code all the way down ####

* In software, same mechanism at every layer
* We all have soldering irons
* Doesn't mean we can do filter design
* Distraction, expansion

#### The paralysis of choice ####

* The impetus of constraint
* Constraint is a driver of creativity

#### Quit fidgeting ####

Carolina Eyck

### Design is imagining ###

* Embrace the constraints
* Be optimistic
* Imagine a lot

### Design is making decisions ###

* Admin little
* Value conveyed is in decisions made
* Leaving all options open you're just avoiding design

### Performing is preparing ###

* practice
* study
* developing design sensibilities you can deploy on the fly

## Take things apart ##

* with an eye towards putting them back together

## Design like Bartok ##

* how will everything get put back together

## Code like Coltrane ##

* deep knowledge of software and design sensibilities
* unreasonably effective

## Langs and libs like instruments ##

* simple designs

## Pursue Harmony ##
