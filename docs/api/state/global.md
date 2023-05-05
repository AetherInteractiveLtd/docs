---
icon: material/cube
---

## :material-package-variant-closed: GlobalState

### :material-function-variant: **`#!typescript get(): T`** { #markdown data-toc-label='GlobalState.get()' }

#### Returns

`#!typescript T`

: Returns current global state of type `T`.

### :material-function-variant: **`#!typescript set(setter: T | (oldValue?: T) => T): void`** { #markdown data-toc-label='LocalState.set()' }

#### Parameters

`#!typescript setter: T | (oldValue?: T) => T`

: A setter, which replicates the new value to all the clients.

!!! example "Example"

    ```typescript linenums="1"
        import Tina, { State } from "@rbxts/tina";

        const StateTree = Tina.buildState({
            global: State.namespace({
                round: State.global(0),
            }),
        })

        StateTree.global.round.set(1);
    ```
