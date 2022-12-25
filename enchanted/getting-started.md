---
description: Get started with Enchanted
---

# Getting Started

{% hint style="danger" %}
This version is unstable. Anything may change.
{% endhint %}

## About

Enchanted is a lightweight, annotation-driven command library.

## Installation

Add the following to your build file:

{% tabs %}
{% tab title="Gradle (Kotlin DSL)" %}
```kts
repositories {
    maven("https://repo.jailgens.net/releases")
}

dependencies {
    implementation("net.jailgens:enchanted-paper:0.1.0")
}
```
{% endtab %}

{% tab title="Gradle (Groovy DSL)" %}
```groovy
repositories {
    maven {
        url 'https://repo.jailgens.net/releases'
    }
}

dependencies {
    implementation 'net.jailgens:enchanted-paper:0.1.0'
}
```
{% endtab %}
{% endtabs %}

We use [SemVer](https://semver.org/).

## Documentation

📔 [JavaDocs](https://repo.jailgens.net/javadoc/snapshots/net/jailgens/enchanted-api/0.1.0-SNAPSHOT)

## Repository

[JailGens/enchanted](https://github.com/JailGens/enchanted)
