---
description: >-
  This page lists all the global variables that you can use with localscripts in
  the script builder.
---

# Variables

{% hint style="info" %}
Any global variable that are not listed here that is otherwise normally a part of Roblox should keep their vanilla behavior.
{% endhint %}

## [\_G](https://create.roblox.com/docs/reference/engine/globals/LuaGlobals#\_G)

The sandbox sets [\_G](https://create.roblox.com/docs/reference/engine/globals/LuaGlobals#\_G) to a table that already has a set metatable, and is only shared between sandboxed scripts.

## [shared](https://create.roblox.com/docs/reference/engine/globals/RobloxGlobals#shared)

The sandbox sets [shared](https://create.roblox.com/docs/reference/engine/globals/RobloxGlobals#shared) to a table that already has a set metatable, and is only shared between sandboxed scripts.

## [\_VERSION](https://create.roblox.com/docs/reference/engine/globals/LuaGlobals#\_VERSION)

On localscripts this is set to "Lua 5.1", because they only have Lua 5.1 syntax support. This doesn't mean they can't use newer Roblox features such as [Signal:Once()](https://create.roblox.com/docs/reference/engine/datatypes/RBXScriptSignal#Once).

## owner

A refrence to the [Player](https://create.roblox.com/docs/reference/engine/classes/Player) running the script. Same as [LocalPlayer](https://create.roblox.com/docs/reference/engine/classes/Players#LocalPlayer).

<details>

<summary>Examples</summary>

Sets the walkspeed of the player running the script to 32.

<pre class="language-lua"><code class="lang-lua"><strong>local Character = owner.Character
</strong>if Character then
    local Humanoid = Character:FindFirstChildOfClass("Humanoid")
    if Humanoid then
        Humanoid.WalkSpeed = 32
    end
end
</code></pre>

Kills the player running the script with :BreakJoints() if they have a character.

```lua
local Character = owner.Character
if Character then
    Character:BreakJoints()
end
```

</details>
