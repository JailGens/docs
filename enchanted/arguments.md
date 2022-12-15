# Arguments

## Argument Parsers

Argument parsers transform the input arguments (e.g. `["hello", "world]`) and usually combine  them to form a single string that can be used by [#argument-resolvers](arguments.md#argument-resolvers "mention").

### Default

By default if no annotation is specified, a method argument will map 1:1 with a single command argument. This means that if a command method's signature is `void command(... sender, String s)`, the `s` parameter will always map to the first command argument.

### Join

The [`@Join`](https://github.com/JailGens/enchanted/blob/main/enchanted-api/src/main/java/net/jailgens/enchanted/annotations/Join.java) annotation simply combines all the rest of the arguments and joins with the specified [delimiter](https://github.com/JailGens/enchanted/blob/main/enchanted-api/src/main/java/net/jailgens/enchanted/annotations/Join.java#L32) (defaults to " ").  &#x20;

{% hint style="warning" %}
Additional parameters after an `@Join` parameter may result in unexpected behaviour
{% endhint %}

{% tabs %}
{% tab title="Java" %}
```java
@Command("echo")
public class Echo {
    //    /echo hello         -> message="hello"
    // or /echo hello, world  -> message="hello, world"
    // or /echo 1 2 3 4 5 6   -> message="1 2 3 4 5 6"
    @Command.Default
    public void echo(CommandExecutor sender, @Join String message) {
        sender.sendMessage(message);
    }
}
```

And with the delimiter specified:

```java
@Command("echo")
public class Echo {
    //    /echo hello         -> message="hello"
    // or /echo hello, world  -> message="hello,,world"
    // or /echo 1 2 3 4 5 6   -> message="1,2,3,4,5,6"
    @Command.Default
    public void echo(CommandExecutor sender, @Join(",") String message) {
        sender.sendMessage(message);
    }
}
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
@Command("echo")
class Echo {
    //    /echo hello
    // or /echo hello, world
    // or /echo 1 2 3 4 5 6
    @Command.Default
    fun echo(sender: CommandExecutor, @Join message: String) {
        sender.sendMessage(message)
    }
}
```

And with the delimiter specified:

```kotlin
@Command("echo")
class Echo {
    //    /echo hello         -> hello
    // or /echo hello, world  -> hello,,world
    // or /echo 1 2 3 4 5 6   -> 1,2,3,4,5,6
    @Command.Default
    fun echo(sender: CommandExecutor, @Join(",") message: String) {
        sender.sendMessage(message)
    }
}
```
{% endtab %}
{% endtabs %}

## Argument Resolvers

### Default

The default argument resolver simply attempts to convert an argument into an object using the [#converter-registry](arguments.md#converter-registry "mention").

### Optional

The [`@Optional`](https://github.com/JailGens/enchanted/blob/main/enchanted-api/src/main/java/net/jailgens/enchanted/annotations/Optional.java) annotation provides the exact same behaviour as [#default-1](arguments.md#default-1 "mention"), but returns `null` if the argument is not present.

## Converter Registry

The [`ConverterRegistry`](https://github.com/JailGens/enchanted/blob/main/enchanted-api/src/main/java/net/jailgens/enchanted/ConverterRegistry.java) is responsible for converting a string argument into an object usable by either a subcommand or default command method. It is [open for extension](https://en.wikipedia.org/wiki/Open%E2%80%93closed\_principle) so you can register your own converters or override existing ones:

{% tabs %}
{% tab title="Java" %}
```java
CommandManager commandManager = ...;
commandManager.registerConverter(Player.class, (argument) -> Optional.ofNullable(Bukkit.getPlayerExact(argument)))
```
{% endtab %}

{% tab title="Kotlin" %}
```kotlin
val commandManager = ...;
commandManager.registerConverter(Player::class.java, (argument) -> Optional.ofNullable(Bukkit.getPlayerExact(argument)))
```
{% endtab %}
{% endtabs %}

To signal to Enchanted that the argument is invalid you can return an [empty optional](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html#empty--) or throw an [`ArgumentParseException`](https://github.com/JailGens/enchanted/blob/main/enchanted-api/src/main/java/net/jailgens/enchanted/ArgumentParseException.java) to provide a custom message.

### Default Converters

Every Enchanted implementation provides at least the following converters by default:

| Type                            | Description                                            |
| ------------------------------- | ------------------------------------------------------ |
| `java.lang.String`              | Returns the input                                      |
| `int`, `java.lang.Integer`      | Same as `Integer.valueOf()`                            |
| `double`, `java.lang.Double`    | Same as `Double.valueOf()`                             |
| `float`, `java.lang.Float`      | Same as `Float.valueOf()`                              |
| `boolean`, `java.lang.Boolean`  | Same as `Boolean.valueOf()`                            |
| `long`, `java.lang.Long`        | Same as `Long.valueOf()`                               |
| `short`, `java.lang.Short`      | Same as `Short.valueOf()`                              |
| `byte`, `java.lang.Byte`        | Same as `Byte.valueOf()`                               |
| `char`, `java.lang.Character`   | If there is only character in the input, it returns it |
| `java.util.concurrent.TimeUnit` |                                                        |
| `java.time.Month`               |                                                        |
| `java.time.DayOfWeek`           |                                                        |
| `java.util.UUID`                | Same as `UUID.fromString()`                            |
| `java.util.regex.Pattern`       | Same as `Pattern.compile()`                            |
| `java.awt.Color`                | Same as `Color.decode()`                               |

### Paper Converters

The Paper platform offers a ton of additional converters:

| Type                                                      | Description                                                                   |
| --------------------------------------------------------- | ----------------------------------------------------------------------------- |
| `org.bukkit.Art`                                          |                                                                               |
| `org.bukkit.ChatColor`                                    |                                                                               |
| `org.bukkit.CoalType`                                     |                                                                               |
| `org.bukkit.CropState`                                    |                                                                               |
| `org.bukkit.Difficulty`                                   |                                                                               |
| `org.bukkit.Effect`                                       |                                                                               |
| `org.bukkit.EntityEffect`                                 |                                                                               |
| `org.bukkit.Fluid`                                        |                                                                               |
| `org.bukkit.FireworkEffect.Type`                          |                                                                               |
| `org.bukkit.Instrument`                                   |                                                                               |
| `org.bukkit.Note.Tone`                                    |                                                                               |
| `org.bukkit.Particle`                                     |                                                                               |
| `org.bukkit.PortalType`                                   |                                                                               |
| `org.bukkit.SoundCategory`                                |                                                                               |
| `org.bukkit.Statistic`                                    |                                                                               |
| `org.bukkit.Statistic.Type`                               |                                                                               |
| `org.bukkit.TreeSpecies`                                  |                                                                               |
| `org.bukkit.WhetherType`                                  |                                                                               |
| `org.bukkit.Environment`                                  |                                                                               |
| `org.bukkit.WorldType`                                    |                                                                               |
| `org.bukkit.potion.PotionType`                            |                                                                               |
| `org.bukkit.permission.PermissionDefault`                 |                                                                               |
| `org.bukkit.loot.LootTables`                              |                                                                               |
| `org.bukkit.inventory.MainHand`                           |                                                                               |
| `org.bukkit.inventory.ItemFlag`                           |                                                                               |
| `org.bukkit.inventory.InventoryType`                      |                                                                               |
| `io.papermc.paper.world.MoonPhase`                        |                                                                               |
| `io.papermc.paper.inventory.ItemRarity`                   |                                                                               |
| `io.papermc.paper.inventory.EnchantmentRarity`            |                                                                               |
| `com.destroystokyo.paper.entity.ai.GoalType`              |                                                                               |
| `com.destroystokyo.paper.block.TargetBlockInfo`           |                                                                               |
| `com.destroystokyo.paper.ClientOptionl.ChatVisibility`    |                                                                               |
| `net.kyori.adventure.text.format.TextDecoration`          |                                                                               |
| `net.kyori.adventure.bossbar.BossBar.Color`               |                                                                               |
| `net.kyori.adventure.bossbar.BossBar.Flag`                |                                                                               |
| `net.kyori.adventure.bossbar.BossBar.Overlay`             |                                                                               |
| `net.kyori.adventure.sound.Sound.Source`                  |                                                                               |
| `org.bukkit.NamespacedKey`, `net.kyori.adventure.key.Key` | Same as `NamespacedKey.fromString`                                            |
| `net.kyori.adventure.text.Component`                      | Same as `LegacyComponentSerializer.legacyAmpersand.deserialize()`             |
| `net.kyori.adventure.text.format.TextColor`               | Same as `TextColor.fromHexString()`                                           |
| `org.bukkit.Sound`                                        | Gets a sound by namespaced key                                                |
| `org.bukkit.Material`                                     | Gets a material by namespaced key                                             |
| `org.bukkit.potion.PotionEffectType`                      |                                                                               |
| `org.bukkit.StructureType`                                |                                                                               |
| `org.bukkit.GameMode`                                     | Enum value + by id + by shorthand (s, c, a, sp)                               |
| `org.bukkit.entity.Player`                                | Gets a player by name (case insensitive)                                      |
| `org.bukkit.command.CommandSender`                        | Gets a player by name or "console" to get the console sender                  |
| `net.kyori.adventure.audience.Audience`                   | Gets a player by name, "console" to get the console or "\*" to get the server |
| `org.bukkit.plugin.Plugin`                                | Gets a plugin by name                                                         |
| `org.bukkit.permission.Permission`                        | Gets a permission by name                                                     |
| `org.bukkit.datapack.Datapack`                            | Gets a datapack by name                                                       |
| `org.bukkit.World`                                        | Gets a world by namespaced key or id                                          |
