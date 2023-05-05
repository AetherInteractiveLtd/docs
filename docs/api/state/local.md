---
icon: material/cube
---

## :material-package-variant-closed: LocalState

### :material-function-variant: **`#!typescript get(): T`** { #markdown data-toc-label='LocalState.get()' }

#### Returns

`#!typescript T`

: Returns current state of type `T`.

### :material-function-variant: **`#!typescript set(setter: T | (oldValue?: T) => T): void`** { #markdown data-toc-label='LocalState.set()' }

#### Parameters

`#!typescript setter: T | (oldValue?: T) => T`

: A setter, which sets the current state's value to the value passed/evaluated.

!!! example "Example"

    ```typescript linenums="1"
        import Tina, { State } from "@rbxts/tina";

        const StateTree = Tina.buildState({
            global: State.namespace({
                gameIsRunning: State.create(false),
            }),
        })

        StateTree.global.gameIsRunning.set(true);
    ```
