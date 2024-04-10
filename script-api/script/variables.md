---
description: >-
  This page lists all the global variables that you can use with scripts in the
  script builder.
---

# Variables

{% hint style="info" %}
Any global variable that are not listed here that is otherwise normally a part of Roblox should keep their vanilla behavior.
{% endhint %}

## [\_G](https://create.roblox.com/docs/reference/engine/globals/LuaGlobals#\_G)

The sandbox sets [\_G](https://create.roblox.com/docs/reference/engine/globals/LuaGlobals#\_G) to a table that already has a set metatable, and is only shared between sandboxed scripts.

## [shared](https://create.roblox.com/docs/reference/engine/globals/RobloxGlobals#shared)

The sandbox sets [shared](https://create.roblox.com/docs/reference/engine/globals/RobloxGlobals#shared) to a table that already has a set metatable, and is only shared between sandboxed scripts.

## owner

A refrence to the [Player](https://create.roblox.com/docs/reference/engine/classes/Player) running the script.

<details>

<summary>Examples</summary>

Sets the walkspeed of the player running the script to 32.

```lua
local Character = owner.Character
if Character then
    local Humanoid = Character:FindFirstChildOfClass("Humanoid")
    if Humanoid then
        Humanoid.WalkSpeed = 32
    end
end
```

Kills the player running the script with :BreakJoints() if they have a character.

```lua
local Character = owner.Character
if Character then
    Character:BreakJoints()
end
```

</details>
