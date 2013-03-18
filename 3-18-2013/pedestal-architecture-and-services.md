Pedestal Architecture and Services
==================================
by Tim Ewald
------------------

# Introducing Pedestal #

* Who: Relevance
* What: alpha release, open source libs
* Where: Clojure/West
* When: Now
* Why, How...

# Goal #

* Use clojure end-to-end to build rich interactive collaborative Web applications and services
  that scale.

# Demo #

* Clojurescript based applications interacting with common backend (datomic)

# Archetype #

## Problems ##

* Services notifying app
* Building complex UIs in browser

## Service Plumbing ##

### Ring Middlewares ###

* Chained fns bound to thread's stack

### Interceptors ###

* maps of fns not bound to thread's stack

#### Pause/Resume ####

* can pause/resume across threads
* supports bindings and error propagation

### Ring compatibility ###

* as compatible as possible
* same request/response maps
* core middlewares refactored and accepted
** interceptor version provided
* easy to port exiting code

## Notifications ##

* Thread management enables long polling
** Park request as needed
* Also, server-sent-events
** Build on low-level streaming API

## Routes and URLs ##

    ;; ring handler fn
    (defn hello-world [req]
      (ring/response (map inc [1 2 3])))         ;; native edn serialization
    
    (defroutes routes
      [[["/hello-world" {:get hello-world}]]])   ;; routes as data
    
    (def url-for (routes/url-for-routes routes)) ;; make urls from routes
    
    (url-for ::hello-world)
    ;=> "/hello-world"


