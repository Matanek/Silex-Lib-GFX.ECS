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

## Temporarily disable an entity

`world.enable(entity, false)` preserves the entity identity and components,
but excludes it from every `ECS.Query` and from `world.entities()`. The entity
remains alive: `is_alive`, `has`, `get`, `update`, `insert`, `remove` and
`destroy` stay available. Editing its components does not enable it again.
`world.enable(entity, true)` returns it to queries with the same identifier.

`world.is_enabled(entity)` reads its state; a destroyed entity returns `false`.
`world.entities(include_disabled:true)` inspects all living entities. Newly
spawned entities are enabled by default.

Activation is a structural change: do not change it directly during query
iteration. Use `commands.enable(entity, state)` in a system to apply it at the
usual command flush point, in the same deterministic order as other commands.
A command targeting an already destroyed entity is ignored.

## Query and defer changes

An `ECS.Query` exposes the components it reads and changes in its signature.
`ECS.Commands` defers structural changes — creating or destroying entities and
adding or removing components — so an active iteration remains stable.

## Integrate a GFX application

`ECS.Plugin` installs the world, deferred commands, and system integration into
`GFX.Application`. The package also contributes the capability to GFX's open
catalogs as `Plugins.ECS`, `Resources.World`, and `Resources.Commands`.
