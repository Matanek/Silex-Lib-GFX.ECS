# GFX.ECS

`GFX.ECS` provides stable entities, typed components, spawn recipes, queries,
and deferred structural commands. Concrete scene components remain owned by
their scene package.

[Lire cette documentation en français.](../FR/README.md)

## Install the package

```text
silex install GFX.ECS
```

GFX.ECS requires Silex 0.39.0 or newer.

## Create and modify an entity

An `EntityRecipe` groups initial components before creating the entity:

```sx
use GFX.ECS

struct Health { var value:int }

func main() {
    var world = ECS.World()
    let entity = world.spawn(ECS.EntityRecipe()..with(Health(value:100)))
    world.update<Health>(entity, func(health:&Health) {
        health.value -= 10
    })
}
```

The returned identifier remains stable until the entity is destroyed.
`World.update` makes the modified type explicit and limits the mutable borrow
to the provided function.

## Query and defer changes

An `ECS.Query` exposes the components it reads and changes in its signature.
`ECS.Commands` defers structural changes — creating or destroying entities and
adding or removing components — so an active iteration remains stable.

## Integrate a GFX application

`ECS.Plugin` installs the world, deferred commands, and system integration into
`GFX.Application`. The package also contributes the capability to GFX's open
catalogs as `Plugins.ECS`, `Resources.World`, and `Resources.Commands`.
