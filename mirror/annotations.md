# Annotations

Mirror has its own annotation system that is safer but can also interop with existing Java annotations.

You can get an `Annotated` object's annotations with `Annotated.getAnnotations()`.&#x20;

The supported `Annotated` objects are:

* `Constructor`
* `Invokable`
* `Method`
* `Parameter`
* `ParameterizedType`
* `TypeDefinition`

{% tabs %}
{% tab title="Java" %}
```java
@Manufacturer("TOYOTA")
class Car {}

TypeDefinition<Car> carType = mirror.reflect(Car.class);
AnnotationValues annotations = carType.getAnnotations();

assert annotations.getString(AnnotationElement.value(Manufacturer.class))
        .orElseThrow(AssertionError::new).equals("TOYOTA");

Manufacturer manufacturer = carType.getRawAnnotation(Manufacturer.class);

assert manufacturer != null;
assert manufacturer.value().equals("TOYOTA");
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
@Manufacturer("TOYOTA")
class Car {}

val carType = mirror.reflect(Car.class)
val annotations = carType.getAnnotations()

assert(annotations.getString(AnnotationElement.value(Manufacturer::class.java))
        .orElseThrow(AssertionError::new) == "TOYOTA")

val manufacturer = carType.getRawAnnotation(Manufacturer::class.java)

assert(manufacturer?.value() == "TOYOTA")
```
{% endtab %}

{% tab title="Groovy" %}
```groovy
@Manufacturer("TOYOTA")
class Car {}

def carType = mirror.reflect(Car)
def annotations = carType.getAnnotations()

assert annotations.getString(AnnotationElement.value(Manufacturer))
        .orElseThrow(AssertionError::new) == "TOYOTA"

def manufacturer = carType.getRawAnnotation(Manufacturer)

assert manufacturer?.value() == "TOYOTA"
```
{% endtab %}
{% endtabs %}

## Creating Annotations

Mirror supports creating your own `AnnotationValues` instances via `AnnotationValues#builder()`.

{% tabs %}
{% tab title="Java" %}
```java
AnnotationValues annotations = AnnotationValues.builder()
        .annotate(Awesome.class)
        .value(AnnotationElement.value(Manufacturer.class), "TOYOTA")
        .build();

// equivalient to @Awesome @Manufacturer("TOYOTA")
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
val annotations = AnnotationValues.builder()
        .annotate(Awesome::class.java)
        .value(AnnotationElement.value(Manufacturer::class.java), "TOYOTA")
        .build()

// equivalient to @Awesome @Manufacturer("TOYOTA")
```
{% endtab %}

{% tab title="Untitled" %}
```groovy
def annotations = AnnotationValues.builder()
        .annotate(Awesome)
        .value(AnnotationElement.value(Manufacturer), "TOYOTA")
        .build()

// equivalient to @Awesome @Manufacturer("TOYOTA")
```
{% endtab %}
{% endtabs %}
