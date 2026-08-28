# GFX.ECS

`GFX.ECS` fournit des entités stables, des composants typés, des recettes de
création, des queries et des commandes structurelles différées. Les composants
concrets d’une scène restent la propriété de leur package de scène.

[Read this documentation in English.](../EN/README.md)

## Installer le package

```text
silex install GFX.ECS
```

GFX.ECS demande Silex 0.39.0 ou une version plus récente.

## Créer et modifier une entité

Une `EntityRecipe` regroupe les composants initiaux avant de créer l’entité :

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

L’identifiant renvoyé reste stable jusqu’à la destruction de l’entité.
`World.update` rend explicite le type modifié et limite l’emprunt mutable à la
fonction fournie.

## Interroger et différer les changements

Une `ECS.Query` rend visibles dans sa signature les composants qu’elle lit et
modifie. `ECS.Commands` diffère les changements structurels — création,
destruction, ajout ou retrait de composants — afin qu’une itération active
reste stable.

## Intégrer une application GFX

`ECS.Plugin` installe le monde, les commandes différées et l’intégration des
systèmes dans `GFX.Application`. Le package contribue aussi cette capacité aux
catalogues ouverts de GFX sous `Plugins.ECS`, `Resources.World` et
`Resources.Commands`.
