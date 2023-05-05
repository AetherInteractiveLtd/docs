---
icon: material/cube
---

## :material-package-variant-closed: State

### :material-function-variant: **`#!typescript namespace(stateTree: object): object`** { #markdown data-toc-label='namespace()' }

Creates a namespace for organizing your state in a single place.

#### Parameters

`#!typescript stateTree: object`

: The state tree you want to store within the new namespace.

#### Returns

`#!typescript object`

: It returns the object passed to it for access later on.

!!! warning "Have in mind"

    It is important that you only store state objects within your State Tree, storing any other value may result in not intended behavior.

    !!! example "Example"

        ```typescript linenums="1"
            import { State } from "@rbxts/tina";

            const StateTree = State.namespace({
                myState: 0, // Not valid.
            });
        ```

!!! tip "Organization"

    Organizing State is hard, having too deep and complicated routes like `StateTree.local.game.round.time` can pollute rapidly your codebase. We recommend and encourage to split up your state when possible.

    !!! example "Example"

        ```typescript linenums="1"
            import Tina, { State } from "@rbxts/tina";

            const PlayerState = Tina.buildState({
                inventory: State.player<InventoryScheme>(),
            });

            const GameState = Tina.buildState({
                roundInfo: State.namespace({
                    time: State.create(0),
                }),
            });
        ```

        It is also recommended that you use `Tina.buildState()` when declaring a new StateTree, as it will make sense and not only having a namespace holding state.

### :material-function-variant: **`#!typescript create<T>(value: T | (value?: T) => T): LocalState<T>`** { #markdown data-toc-label='create()' }

Builds a new [`#!typescript LocalState`](local.md) object.

#### Parameters

`#!typescript value: T | (value?: T) => T`

: The initial value of type `T` for State to start with.

#### Returns

`#!typescript LocalState<T>`

: Returns the LocalState object.

### :material-function-variant: **`#!typescript global<T>(value: T | (value?: T) => T): GlobalState<T>`** { #markdown data-toc-label='global()' }

Builds a new [`#!typescript GlobalState`](global.md) object.

#### Parameters

`#!typescript value: T | (value?: T) => T`

: The initial value of type `T` for GlobalState to start with.

#### Returns

`#!typescript GlobalState<T>`

: Returns the GlobalState object.

### :material-function-variant: **`#!typescript player<T>(value: T | (value?: T) => T): PlayerState<T>`** { #markdown data-toc-label='player()' }

Builds a new [`#!typescript PlayerState`](player.md) object.

#### Parameters

`#!typescript value: T | (value?: T) => T`

: The initial value of type `T` for PlayerState to start with.

#### Returns

`#!typescript PlayerState<T>`

: Returns the PlayerState object.

!!! note "Subcriptions"

    Subscriptions to State are possible, this opens the possibility to hear for changes on your State tree (at individual points).

    ??? example "Example"

        ```typescript linenums="1"
            import Tina, { State } from "@rbxts/tina";

            const StateTree = Tina.buildState({
                testing: State.create(0),
            });

            StateTree.testing.when().do((oldValue: number) => {
                // Handle state change
            });

            StateTree.testing.set((oldValue) => {
                return oldValue + 1;
            });
        ```

        !!! tip "Same to EventListeners"

            You can see the way to subscribe to your State it's similar to EventListeners, thus meaning you can take advantage of some other methods such as `.cond()` & `.await()`.
