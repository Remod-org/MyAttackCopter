# MyAttackCopter
A simple plugin to spawn an attack copter for yourself. Limits to one attack copter per player with optional cooldown (using permission).

![](https://www.remod.org/sites/default/files/inline-images/mincopter.jpg)

Uses: NoEscape (optional)

## Permissions

- `myattackcopter.spawn` -- Allows player to spawn an attack copter (/myattack)
- `myattackcopter.fetch`    -- Allows player to use /gattack retrieve their attack copter
- `myattackcopter.where`    -- Allows player to use /wattack to locate their attack copter (NEW!)
- `myattackcopter.admin`  -- Allows an admin to run console commands (may change)
- `myattackcopter.cooldown` -- Adds a cooldown to player
- `myattackcopter.unlimited` -- Player can fly without fuel usage (will need to add at least 1 LGF unless "Allow unlimited to use fuel tank" is set to false)
- `myattackcopter.canhover` -- Player can enable or disable hover mode

## Chat Commands

- `/myheli` -- Spawn an attack copter
- `/noheli` -- Despawn attack copter
- `/wheli`  -- Find attack copter
- `/gheli`  -- Get/fetch attack copter
- `/hheli`  -- Enable/disable hovering

## Console Commands

- `spawnattackcopter <player ID>`
- `killattackcopter <player ID>`

## For Developers

```csharp
(void) SpawnMyAttackcopter (BasePlayer player)
(void) KillMyAttackcopterPlease (BasePlayer player)
no return value;
```

## Configuration

```json
{
  "Global": {
    "allowWhenBlocked": false,
    "useCooldown": true,
    "copterDecay": false,
    "allowDamage": true,
    "killOnSleep": false,
    "allowFuelIfUnlimited": false,
    "allowDriverDismountWhileFlying": true,
    "allowPassengerDismountWhileFlying": true,
    "stdFuelConsumption": 0.25,
    "cooldownmin": 60.0,
    "mindistance": 0.0,
    "gattackdistance": 0.0,
    "minDismountHeight": 7.0,
    "startingFuel": 0.0,
    "Prefix": "[My AttackCopter] :",
    "TimedHover": false,
    "DisableHoverOnDismount": true,
    "EnableRotationOnHover": true,
    "PassengerCanToggleHover": false,
    "HoverWithoutEngine": false,
    "UseFuelOnHover": true,
    "HoverDuration": 60.0,
    "UseKeystrokeForHover": false,
    "HoverKey": 134217728
  },
  "VIPSettings": {
    "myattackcopter.viplevel1": {
      "unlimited": false,
      "canloot": true,
      "stdFuelConsumption": 0.15,
      "startingFuel": 20.0,
      "cooldownmin": 120.0,
      "mindistance": 0.0,
      "gattackdistance": 0.0
    }
  },
  "Version": {
    "Major": 0,
    "Minor": 4,
    "Patch": 7
  }
}
```

Global:

- `allowWhenBlocked` -- Set to true to allow player to use /myattack while building blocked
- `useCooldown` -- Enforce a cooldown for minutes between use of /myattack.
- `useNoEscape` -- Use the NoEscape plugin to check and prevent command use while "raid blocked" per that plugin
- `copterDecay` -- Enable decay
- `allowDamage` -- Enable/allow damage (old default)
- `killOnSleep` -- Kill the copter when the user leaves the server
- `allowFuelIfUnlimited` -- Allow unlimited permission users to add fuel anyway.
- `allowDriverDismountWhileFlying` -- Allow the driver to dismount while flying above minDismountHeight.
- `allowPassengerDismountWhileFlying` --  Allow passenger to dismount while flying above minDismountHeight.
- `stdFuelConsumption` -- Adjust fuel consumption per second from standard amount (0.25f)
- `cooldownmin` -- Minutes to wait between usage of /myattack
- `mindistance` -- Attackumum distance to copter for using /noattack
- `gattackdistance` -- Attackumum distance to copter for using /gattack
- `minDismountHeight` -- Attackumum height for dismount (for allow rules above)
- `startingFuel` -- How much fuel to start with for non-unlimited permission players (default 0)
- `Prefix` -- Prefix for chat messages (default [MyAttackCopter: ])
- `TimedHover` -- Use a timer to limit how long a copter can remain in hover mode
- `DisableHoverOnDismount` -- Disable hover if no players are seated
- `EnableRotationOnHover` -- Allow the driver to rotate the copter while hovering
- `PassengerCanToggleHover` -- Allow the passenger to toggle hovering
- `HoverWithoutEngine` -- Hover with engine stopped
- `UseFuelOnHover` -- Use fuel while hovering
- `HoverDuration` -- How long the copter will hover if TimedHover is true
- `UseKeystrokeForHover` -- Allow use of middle mouse button (by default) to toggle hovering
- `HoverKey` -- Set the key used for hover toggling (default is middle mouse button)

Set "Value in meters" for gattack or noattack to 0 to disable the requirement (default).

#### VIPSettings

For each level of VIP access you want, edit or create a copy of the default entry in the config:

```json
  "VIPSettings": {
    "myattackcopter.viplevel1": {
      "unlimited": false,
      "canloot": true,
      "stdFuelConsumption": 0.15,
      "startingFuel": 20.0,
      "cooldownmin": 120.0,
      "mindistance": 0.0,
      "gattackdistance": 0.0
    },
    "myattackcopter.viplevel2": {
      "unlimited": true,
      "canloot": false,
      "stdFuelConsumption": 0.0,
      "startingFuel": 20.0,
      "cooldownmin": 240.0,
      "mindistance": 0.0,
      "gattackdistance": 0.0
    }
  }
```

The setting above called myattackcopter.viplevel1 is the name of the permission to assign to give a user or group this access.

Below this are the things you can change vs. the default settings.  Overall, they should work the same as the default settings.

You can set the key for each value simply to viplevel1, maxattacks, etc.  The code will automatically add myattackcopter. to the beginning as is required.

### Notes on hovering

1. The player/owner must have the myattackcopter.hover permission.

2. Some code was borrowed from HelicopterHover but modified for our purposes.  Essentially, the parts that maintain height and control fuel usage are from that plugin.  See the plugin code for more details.

3. Fuel usage is still disabled if the player has the unlimited permission.

4. While hovering, if UseKeystrokeForHover is enabled, the driver can click the middle mouse button to toggle hovering on and off.

5. Also, if UseKeystrokeForHover is enabled, the BACK button (S) will become the stabilzation button while hovering.  This allows the player to automatically right the attackcopter.

6. If UseKeystrokeForHover is NOT enabled (false), then neither of these functions work.  To toggle hovering in that case, use the /hattack chat command.

7. If you want to use another key instead of MMB, you must use one that the game will actually send.  This is generally limited to WASD, Shift, Ctrl, and perhaps a few others.  Be careful not to interfere with other player motion commands, map, etc.

## Future Plans

* health workaround
* check console commands input/NRE
