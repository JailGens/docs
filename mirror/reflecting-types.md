# Reflecting Types

To obtain a `TypeDefinition` object, you can do the following:

{% tabs %}
{% tab title="Java" %}
```java
class Car {}

TypeDefinition<Car> carType = mirror.reflect(Car.class);
String name = carType.getName()

assert name.equals("Car");
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
class Car {}

val carType = mirror.reflect(Car.class)
val name = carType.name

assert(name == "Car");
```
{% endtab %}

{% tab title="Groovy" %}
```groovy
class Car {}

def carType = mirror.reflect(Car)
def name = carType.name

assert name =="Car"
```
{% endtab %}
{% endtabs %}

## Fields

{% tabs %}
{% tab title="Java" %}
```java
class Car {
    int fuel;
    Car(int fuel) { ... }
}

Car car = new Car(100);
Field<Car, Integer> fuelField = (Method<Car, Integer>) carType.getFields()
        .stream()
        .filter((m) -> m.getName().equals("fuel"))
        .findAny()
        .orElseThrow();
fuelField.set(car, 10);
int fuel = fuelField.get();

assert fuel == 10;
assert car.fuel == 10;
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
class Car(fuel: Int)

val car = Car(100)
val method = carType.getFields()
        .stream()
        .filter { it.name == "fuel" }
        .findAny()
        .orElseThrow() as Field<Car, Int>
fuelField.set(car, 10)
val fuel = fuelField.get()

assert(fuel == 10)
assert(car.fuel == 10)
```
{% endtab %}

{% tab title="Groovy" %}
```groovy
class Car {
    int fuel;
    Car(int fuel) { ... }
}

def car = new Car(100);
def fuelField = (Method<Car, Integer>) carType.getFields()
        .stream()
        .filter { it.name == "fuel" }
        .findAny()
        .orElseThrow();
fuelField.set(car, 10);
def fuel = fuelField.get();

assert fuel == 10;
assert car.fuel == 10;
```
{% endtab %}
{% endtabs %}

## Constructors

{% tabs %}
{% tab title="Java" %}
```java
class Car {
    int fuel;
    Car(int fuel) { ... }
}

Constructor<Car> constructor = carType.getConstructors()
        .stream()
        .findAny()
        .orElseThrow();
Car car = constructor.construct(100);

assert car.fuel == 100;
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
class Car(fuel: Int)

val carConstructor = carType.getConstructors()
        .stream()
        .findAny()
        .orElseThrow()
val car = carConstructor.construct(100)

assert(car.fuel == 100)
```
{% endtab %}

{% tab title="Groovy" %}
```groovy
class Car {
    int fuel;
    Car(int fuel) { ... }
}

def constructor = carType.getConstructors()
        .stream()
        .findAny()
        .orElseThrow()
def car = constructor.construct(100)

assert car.fuel == 100
```
{% endtab %}
{% endtabs %}

## Methods

{% tabs %}
{% tab title="Java" %}
```java
class Car {
    int fuel;
    Car(int fuel) { ... }
    int getFuel() { ... }
}

Car car = new Car(100);
Method<Car, Integer> method = (Method<Car, Integer>) carType.getMethods()
        .stream()
        .filter((m) -> m.getName().equals("getFuel"))
        .findAny()
        .orElseThrow();
int fuel = method.invoke(car);

assert fuel == 100;
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
class Car(fuel: Int)

val car = Car(100)
val method = carType.getMethods()
        .stream()
        .filter { it.name == "getFuel" } // generated java method
        .findAny()
        .orElseThrow() as Method<Car, Int>
val fuel = method.invoke(car)

assert(fuel == 100)
```
{% endtab %}

{% tab title="Groovy" %}
```groovy
class Car {
    int fuel;
    Car(int fuel) { ... }
    int getFuel() { ... }
}

def car = new Car(100)
def method = (Method<Car, Integer>) carType.getMethods()
        .stream()
        .filter { it.name == "getFuel" }
        .findAny()
        .orElseThrow()
def fuel = method.invoke(car)

assert fuel == 100
```
{% endtab %}
{% endtabs %}
