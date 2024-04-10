---
description: >-
  This page lists all the global functions that you can use with localscripts in
  the script builder.
---

# Functions

{% hint style="info" %}
Any global function that are not listed here that is otherwise normally a part of Roblox should keep their vanilla behavior.
{% endhint %}

## [print](https://create.roblox.com/docs/reference/engine/globals/LuaGlobals#print)

```lua
print(...: any): void
```

Prints into the script builder output instead of the Roblox one. If the format printing setting is enabled then it will format the message with [repr](https://github.com/Ozzypig/repr).

## [warn](https://create.roblox.com/docs/reference/engine/globals/RobloxGlobals#warn)

```lua
warn(...: any): void
```

Warns into the script builder output instead of the Roblox one. If the format printing setting is enabled then it will format the message with [repr](https://github.com/Ozzypig/repr).

## [error](https://create.roblox.com/docs/reference/engine/globals/LuaGlobals#error)

```lua
error(message: any, level: number?): void
```

Throws an error, if the format printing setting is enabled then it will format the message with [repr](https://github.com/Ozzypig/repr). Script errors are automatically caught by the script builder and outputted into your output.

## printf

```lua
printf(...: any): void
```

Prints into the script builder output with richtext enabled, ignoring the format printing setting. Useful if you want to print information to the user into the output.

## warnf

```lua
warnf(...: any): void
```

Warns into the script builder output with richtext enabled, ignoring the format printing setting. Useful if you want to warn information to the user into the output.

## [printidentity](https://create.roblox.com/docs/reference/engine/globals/RobloxGlobals#printidentity)

```lua
printidentity(prefix: string?): void
```

Exactly like [printidentity](https://create.roblox.com/docs/reference/engine/globals/RobloxGlobals#printidentity) on Roblox but will always print identity 2, and outputs into the script builder output.

{% hint style="warning" %}
This is a deprecated Roblox function, and should not be used in new work. It's just added for compatibility.
{% endhint %}

## [require](https://create.roblox.com/docs/reference/engine/globals/RobloxGlobals#require)

```lua
require(): never
```

This function is disabled on client.

## [loadstring](https://create.roblox.com/docs/reference/engine/globals/LuaGlobals#loadstring)

```lua
loadstring(contents: string, chunkname: string?): (((...) -> any)?, string?)
```

A reimplementation of Roblox's [loadstring](https://create.roblox.com/docs/reference/engine/globals/LuaGlobals#loadstring) function on client.

## LoadLibrary

```lua
LoadLibrary(library: string): any
```

A reimplementation of Roblox's LoadLibrary feature which was [removed](https://devforum.roblox.com/t/loadlibrary-is-going-to-be-removed-on-february-3rd/382516).

<details>

<summary>List of libraries</summary>

* RbxGui
* RbxStamper
* RbxUtility

</details>

{% hint style="danger" %}
This is a removed Roblox function, and should not be used in new work. It's just added for compatibility.
{% endhint %}

## NewScript

```lua
NewScript(source: string, parent: Instance?, ...any): Script
```

Creates and returns a new [Script](https://create.roblox.com/docs/reference/engine/classes/Script) with the specified source under the specified parent, with optional run arguments.

{% hint style="info" %}
If no parent argument is specified then it will default to [Workspace](https://create.roblox.com/docs/reference/engine/classes/Workspace).
{% endhint %}

<details>

<summary>Examples</summary>

Creates a new script in workspace that prints "Hello World! I'm in " followed by its full name.

```lua
NewScript([[
printf("Hello World! I'm in", script:GetFullName())
]])
```

Creates a new script in workspace, and prints the 3 values it's given.

```lua
NewScript([[
local var1, var2, var3 = ...
printf("var1:", var1)
printf("var2:", var2)
printf("var3:", var3)
]], workspace, "Hello I'm a string", Instance.new("Part", workspace), {"Tables work too!"})
```

</details>

## NS

Alias for [NewScript](functions.md#newscript).

## NewLocalScript

```lua
NewLocalScript(source: string, parent: Instance?, ...any): LocalScript
```

Creates and returns a new [LocalScript](https://create.roblox.com/docs/reference/engine/classes/LocalScript) with the specified source under the specified parent, with optional run arguments.

{% hint style="info" %}
If no parent argument is specified then it will default to the script [owner](../script/variables.md#owner)'s [PlayerGui](https://create.roblox.com/docs/reference/engine/classes/PlayerGui).
{% endhint %}

<details>

<summary>Examples</summary>

Creates a new local script in your player gui that prints "Hello World! I'm in " follwed by its full name.

```lua
NewLocalScript([[
printf("Hello World! I'm in", script:GetFullName())
]])
```

Creates a new local script in your PlayerGui, and prints the 3 values it's given.

```lua
NewLocalScript([[
local var1, var2, var3 = ...
printf("var1:", var1)
printf("var2:", var2)
printf("var3:", var3)
]], nil, "Hello I'm a string", Instance.new("Part", workspace), {"Tables work too!"})
-- Pass nil as the parent if you want it to go to the default location.
```

Creates a new local script in your player gui to send your key inputs to the server.

```lua
local Remote = Instance.new("RemoteEvent")
Remote.Name = "Input"
Remote.Parent = script

NewLocalScript([[
local UserInputService = game:GetService("UserInputService")
local Remote = ...

UserInputService.InputBegan:Connect(function(Input)
    if UserInputService:GetFocusedTextBox() ~= nil then -- Don't send if typing.
        return
    end
    
    if Input.UserInputType ~= Enum.UserInputType.Keyboard then
        return
    end
    
    Remote:FireServer(Input.KeyCode)
end)
]], nil, Remote)
-- Parents the LocalScript to the default location and gives it the RemoteEvent.

Remote.OnServerEvent:Connect(function(Player, KeyCode)
    if Player ~= owner then -- We don't want other players spoofing!
        return
    end
    
    if typeof(KeyCode) ~= "EnumItem" then
        return
    end
    
    print(KeyCode.Name) -- Print the keycode's name.
end)
```

</details>

## NLS

Alias for [NewLocalScript](functions.md#newlocalscript).
