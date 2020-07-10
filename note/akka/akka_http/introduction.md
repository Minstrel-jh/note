[akka]: /note/akka/README.md
[url:akka-http]: https://doc.akka.io/docs/akka-http/current/introduction.html

# 介绍

[返回][akka]

> [akka-http][url:akka-http]

1. akka-http是基于akka-actor和akka-stream的
2. akka-http的理念并非是提供一个web框架，而是提供一个基于http构建集成层的通用工具包
3. akka-http遵循一个更加开放的设计，并且为“做同一件事”提供了不同等级的API。你可以为应用程序选择最合适API抽象级别。比如你在使用high-level的API实现某件事时遇到了问题，很有可能可以通过一个low-level的API来完成这件事。获取更大的灵活性的同时，也可能需要你编写更多的应用程序代码

## 包

- Gradle

```groovy
compile group: 'com.typesafe.akka', name: 'akka-http_2.12',   version: '10.1.11'
compile group: 'com.typesafe.akka', name: 'akka-stream_2.12', version: '2.5.26'
```

## Routin DSL

```kotlin
import akka.actor.ActorSystem
import akka.http.javadsl.ConnectHttp
import akka.http.javadsl.Http
import akka.http.javadsl.ServerBinding
import akka.http.javadsl.server.AllDirectives
import akka.http.javadsl.server.Route
import akka.stream.ActorMaterializer
import akka.stream.Materializer
import java.util.concurrent.CompletionStage

fun main() {

    val serverApp = ServerApp().start()

    println("服务器启动，输入回车关闭...")
    System.`in`.read()

    serverApp.close()
}

class ServerApp: AllDirectives() {

    lateinit var system: ActorSystem
    lateinit var http: Http
    lateinit var materializer: Materializer

    lateinit var binding: CompletionStage<ServerBinding>

    fun start(): ServerApp {
        system = ActorSystem.create("routes")

        http = Http.get(system)
        materializer = ActorMaterializer.create(system)

        val routeFlow = this.createRoute().flow(system, materializer)

        binding = http.bindAndHandle(routeFlow, ConnectHttp.toHost("0.0.0.0", 8088), materializer)

        return this
    }

    fun close() {
        if (this::binding.isInitialized) {
            binding
                .thenCompose(ServerBinding::unbind) // 解除对端口的绑定
                .thenAccept {
                    println("http unbind 结束")
                }
        } else {
            println("http server未启动")
        }
    }

    fun createRoute(): Route {
        return concat(
            path("hello") {
                get {
                    complete("Say hello to akka-http")
                }
            }
        )
    }
}
```

## 1 Marshalling

```kotlin
entity(Jackson.unmarshaller())
```

## 2 Streaming
