---
icon: material/cube
---

## :material-package-variant-closed: PlayerState

### :material-function-variant: **`#!typescript get(user: DefaultUserDeclaration): T`** { #markdown data-toc-label='GlobalState.get()' }

#### Parameters

`#!typescript user: DefaultUserDeclaration`

: The User you want to retrieve their state from.

#### Returns

`#!typescript T`

: Returns current player state of type `T`.

### :material-function-variant: **`#!typescript set(user: DefaultUserDeclaration, setter: T | (oldValue?: T) => T): void`** { #markdown data-toc-label='LocalState.set()' }

#### Parameters

`#!typescript user: DefaultUserDeclaration`

: The User to set their State to the value passed.

`#!typescript setter: T | (oldValue?: T) => T`

: A setter, which replicates the new value to the specified User.

!!! example "Example"

    ```typescript linenums="1"
        import Tina, { State, Users } from "@rbxts/tina";

        const StateTree = Tina.buildState({
            player: State.namespace({
                inventory: State.player<InventoryScheme>(),
            }),
        })

        StateTree.player.inventory.set(...);

        StateTree.player.inventory.get(Users.get(playerReference));
    ```
