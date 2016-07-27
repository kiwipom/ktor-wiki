# Ktor Documentation

* [Getting Started](Getting-Started)
* [Under the Hood](Under-the-Hood)

# Quick Start

### Create a project 
* Create a Kotlin project [with Maven](https://kotlinlang.org/docs/reference/using-maven.html) 
  or [with Gradle](https://kotlinlang.org/docs/reference/using-gradle.html)
* Add dependency to ktor:
  * repository: http://dl.bintray.com/kotlin/ktor
  * group: org.jetbrains.ktor
  * artifact: ktor-core
  * version: LATEST
  
If you prefer to use some other project system, refer to the documentation for that system to learn how to set up
dependencies. You can download artifacts directly from [bintray repository](https://bintray.com/kotlin/ktor/ktor) 
  
### Create an Application class
```kotlin
class MyApplication(environment: ApplicationEnvironment) : Application(environment) {
    init {
      
    }
}
```

See also: [Application Lifecycle](Lifecycle)

### Create an application configuration file
In resources folder create file `application.conf` with the following content
```
ktor {
    deployment {
        environment = development
        port = 8080
    }

    application {
        class = my.company.MyApplication
    }
}
```
Replace `my.company.MyApplication` with the fully-qualified name of your application class.

See also: [Application Configuration](Configuration)
  
### Add routes to application
In the Application `init` block add routing section:
```kotlin
        routing {
            get("/") {
                call.respondText(ContentType.Text.Plain, "Hello, World!")
            }
        }
```

See also: [Routing](Routing)

### Run the application
Use one of the following options:
* use DevelopmentHost as a class for `main` function.
  * Netty: use `org.jetbrains.ktor.netty.DevelopmentHost` 
  * Jetty: use `org.jetbrains.ktor.jetty.DevelopmentHost` 
  
* use embedded server and start it in your own `main` function 
  * Netty: `embeddedNettyServer` 
  * Jetty: `embeddedJettyServer`
  
  
See also: [Hosting Application](Hosting)

### Use automatic reloading
Ktor can automatically reload Application when changes to class files are detected, i.e. when you build the Application.
To enabled this feature, add `autoreload` configuration to `application.conf`:
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

See also: [Automatic Reloading](Autoreload)
