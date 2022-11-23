# Getting Started

## About

Mirror is a Java reflection wrapper that provides a clean, and easy-to-use API.

## Installation

Add the following to your build file:

{% tabs %}
{% tab title="Gradle (Kotlin DSL)" %}
```kts
repositories {
    maven("https://repo.sparky983.me/releases")
}

dependencies {
    implementation("net.jailgens:mirror:0.4.0")
}
```
{% endtab %}

{% tab title="Gradle (Groovy DSL)" %}
```groovy
repositories {
    maven {
        url "https://repo.sparky983.me/releases"
    }
}

dependencies {
    implementation "net.jailgens:mirror:0.4.0"
}
```
{% endtab %}

{% tab title="Maven" %}
```xml
<repositories>
    <repository>
        <id>sparky</id>
        <url>https://repo.sparky983.me/releases</url>
    </repository>
</repositories>

<dependencies>
    <dependency>
        <groupId>net.jailgens</groupId>
        <artifactId>mirror</artifactId>
        <version>0.4.0</version>
    </dependency>
</dependencies>
```
{% endtab %}
{% endtabs %}

We use [SemVer](https://semver.org/).

## Documentation

📔 [JavaDocs](https://repo.sparky983.me/javadoc/releases/net/jailgens/mirror/0.4.0)

## Repository

[JailGens/mirror](https://github.com/JailGens/mirror)
