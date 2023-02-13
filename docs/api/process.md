---
icon: material/cube
---

#  

## :material-cube: Process

### :material-lightning-bolt: Events

### :material-lightning-bolt: [**`<empty>`** :octicons-link-16:](event_listener.md/#using-blank-events "Using blank events...")

#### Event Parameters

`#!typescript dt: number`

:   `dt` is always equal to the delta between the last time a tick was processed and the current time. This value is presented in seconds.
    ???+ warning
        This will not respect the latest run of the current process, it will respect the latest run of the overall process scheduler.

---

### :material-function-variant: **`#!typescript when(): EventListener`** { #markdown data-toc-label='when' }

#### Returns

[`#!typescript EventListener` :octicons-link-16:](event_listener.md)

: A new `EventListener` for you to run actions during a `Process` tick.

---

### :material-function-variant: **`#!typescript resume(): boolean`** { #markdown data-toc-label='resume' }

This resumes your `Process` **if** it was paused, otherwise it does nothing.

#### Returns

`#!typescript boolean`

: Whether or not resuming the process did anything. This basically returns `#!typescript true` if the process did not run during this tick and will now run the next tick.

---

### :material-function-variant: **`#!typescript suspend(ticks: number): void`** { #markdown data-toc-label='suspend' }

Prevent any listeners on this `Process` from being called for the next `ticks` simulation steps.

#### Parameters
`#!typescript ticks: number = 1`

: The amount of simulation steps to pause for.

#### Returns

`#!typescript void`

!!! warning "Client/Server Parity Issue"

    On the `Client`, `Process`es will always run at 60 Ticks per Second.
    
    On the `Server`, `Process`es will always run at whatever tick-rate you selected in your [Manifest](/docs/api/types/manifest.md)

---

!!! example "Examples"

    === "Process Creation"
        ```typescript
        import Tina from '@rbxts/tina';

        let GameplayLoop = Tina.process("gameloop"); // (1)!

        // If we ever forget or lose the process, we can always just fetch it;

        GameplayLoop = Tina.process("gameloop"); // (2)!
        ```

        1. :material-information-outline: Here we create a new process called `gameloop`.
        2. :material-information-outline: This will get us the exact same process, `gameloop`.
    
    === "Process Usage"
        ```typescript
        import Tina from `@rbxts/tina`;

        GameplayLoop
            .when()
            .do((dt: number /* (1)!  */) => {
                console.log("Hey!");
            });
        ```

        1.  :material-information-outline: `dt` is always equal to the delta between the last time a tick was processed and the current time.

            This will not respect the latest run of the current process, it will respect the latest run of the overall process scheduler.