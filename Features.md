Ktor is an extensible system and each extension comes in the form of a `Feature`. A feature is installed into application
using an `install` function which accepts an instance (typically an `object`) of `ApplicationFeature` type. 

The following example installs a [Routing](Feature-Routing) feature and configures it:
```kotlin
fun Application.main() {
    install(Routing) {
        get("/") {
            call.respondText("Hello, World!")
        }
    }
}
```

Ktor comes with a number of ready-made features that can be installed into your application:

> Some features might need adding an extra dependency to your project. See feature pages for more details.

### Application Features

* [Routing](Feature-Routing): attaches code to specific path/query/method/header and extract parameters from placeholders
* [Sessions](Feature-Sessions): stores and retrieves additional information attached to client session
* [Transform](Feature-Transform): transforms response content on the fly and utilise unified mechanism to build a response
* [Authentication](Feature-Authentication): authenticates client using Basic, Digest, Form, OAuth (1a & 2)
* [Status Pages](Feature-Status-Pages): sends custom content for specific status responses such as 404 Not Found
* [File type mapping](Feature-File-Mapping): maps file extension to mime type for static file serving
* [Static content](Feature-Static-Content): serves streams of data from local file system with full asynchronous support

### HTTP transport features

* [Compression](Feature-Compression): enables gzip/deflate compression when client accepts it
* [Conditional Headers](Feature-Conditional-Headers): sends 304 Not Modified response when if-modified-since/etag indicate content is the same
* [Partial Content](Feature-Partial-Content): sends partial content for streaming ranges, like in video streams
* [HEAD response](Feature-Head-Response): responds to HEAD requests by running through pipeline and dropping response body
* [CORS](Feature-CORS): verifies and sends headers according to cross-origin resource sharing control
* [HSTS and https redirect](Feature-HSTS): supports strict transport security

### Custom features

You can develop your own features and reuse them across your Ktor applications, or share with the community. Typical 
feature has the following structure:

```kotlin
class CustomFeature(configuration: Configuration) {
    val prop = configuration.prop // get snapshot of config
    
    class Configuration {
       var prop = "value" // mutable property
    }

    companion object Feature : ApplicationFeature<ApplicationCallPipeline, CustomFeature.Configuration, CustomFeature> {
       // feature unique key
       override val key = AttributeKey<CustomHeader>("CustomFeature")
       override fun install(pipeline: ApplicationCallPipeline, configure: Configuration.() -> Unit): CustomFeature {
           // installation script       
       }
    }
}
```

`CustomFeature` here is a feature instance class, which should be immutable to avoid unintended side-effects in a highly
concurrent environment. 
`Configuration` instance is handed to the user installation script and allows for feature configuration. 
`Feature` companion object conforms to Ktor API and wires things together.
 
Feature can be installed with the standard `install` function:
```kotlin
fun Application.main() {
    install(CustomFeature) { // Install a custom feature
        prop = "Hello" // configuration script
    }
}
```

See complete example in a [custom feature sample](https://github.com/Kotlin/ktor/blob/master/ktor-samples/ktor-samples-custom-feature/src/org/jetbrains/ktor/samples/feature/CustomHeader.kt)