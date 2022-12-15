---
description: An introduction to the command manager.
---

# The Command Manager

The [`CommandManager`](https://github.com/JailGens/enchanted/blob/main/enchanted-api/src/main/java/net/jailgens/enchanted/CommandManager.java) class is the central class that manages all your commands. On the Paper platform it can be instantiated via [`PaperCommandManager.create(Plugin)`](https://github.com/JailGens/enchanted/blob/main/enchanted-paper/src/main/java/net/jailgens/enchanted/PaperCommandManager.java#L31):

{% tabs %}
{% tab title="Java" %}
```java
PaperCommandManager commandManager = PaperCommandManager.create()
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
val commandManager = PaperCommandManager.create()
```
{% endtab %}
{% endtabs %}

## Command Registration

You can register a command by calling [`CommandManager.registerCommand(Object)`](https://github.com/JailGens/enchanted/blob/main/enchanted-api/src/main/java/net/jailgens/enchanted/CommandManager.java#L32). The `Object` argument is an object annotated with [`@net.jailgens.enchanted.annotations.Command`](https://github.com/JailGens/enchanted/blob/main/enchanted-api/src/main/java/net/jailgens/enchanted/annotations/Command.java).&#x20;

{% tabs %}
{% tab title="Java" %}
<pre class="language-java"><code class="lang-java"><strong>@Command("example")
</strong>class Example {}

commandManager.registerCommand(new Example());
</code></pre>
{% endtab %}

{% tab title="Kotlin" %}
<pre class="language-kotlin"><code class="lang-kotlin"><strong>@Command("example")
</strong><strong>class Example
</strong><strong>
</strong><strong>commandManager.registerCommand(Example())
</strong></code></pre>
{% endtab %}
{% endtabs %}

Learn about [commands](commands.md).&#x20;
