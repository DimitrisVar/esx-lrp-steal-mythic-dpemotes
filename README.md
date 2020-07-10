# esx-lrp-steal-mythic-dpemotes

Due to some issues with [Progress Bar 1.0](https://github.com/EthanPeacock/progressBars/releases/tag/1.0) and an issue where the script didn't recogrize the ped's handsup animation, I did some quick changes to the code of [LRP-Steal](https://github.com/karenciita/LRP-Steal) publiced by [karenciita](https://github.com/karenciita), so that it works with [dpEmotes](https://github.com/andristum/dpemotes) and [mythic progressbar](https://github.com/XxFri3ndlyxX/mythic_progressbar).

## Requirements
  1. [dpEmotes](https://github.com/andristum/dpemotes)
  2. [mythic progressbar](https://github.com/XxFri3ndlyxX/mythic_progressbar)

## Installation
  1. Clone/Download this repository.
  2. Rename the folder to ```LRP-Steal```.
  3. Add the folder to your ```[esx]``` resources.
  4. Add resource to your server.cfg using ```start LRP-Steal```.


## The quick changes I made:  
Replaced under ```LRP-Steal > client.lua``` ***line 62:***
  ```
   if IsEntityPlayingAnim(searchPlayerPed, 'random@mugging3', 'handsup_standing_base', 3) or IsEntityDead(searchPlayerPed) or GetEntityHealth(searchPlayerPed) <= 0 then
  ```
  ***with:***
  ``` 
  if IsEntityPlayingAnim(searchPlayerPed, 'missminuteman_1ig_2', 'handsup_base', 3) or IsEntityDead(searchPlayerPed) or GetEntityHealth(searchPlayerPed) <= 0 then
  ``` 

Replaced ```RegisterNetEvent('robo:doarrested')``` under ```LRP-Steal > client.lua``` ***line 109***:

```
RegisterNetEvent('robo:doarrested')
AddEventHandler('robo:doarrested', function()
	local target, distance = ESX.Game.GetClosestPlayer()
	Citizen.Wait(250)
	loadanimdict('combat@aim_variations@arrest')
	TaskPlayAnim(GetPlayerPed(-1), 'combat@aim_variations@arrest', 'cop_med_arrest_01', 8.0, -8,3750, 2, 0, 0, 0, 0)
	exports['progressBars']:startUI(3500, " Buscando Objetos ")	
	Citizen.Wait(3000)
	OpenBodySearchMenu(target)
end) 
```
***with:***
```
RegisterNetEvent('robo:doarrested')
AddEventHandler('robo:doarrested', function()
	local target, distance = ESX.Game.GetClosestPlayer()
	Citizen.Wait(250)
	loadanimdict('combat@aim_variations@arrest')
	TaskPlayAnim(GetPlayerPed(-1), 'combat@aim_variations@arrest', 'cop_med_arrest_01', 8.0, -8,3750, 2, 0, 0, 0, 0)
	
	TriggerEvent("mythic_progbar:client:progress", {
		name = "unique_action_name",
		duration = 3500,
		label = "Robbing",
		useWhileDead = false,
		canCancel = true,
		controlDisables = {
			disableMovement = true,
			disableCarMovement = true,
			disableMouse = false,
			disableCombat = true,
		},
		animation = {
			animDict = "combat@aim_variations@arrest",
			anim = "cop_med_arrest_01",
		}
	}, function(status)
		if not status then
			OpenBodySearchMenu(target)
		end
	end)
end) 
```
