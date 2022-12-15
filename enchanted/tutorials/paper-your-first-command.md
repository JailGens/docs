---
description: >-
  Learn how to create a simple hello world plugin with Enchanted and the Paper
  API.
---

# \[Paper] Your First Command

## Prerequisites

* [getting-started.md](../getting-started.md "mention")
* [the-command-manager.md](../the-command-manager.md "mention")

## Creating the Command

Every command must be annotated with [`@Command`](https://github.com/JailGens/enchanted/blob/main/enchanted-api/src/main/java/net/jailgens/enchanted/annotations/Command.java). If you get an error, double check that you didn't accidentally use `net.jailgens.enchanted.Command`.&#x20;

{% tabs %}
{% tab title="Java" %}
```java
@Command("hello-world")
public class HelloWorldCommand {}
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
@Command("hello-world")
class HelloWorldCommand
```
{% endtab %}
{% endtabs %}

If you have typed the command name in incorrectly, you should get a warning inside your IDE.

<figure><img src="../../.gitbook/assets/ide-warning.png" alt="IDE Warning"><figcaption></figcaption></figure>

Now, simply create a method annotated with [`@Command.Default`](https://github.com/JailGens/enchanted/blob/main/enchanted-api/src/main/java/net/jailgens/enchanted/annotations/Command.java#L45) and inside print "Hello, world!"

{% tabs %}
{% tab title="Java" %}
```java
@Command("hello-world")
public class HelloWorldCommand {
    @Command.Default
    public void sayHelloWorld() {
        System.out.println("Hello, World!");
    }
}
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
@Command("hello-world")
class HelloWorldCommand {
    @Command.Default
    fun sayHelloWorld() {
        println("Hello, world!")
    }
}
```
{% endtab %}
{% endtabs %}

All [`@Command.Default`](https://github.com/JailGens/enchanted/blob/main/enchanted-api/src/main/java/net/jailgens/enchanted/annotations/Command.java#L45)  tells enchanted is that the annotated method should run when the user executes the "hello-world" command without specifying any subcommand.

## Register the Command

Command registration is fairly simple, just call [`CommandManager.registerCommand(Object)`](https://github.com/JailGens/enchanted/blob/main/enchanted-api/src/main/java/net/jailgens/enchanted/CommandManager.java#L32) `` with the command you just created:

{% tabs %}
{% tab title="Java" %}
```java
public class YourPlugin extends JavaPlugin {
    public void onEnable() {
        PaperCommandManager commandManager = PaperCommandManager.create(this);
        commandManager.registerCommand(new HelloWorldCommand());
    }
}
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
class YourPlugin : JavaPlugin() {
    fun onEnable() {
        val commandManager = PaperCommandManager.create(this);
        commandManager.registerCommand(HelloWorldCommand());
    }
}
```
{% endtab %}
{% endtabs %}

Now, simply start up your server, and see the output on console.
