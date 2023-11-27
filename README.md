# Metakidia Inventory
Metakidia Inventory
## ProfileService
Save player's data (Inventory data, Cash, Gems, Avatar, Playtime ... )
It is not used by Template. But Save the data in InventoryService through Bindable events.

## InventoryService
### Remote Function
- GetInventory(player, tab: string, tabs: table, option: string?) : Return player inventory data.
	- Return Type : table
	- tab : "Back", "Character", "Consumable", "Food", "LeftShoulder", "Material", "Others","RightShoulder", "tool", "tools"
	- tabs: slot table
	- option : "All" or nil
### Remote Event
- AvatarChangedRemoteEvent : Not used in Inventory Service.
- CloseInventory(player: Player) : Send a close message to the Inventory UI.
- CommandOutput(player: Player, text: string) : Displays a (text: string) in the chat window.
- HandleEquip(player: Player,tool: Tool,state: BoolValue) : Equip or Unequip Tool.
	- true : Equip Tool
	- false : Uneuip Tool
- RequestClothing(player: Player,item: string,switch: string,currslot: string) : Destory clothes or tools in Inventory UI
	- items : item name
	- switch : item type, "Clothing" or "Tool"
	- currslot : target slot item
- SendClothing(player: Player, item: string, back: string, leftShoulder: string, rightShoulder: string) : 
	- send Equip message in Inventory UI
- SendDrop(player: Player,itemname: string, RunOnCommand: boolean?) : Drop item from player's inventory
- SendEat(player: Player,item: string) : Eat Item(Type : Food)
	- Only Magic Food type items work.
- SendInv(player: Player,maxW: IntValue,currweight: IntValue, tab: string): Displays the items in the Inventory UI.
	- The weight is currently unlimited, so it doesn't matter if you put any numbers in.
- SendItem(player: Player, itemName: string, droppedItem: Instance?, amount: IntValue?) : Give the player an item.
```lua
	SendItem:Fire(player, "Apple",nil,4) -- give 4 Apple to player
	SendItem:Fire(player, "Apple",nil) -- give 1 Apple to player
```
- SendWeapons(player: Player,item: string) : Equip item(type : Tool)
- UpdateClient(player: Player, tab: string) : Update the information in the target tab.
- UpdatePrompt(player: Player): Not used in Inventory Service.
### Bindable Event
- AvatarChangeBindableEvent : Not used in Inventory Service.
- SendItemServer(player: Player, itemName: string, droppedItem: Instance?, amount: IntValue?) : Give the player an item.
### Bindable Function
- GetInventoryServer
	- Return player inventory data
		- Return Type : table
		- tab : "Back", "Character", "Consumable", "Food", "LeftShoulder", "Material", "Others","RightShoulder", "tool", "tools"
		- tabs: slot table
		- option : "All" or nil

### Inventory Service Modules
##### Crafting
This Module Script used for the Crafting Service.
Save the recipe for the item.
``` lua
	["Torch"] = {
		"Log", -- Item name from Items Module Script
		"Coal",
		}
	}
```
##### Items
List of items used by Inventory Service.
``` lua
	["Apple"] = {
		Name = "Apple",
		KorName = "사과", 
		Amount = 1, 
		Desc = "A healthy apple",
		Type = "Consumable", 
		-- Item Type ("Back", "Character", "Consumable", "Food", "LeftShoulder", "Material", "Others","RightShoulder", "tool")
		Weight =  5,
		Selling = 15, 
		Picture = 137510460,
		Dropable = true,
	};
```

##### MagicItemList
The module save the effects of the Magic Item.
```lua
	-- If the number is -1, no effect will be applied. If 0 is given, the corresponding value will be zero, so be careful.
	-- Default jumping force 50, speed 16, humanoid attributes are all 1 except BodyTypeScale = 0
	-- For the 2023.08.24 Scale option, we recommend that you disable it due to abnormal movement due to current physical engine bugs.
	["MagicBanana"] = { -- (name is from Items Module)
		Name = "MagicBanana",
		JumpPower = 100,          -- Set the player's jumping power.
		Speed = 100,              -- Set the player's Speed power.
		WidthScale = -1,           -- Set the player's Humanoid WidthScale.
		HeightScale = -1,	      -- Set the player's Humanoid HeightScale.
		DepthScale = -1,           -- Set the player's Humanoid DepthScale.
		BodyProportionScale = 0,  -- Set the player's Humanoid BodyProportionScale.
		BodyTypeScale = -1,       -- Set the player's Humanoid BodyTypeScale.
		HeadScale = -1,           -- Set the player's Humanoid HeadScalee.
		Heal = -1,                -- Restoring the player's HP.
		Damage = -1,              -- Damaging the player's HP.
		Effect = "RainbowEffect", -- Set the player's Effect (name is from Items Module)
		Duration = 15,            -- The duration of the item
		Scale = -1				 -- Do not used. set defult -1
	};
```
##### Prices
This List is items price in Items Module
``` lua
Prices.Apple = {
	cashPrice = 3000,
	crystalGemPrice = 0,
	cubicGemPrice = 10,
	diamondGemPrice = 20,
	rubyGemPrice = 30
}
```

### How to make new item
1. Move the model what you want to use to ServerStorage - Inventory Service - Items.
2. Fill out the Items and Pirces script in accordance with the form.
	2-1. MagicItems module scripts are created only for items of food type.
3. Add Proximity Prompt object under Item.  
	3-1. ActionText: Create 'Purchase'  
	3-2. ObjectText: Create a price for that item

There are two frequently used events for Inventory Service: SendItem and SendItemServer.

### How to get Cash and Gems
![image](https://github.com/bgloh/metakidia_inventory/blob/main/Images/how_to_get_cash_and_gems.png)
Interact cube to 'E'
