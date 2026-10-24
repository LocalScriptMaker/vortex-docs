---
title: Character
description: The runtime Model projection associated with a Player.
---

# Summary

A Character is the live [`Model`](./model.md) exposed by a player's
[`Player.Character`](./player.md) field. It is not a separately constructible
Vortex class. Obtain it from the local player in a LocalScript or from a Player
returned by `Players:GetPlayers()` in a server Script.

```luau
-- LocalScript
local player = game:GetService('Players').LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
```

```luau
-- Script
local player = game:GetService('Players'):GetPlayers()[1]
local character = player and player.Character or player and player.CharacterAdded:Wait()
```

Supported properties, methods and signals for [`Model`](./model.md) references
