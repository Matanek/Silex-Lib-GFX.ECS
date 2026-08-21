# GFX.ECS

`GFX.ECS` provides stable entities, typed components, spawn recipes, queries,
and deferred structural commands. Concrete scene components remain owned by
their scene packages.

```text
silex install GFX.ECS
```

```silex
use GFX.ECS

struct Health { var value:int }

var world = ECS.World()
let entity = world.spawn(ECS.EntityRecipe()..with(Health(value:100)))
world.update<Health>(entity, func(health:&Health) { health.value -= 10 })
```

`ECS.Plugin` integrates the world and deferred commands with
`GFX.Application`. The package contributes the same capability as
`Plugins.ECS`, `Resources.World`, and `Resources.Commands` to GFX's open
catalogs.

See [Docs/README.md](Docs/README.md) for queries, commands, and application
integration.
