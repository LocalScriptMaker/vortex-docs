---
title: Humanoid
description: Represents the humanoid controller in a Player Character.
---

## Summary

<details>
<summary><b>Properties</b></summary>
Properties of `Humanoid`.
<br><br>

* [Health](#health): `number`
* [MaxHealth](#maxhealth): `number`

</details>

<details>
<summary><b>Methods</b></summary>
Signals of `Humanoid`.
<br><br>

* [IsDead](#isdead): `boolean`
* [GetState](#getstate): `Enum.HumanoidStateType`

</details>

<details>
<summary><b>Signals</b></summary>
Signals of `Humanoid`.
<br><br>

* [Changed: `Connection`](#changed)

</details>

## Overview

`Humanoid` provides character functionality.

The current player's humanoid is available in both contexts; `Script` and `LocalScript`.

```luau
-- Script

local player = game:GetService('Players'):GetPlayers()[1]
local character = player and player.Character or player.CharacterAdded:Wait()
local humanoid:Humanoid = character:WaitForChild('Humanoid')
```

```luau
-- LocalScript

local player = game:GetService('Players').LocalPlayer
local character = player and player.Character or player.CharacterAdded:Wait()
local humanoid:Humanoid = character:WaitForChild('Humanoid')
```

## Properties

### Health

> `Humanoid.Health: number`
>
> The current health value. If the `Humanoid` is dead, this property is continually set to 0.

### MaxHealth

> `Humanoid.MaxHealth: number`
>
> The maximum health value.

### WalkSpeed

> `Humanoid.WalkSpeed: number`
>
> Determines how quickly the `Humanoid` is able to walk, in studs per second.

### JumpPower

> `Humanoid.JumpPower: number`
>
> Determines how much upwards force is applied to the `Humanoid` when jumping.

### JumpHeight

> `Humanoid.JumpPower: number`
>
> Determines how much studs `Humanoid` can jump.

### AutoRotate

> `Humanoid.AutoRotate: boolean`
>
> Determines whether or not the `Humanoid` will automatically rotate to face in the direction they are moving.

### HipHeight

> `Humanoid.HipHeight: number`
>
> Determines the distance in studs off the ground the `HumanoidRootPart` should be.

### MoveDirection

> `Humanoid.MoveDirection: Vector3`
>
> Normalized Vector3 that describes the direction a `Humanoid` is walking in.

### Sit

> `Humanoid.Sit: boolean`
>
> Describes whether the `Humanoid` is currently sitting.

### SeatPart

> `Humanoid.SeatPart: BasePart`
>
> A part that a `Humanoid` is currently sitting in.

## Methods

### IsDead

> `Humanoid:IsDead(): boolean`
>
> Returns whether the `Humanoid` is dead.

### GetState

> `humanoid:GetState(): Enum.HumanoidStateType`
>
> Returns the Humanoid's current state.

### TakeDamage

> `Humanoid:TakeDamage()`
>
> Lowers the `Health` property by the given amount if it is not protected by a `ForceField`

## Signals

### Died

> [`Signal`](../datatypes/signal.md)
>
> `humanoid.Died`
>
> Signals that the Humanoid died.

<br/>

### HealthChanged

> [`Signal`](../datatypes/signal.md)
>
> `humanoid.HealthChanged`
>
> Signals when the Humanoid health changes.

### StateChanged

> [`Signal`](../datatypes/signal.md)
>
> `humanoid.StateChanged`
>
> Signals when the Humanoid state changes.

### Jumping

> [`Signal`](../datatypes/signal.md)
>
> `humanoid.Jumping`
>
> Signals when the Humanoid is jumping.

### FreeFalling

> [`Signal`](../datatypes/signal.md)
>
> `humanoid.FreeFalling`
>
> Signals when the Humanoid enters or leaves the freefall state.

### Running

> [`Signal`](../datatypes/signal.md)
>
> `humanoid.Running`
>
> Signals when the Humanoid enters or leaves the running state.

#### Parameters

- `health`: `Number` — the updated health value.

## Testing Notes

These observations are from Vortex Studio 0.3.4 and may differ in later
releases.

The observed character Humanoid was a specialized table with `ClassName` and
`Name` both reporting `"Humanoid"`, and `Health` and `MaxHealth` both
reporting `100`. `IsDead()` was exposed and returned `false`. `Died` and
`HealthChanged` exposed `Signal:Connect`.

Assigning `Health` or `MaxHealth` is rejected in both a `LocalScript` and a
confirmed server `Script` (`RunService:IsServer()` returned `true`). The error
still says that the value is read-only in a LocalScript and server-authoritative,
so its wording does not match the observed server behavior. Server code can now
enumerate a `Player`, read its `Character`, and obtain this `Humanoid`, but that
projection does not currently grant Health-write authority.

`WalkSpeed`, `JumpPower`, `JumpHeight`, and `UseJumpPower` read as `nil` on the
tested server-visible Humanoid projection, so their write behavior remains
unknown. `Died` and `HealthChanged` are exposed Signal references; their
delivery remains unconfirmed.

The confirmed server method surface contains `IsDead()` only. The Roblox-style
methods `TakeDamage`, state/movement control, animation, and accessory methods
all read as `nil`. Of the tested Roblox-style signals, only `Died` and
`HealthChanged` are connectable; `Jumping`, `FreeFalling`, `Running`,
`StateChanged`, `MoveToFinished`, `PlatformStanding`, `Ragdoll`, and `Seated`
all read as `nil`.

The specialized Humanoid has an opaque `__index` and `__newindex` metatable;
`IsA` was not exposed on the value itself.
