## Hosting Applications

Ktor Applications can be self-hosted or hosted in an Application Server. This section shows to how host Ktor applications externally.

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

#### Use automatic reloading

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
