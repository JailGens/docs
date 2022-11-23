# The Mirror

The `Mirror` is the main class that allows for reflecting types.&#x20;

{% tabs %}
{% tab title="Java" %}
```java
Mirror mirror = Mirror.builder().build();
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
val mirror = Mirror.builder().build()
```
{% endtab %}

{% tab title="Groovy" %}
```groovy
def mirror = Mirror.builder().build()
```
{% endtab %}
{% endtabs %}

You can specify whether to enable/disable `TypeDefinition` caching via the `Mirror.Builder#cache` method. By default this is `true`.

{% tabs %}
{% tab title="Java" %}
```java
Mirror mirror = Mirror.builder()
        .cache(false)
        .build();
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
var mirror = Mirror.builder()
        .cache(false)
        .build()
```
{% endtab %}

{% tab title="Groovy" %}
```groovy
def mirror = Mirror.builder()
        .cache(false)
        .build()
```
{% endtab %}
{% endtabs %}

If you would like to specify a class loader for [proxies](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Proxy.html#newProxyInstance-java.lang.ClassLoader-java.lang.Class:A-java.lang.reflect.InvocationHandler-) you can do so:

{% tabs %}
{% tab title="Java" %}
```java
Mirror mirror = Mirror.builder()
        .classLoader(Object.class.getClassLoader())
        .build();
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
val mirror = Mirror.builder()
        .classLoader(Test::class.java.classLoader)
        .build()
```
{% endtab %}

{% tab title="Groovy" %}
```groovy
def mirror = Mirror.builder()
        .classLoader(Test.class.classLoader)
        .build()
```
{% endtab %}
{% endtabs %}

See [here](https://repo.sparky983.me/javadoc/releases/net/jailgens/mirror/0.4.0) for more details.

