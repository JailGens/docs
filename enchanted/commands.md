# Commands

Commands can be declared using the [`@net.jailgens.enchanted.annotations.Command`](https://github.com/JailGens/enchanted/blob/main/enchanted-api/src/main/java/net/jailgens/enchanted/annotations/Command.java) annotation on a class:

{% tabs %}
{% tab title="Java" %}
```java
@Command("foo")
public class Foo {}
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
@Command("foo")
class Foo
```
{% endtab %}
{% endtabs %}

## Subcommands

You can add a subcommand to a command by annotating a public method with [`@net.jailgens.enchanted.annotations.Command`](https://github.com/JailGens/enchanted/blob/main/enchanted-api/src/main/java/net/jailgens/enchanted/annotations/Command.java). The annotated method will be invoked whenever the specified subcommand is executed.

{% tabs %}
{% tab title="Java" %}
```java
@Command("foo")
public class Foo {
    @Command("bar")
    public void bar() {
        System.out.println("bar");
    }
}
```

This `Foo.bar()` is invoked when the `/foo bar` command is executed.&#x20;
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
@Command("foo")
class Foo {
    @Command("bar")
    fun bar() {
        println("bar")
    }
}
```

This will be executed if the `/foo bar` command is executed.&#x20;
{% endtab %}
{% endtabs %}

## Default Commands

Default commands are commands that are run without the need for a subcommand. They are declared by annotating a public method with [`@Command.Default`](https://github.com/JailGens/enchanted/blob/main/enchanted-api/src/main/java/net/jailgens/enchanted/annotations/Command.java#L45):

{% tabs %}
{% tab title="Java" %}
```java
@Command("foo")
public class Foo {
    // Normal import
    // e.g. import net.jailgens.enchanted.annotations.Command;
    @Command.Default
    public void foo() {
        System.out.println("foo");
    }
    
    // Import inner class
    // e.g. import net.jailgens.enchanted.annotations.Command.Default;
    @Default
    public void foo() {
        System.out.println("foo");
    }
}
```

Both methods are invoked upon the execution of `/foo`.
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
@Command("foo")
class Foo {
    // Normal import
    // e.g. import net.jailgens.enchanted.annotations.Command
    @Command.Default
    fun foo() {
        println("foo")
    }
    
    // Import inner class
    // e.g. import net.jailgens.enchanted.annotations.Command.Default
    @Default
    fun foo() {
        println("foo")
    }
}
```

Either command is executed when the `/foo` command is run.
{% endtab %}
{% endtabs %}

You can combine default commands and subcommands:

{% tabs %}
{% tab title="Java" %}
```java
@Command("foo")
public class Foo {
    // /foo
    @Command.Default
    public void foo() {
        System.out.println("foo");
    }
    
    // /foo bar
    @Command("bar")
    public void bar() {
        System.out.println("bar");
    }
    
    // /foo baz
    @Command("baz")
    public void baz() {
        System.out.println("baz");
    }
}
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
@Command("foo")
class Foo {
    // /foo
    @Command.Default
    fun foo() {
        println("foo")
    }
    
    // /foo bar
    @Command("bar")
    public void bar() {
        println("bar")
    }
    
    // /foo baz
    @Command("baz")
    public void baz() {
        println("baz")
    }
}
```
{% endtab %}
{% endtabs %}

## Command Executors

_Not to be confused with_ [_`org.bukkit.command.CommandExecutor`_](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/command/CommandExecutor.html)_._

Command executors are simply something that can execute a command. You can obtain it by adding a [`CommandExecutor`](https://github.com/JailGens/enchanted/blob/main/enchanted-api/src/main/java/net/jailgens/enchanted/CommandExecutor.java) parameter to any subcommand or default command. Some platforms such as Paper provide their own additional command executor's like [`org.bukkit.command.CommandSender`](https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/command/CommandSender.html).

{% tabs %}
{% tab title="Java" %}
```java
@Command("foo")
public class Foo {
    // (Paper) anyone can execute /foo bar
    @Command("bar")
    public void bar(CommandSender sender) {}
    
    // (Paper) only players can execute /foo baz
    @Command("baz")
    public void baz(Player sender) {}
    
    // Anyone can execute this /foo qux, reguardless of implementation or platform
    @Command("qux")
    public void qux(CommandExecutor sender) {}
}
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
@Command("foo")
class Foo {
    // (Paper) anyone can execute /foo bar
    @Command("bar")
    fun bar(sender: org.bukkit.command.CommandSender) {}
    
    // (Paper) only players can execute /foo baz
    @Command("baz")
    fun baz(sender: org.bukkit.entity.Player) {}
    
    // Anyone can execute this /foo qux, regardless of implementation or platform
    @Command("qux")
    fun qux(sender: net.jailgens.enchanted.CommandExecutor) {}
}
```
{% endtab %}
{% endtabs %}

As shown in previous sections, if no command executor is specified, the subcommand or default command can be executed by any [`CommandExecutor`](https://github.com/JailGens/enchanted/blob/main/enchanted-api/src/main/java/net/jailgens/enchanted/CommandExecutor.java).

## Arguments

Command arguments are really easy to define. Simply append a parameter to the end of your command method:

{% tabs %}
{% tab title="Java" %}
```java
@Command("echo")
public class Echo {
    @Command.Default
    public void echo(CommandExecutor sender, String string) {
        sender.sendMessage(string);
    }
}
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
@Command("echo")
class Echo {
    // echo <message>
    // Example:
    // Command: /echo hello
    // Response: hello
    @Command.Default
    fun echo(sender: CommandExecutor, string: String) {
        sender.sendMessage(string)
    }
}
```
{% endtab %}
{% endtabs %}

You can add additional behaviour to several different aspects of how an argument works such as:

* How it is parsed
* How it gets converted into an object
* If it is required

See [arguments.md](arguments.md "mention") for more details.

## Command Groups

Command groups are simply nested subcommands. Their behaviour and definition is just what you'd expect:

{% tabs %}
{% tab title="Java" %}
```java
@Command("foo")
public class Foo {
    @Command("bar")
    public class Bar {
        // foo bar baz
        @Command("baz")
        public void baz() {}
        
        // foo bar qux
        @Command("qux")
        public void qux() {}
    }
}
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
@Command("foo")
class Foo {
    @Command("bar")
    class Bar {
        // foo bar baz
        @Command("baz")
        fun baz() {}
        
        // foo bar qux
        @Command("qux")
        fun qux() {}
    }
}
```
{% endtab %}
{% endtabs %}

## Command Metadata

The Enchanted API defines three metadata annotations:

* [`@Description`](https://github.com/JailGens/enchanted/blob/main/enchanted-api/src/main/java/net/jailgens/enchanted/annotations/Description.java) - adds a description to a command or subcommand.
* [`@Param`](https://github.com/JailGens/enchanted/blob/main/enchanted-api/src/main/java/net/jailgens/enchanted/annotations/Param.java) - sets the name of a parameter.
* [`@Usage`](https://github.com/JailGens/enchanted/blob/main/enchanted-api/src/main/java/net/jailgens/enchanted/annotations/Usage.java) - sets the usage of a command, subcommand or default command.

## Command Aliases

Aliases are a way to add secondary names to a command:

{% tabs %}
{% tab title="Java" %}
```java
@Command("gamemode")
@Aliases({"gm"})
public class GameMode { ... }
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
@Command("gamemode")
@Aliases(["gm"])
class GameMode { ... }
```
{% endtab %}
{% endtabs %}
