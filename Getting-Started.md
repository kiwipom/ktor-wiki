### Create a project 
Create a Kotlin project [with Maven](https://kotlinlang.org/docs/reference/using-maven.html) 
  or [with Gradle](https://kotlinlang.org/docs/reference/using-gradle.html)
  
If you prefer to use some other project system, refer to the documentation for that system to learn 
how to set up dependencies.      

### Repository

Ktor artifacts are hosted on [Bintray](https://bintray.com/kotlin/ktor). You will need to add Ktor repository
 to your build/package system of choice:

Maven:
```maven
<repository>
    <id>bintray-kotlin-ktor</id>
    <name>bintray</name>
    <url>http://dl.bintray.com/kotlin/ktor</url>
</repository>
```

Gradle:
```gradle
    maven { url  "http://dl.bintray.com/kotlin/ktor" }
```

### Artifacts
 
All artifacts in Ktor belong to `org.jetbrains.ktor` group. You can check what is the latest version
 on [Bintray](https://bintray.com/kotlin/ktor)
    
Ktor is split into several groups of modules:
* `ktor-core` is a core package where most of the application API and implementation is located. 
* `ktor-hosts` contains modules that support hosting Ktor Application in different hosts: Netty, Jetty, Tomcat, and 
generic servlet. It also contains TestHost for setting up application tests without starting real host.
  * `ktor-jetty` support a deployed or embedded Jetty instance
  * `ktor-netty` supports Netty in an embedded mode
  * `ktor-tomcat` supports Tomcat servers
  * `ktor-servlet` is used by Jetty and Tomcat and allows hosting in generic servlet container
  * `ktor-test-host` allows running application tests faster without starting full host
* `ktor-features` groups modules for features that are optional and may not be required by every application
  * `ktor-auth` provides support for different authentication systems like Basic, Digest, Forms, OAuth 1a and 2
  * `ktor-auth-ldap` adds ability to authenticate against LDAP instance
  * `ktor-freemarker` integrates Ktor with Freemarker templates
  * `ktor-html-builder` integrates Ktor with kotlinx.html builders
  * `ktor-locations` contains experimental support for typed locations
  * `ktor-server-sessions` adds ability to use stateful sessions stored on a server
  * `ktor-websockets` provides support for Websockets
  
  
> Typical Ktor application would need a single host dependency which brings ktor-core as well, and some of the ktor-features 
as needed   
   
### Create an Application function

Ktor application is defined by a single function that sets up an Application instance. 

```kotlin
fun Application.main() 
```

Application is responsible to process and respond on application calls (http requests). `ApplicationCall` holds both
 `request` and `response` and goes through the Application pipeline to handle the request.

Typical application will install several features, including `Routing` feature and configure routing to handle 
application calls at various endpoints:
```kotlin
fun Application.main() {
    install(DefaultHeaders) // installs a feature that adds default headers to every response
    install(CallLogging) // installs a feature that sends successful and failed requests' information to the log
    install(Routing) { // installs and configures a routing
        get("/") { // GET request on a root of the server
            call.respondText("Hello, World!") // respond on a call with plain text
        }
    }
}
```

See also: [Routing](Feature-Routing), [Lifecycle](Lifecycle)

### Create an embedded application

Creating an embedded application is as easy as calling a function using a host of choice. If you want to create an
embedded web application using Netty, you will need to add dependency to `ktor-netty` module and 
call `embeddedNettyServer` function. 

You can configure application right there in the configuration block: 

```kotlin
embeddedNettyServer(8080) {
    routing {
        get("/") {
            call.respondText(ContentType.Text.Html, "Hello, world!")
        }
    }
}.start(wait = true)
```

Alternatively, you can pass existing `Application.main` function:
 ```kotlin
embeddedJettyServer(8080, configure = Application::main)
```

### Hosting an application in an external host

When you need to host Ktor application in an independently maintained host, you will need an `application.conf` file
to tell Ktor how to start your application. 

In resources folder create file `application.conf` with the following content
```
ktor {
    deployment {
        environment = development
        port = 8080
    }

    application {
        modules = [ my.company.MyApplication.ApplicationKt.main ]
    }
}
```

Replace `my.company.MyApplication` with the package of your application, and `ApplicationKt` with then name of the
file your `Application.main` function is located.

Running hosted application in a development environment is supported by using development hosts 

* Create Run Configuration in IntelliJ IDEA using "Application Template"
* use DevelopmentHost as a class for `main` function.
  * Netty: use `org.jetbrains.ktor.netty.DevelopmentHost` 
  * Jetty: use `org.jetbrains.ktor.jetty.DevelopmentHost` 
    
See also: [Hosting](Hosting), [Configuration](Configuration)

### Use automatic reloading
Ktor can automatically reload Application when changes to class files are detected, i.e. when you build the Application.
Enable this feature by adding `autoreload` configuration to `application.conf`:
```conf
ktor {
    deployment {
        environment = development
        port = 8080
        autoreload = false
        watch = [ my.company ]
    }

    …
}
```

See also: [Automatic Reload](Automatic-Reload)
