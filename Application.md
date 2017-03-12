`Application` instance is a main unit of a web application. It owns pipeline for handling
application requests. HTTP requests received by host are translated into `ApplicationCall` instances.
`ApplicationCall` enters pipeline and goes through installed interceptors until it is fully handled 
and `ApplicationResponse` is completed.

`ApplicationCall` is an act of communication between client and server. It provides access to `ApplicationRequest` and
`ApplicationResponse`, as well as lots of useful functions to respond to a client's request. Given that pipeline can be
executed asynchronously it also represents logical execution context, with `Attributes` to pass data between various
parts of a pipeline.

Installing interceptor into pipeline is primary method to alter processing of an `ApplicationCall`.
Almost all features of Ktor are interceptors that perform various operations in different phases of
application call processing. 

```kotlin
    intercept(ApplicationCallPipeline.Call) { 
        if (call.request.uri == "/")
            call.respondText("Test String")
    }
```
This code installs an interceptor into `Call` phase of an `ApplicationCall` processing, and responds with plain text
when request is asking for a root page.  

Of course, you don't need to handle URIs manually like this, there is a [routing](Feature-Routing) facility that does this
 and a lot more for you. 
 
Most functions available on `ApplicationCall` (such as `respondText` above) are `suspend` functions, indicating that they 
can potentially execute asynchronously.
 
See advanced topic [Pipeline](Advanced-Pipeline) for more information on mechanics of processing `ApplicationCall`s 



### The Application Object

A Ktor Application typically consists of a series of [Features](Features) and one or more routes that handle requests. You can think of features as functionality 
that is injected into the request and response pipeline. A typical application would have a series of features such as `DefaultHeaders` which add headers to every outgoing
response. One core feature is `Routing` which allows us to define routes to handle requests. 

```kotlin
fun Application.main() {
    install(DefaultHeaders) 
    install(Routing) { 
        get("/") { 
            call.respondText("Hello, World!")  
        }
    }
}
```

