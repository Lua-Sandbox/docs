---
description: >-
  This page lists all the global functions that you can use with scripts in the
  script builder.
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
require(target: ModuleScript | number): any
```

Exactly the same as Roblox's [require](https://create.roblox.com/docs/reference/engine/globals/RobloxGlobals#require) function, but is blocked from people in place 1 (outside of VIP servers) that don't have place 1 require permissions. This is because this function can very easily escape the sandbox.

{% hint style="info" %}
If you want to use mesh parts or not have to use model to script in place 1, then check out [LoadAssets](functions.md#loadassets).
{% endhint %}

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

## LoadAssets

```lua
type AssetLoader = {
    Get: (asset: string) -> Instance,
    Exists: (asset: string) -> boolean,
    GetNames: () -> {string},
    GetArray: () -> {Instance},
    GetDictionary: () -> {[string]: Instance}
}

LoadAssets(id: number): AssetLoader
```

Has almost the exact same functionality as [require](functions.md#require). But can only have asset id's as the target, and does some security checks to prevent people from running untrusted code outside the sandbox. This is used to load user made assets into the game (even meshparts).

{% hint style="info" %}
Credits to [Raymond](https://roblox.com/users/1714750665) for the idea.
{% endhint %}

<details>

<summary>How to use</summary>

If you would like to use LoadAssets then first download the .rbxm file [here](https://lua-sandbox.ewdev.net/public/assetLoader.rbxm) and then open it in studio. After that you can insert in all assets that you want to be able to use ingame. And then publish the ModuleScript (remember to enable "Distribute on Marketplace"), then copy the asset id.

(Remember that modifying the ModuleScript's source or adding any blocked instances will cause it to be blocked in-game.)

After that you can call LoadAssets() with the asset id you copied and then use :Get() with the name of the asset you want.

</details>

<details>

<summary>Blocked instances</summary>

* Script
* LocalScript
* ModuleScript
* CoreScript
* PackageLink
* AdPortal
* FloorWire
* SkateboardPlatform
* DynamicImage (might change)

</details>

<details>

<summary>Examples</summary>

Loads a noob mesh into the game near spawn.

```lua
local Assets = LoadAssets(13220242943)
local NoobMesh = Assets:Get("Noob")
NoobMesh.Parent = workspace
```

Loads a mesh of Roblox and Builderman into the game near the spawn.

```lua
local Assets = LoadAssets(13242794521)
for _, Asset in ipairs(Assets:GetArray()) do
    Asset.Parent = workspace
end
```

Prints a list of drinks to the player's output, and allows them to get them by saying ",drink " followed by the drink name.

```lua
local Assets = LoadAssets(13242863830)
local function GetList()
	printf("Say ',drinklist' to see this again.")
	
	printf("Say ',drink ' followed by one of the names listed below to get it:")
	for _, Name in ipairs(Assets:GetNames()) do
		print(Name)
	end
end

local function GetDrink(Name)
	if not Assets:Exists(Name) then
		return warnf("Invalid drink name, say ',drinklist' to get a list of drinks.")
	end
	
	local Tool = Assets:Get(Name)	
	local Handle = Tool:WaitForChild("Handle")
	Tool.Parent = owner:FindFirstChildOfClass("Backpack")
	
	Tool.Equipped:Connect(function()
		Handle.OpenSound:Play()
	end)
	
	local Enabled = true
	Tool.Activated:Connect(function()
		if not Enabled then
			return
		end
		
		Enabled = false
		
		Tool.GripForward = Vector3.new(0,-.759,-.651)
		Tool.GripPos = Vector3.new(1.5,-.5,.3)
		Tool.GripRight = Vector3.new(1,0,0)
		Tool.GripUp = Vector3.new(0,.651,-.759)
		
		Handle.DrinkSound:Play()
		
		task.wait(3)
		
		Tool.GripForward = Vector3.new(-.976,0,-0.217)
		Tool.GripPos = Vector3.new(0.03,0,0)
		Tool.GripRight = Vector3.new(.217,0,-.976)
		Tool.GripUp = Vector3.new(0,1,0)

		Enabled = true
	end)
end

owner.Chatted:Connect(function(message)
	if string.sub(message, 1, 3) == "/e " then
		message = string.sub(message, 4)
	end
	
	if string.sub(message, 1, 10) == ",drinklist" then
		GetList()
	elseif string.sub(message, 1, 7) == ",drink " then
		GetDrink(string.sub(message, 8))
	end
end)

GetList()
```

</details>

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
If no parent argument is specified then it will default to the script [owner](variables.md#owner)'s [PlayerGui](https://create.roblox.com/docs/reference/engine/classes/PlayerGui).
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
