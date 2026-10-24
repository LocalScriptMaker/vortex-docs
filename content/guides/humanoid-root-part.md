---
title: HumanoidRootPart
description: The root Part of a visible Player Character.
---

`HumanoidRootPart` is the root object exposed by a visible player's Character
Model. It reports `ClassName == "Part"`, but it is a narrow character-state
projection rather than a normal [`Part`](../reference/classes/part.md) instance. It is not a
separately constructible Vortex class; access it from the character that
Vortex exposes:

```luau
-- LocalScript
local player = game:GetService('Players').LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local rootPart = character:WaitForChild('HumanoidRootPart')
```

A server `Script` can read the same projection from the first visible player:

```luau
local player = game:GetService('Players'):GetPlayers()[1]
local character = player and player.Character or player.CharacterAdded:Wait()
local rootPart = character:WaitForChild('HumanoidRootPart')
```

## Confirmed surface

`IsA` is the only tested method currently exposed. It returns `true` for
`Part`, `BasePart`, and `Instance`, despite the narrow projection
surface described below.

The tested ordinary Part fields are unavailable: `Anchored`, `CanCollide`,
`CastShadow`, `CFrame`, `Color`, `Material`, `Orientation`, `Rotation`,
`Transparency`, and the tested velocity, mass, collision-query, touch, and
root-priority fields all read as `nil`.

The tested Instance, Part, and physics methods including hierarchy, clone,
destroy, attributes, property signals, joints, impulses, mass, and network
ownership methods also read as `nil`. `Changed`, `Touched`, `TouchEnded`, and
the tested hierarchy/lifecycle signals read as `nil`; none is connectable.

## Character-state authority

Writing `HumanoidRootPart.Size`, or `HumanoidRootPart.Orientation` is rejected in both a `LocalScript` and a
confirmed server `Script`. The current error calls the property read-only in a
LocalScript and says character state is server-authoritative, even when
`RunService:IsServer()` reports `true`.

Consequently, do not use the visible character root part as a, resize, or movement-control target in the current runtime. This is a
restriction of the live Character projection; it does not describe ordinary
script-created Parts, whose tested transform properties remain writable.

## Testing Notes

These observations were verified in Vortex Studio 0.8.1; untested members may
still exist in a later runtime release.
