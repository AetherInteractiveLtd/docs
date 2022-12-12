## :material-cube-outline: EventEmitter

### :material-function-variant: **`#!typescript when(name?: string): EventListener`** { #markdown data-toc-label='when' }

#### Parameters

`#!typescript name: string`

: The `name` of the event if there is one, refer to [Using Blank Events](event_listener.md/#using-blank-events) otherwise.

#### Returns

[`#!typescript EventListener` :octicons-link-16:](event_listener.md)

: Returns a new `EventListener`.

---

!!! example "Examples"

    === "EventEmitter Creation"
        ```typescript
        import Tina, { EventEmitter } from '@rbxts/tina';

        interface MyEvents {
            bar: [ newState: number ]; // (1)!
        }

        class Foo extends EventEmitter<MyEvents> /* (2)! */ {}
        ```

        1. :material-information-outline: This is the spread of arguments that any EventListeners for this specific event will receive.
        2. :material-information-outline: This will create an EventEmitter on our class which uses the event definitions from `MyEvents`.

        !!! tip "Blank Events"
            You can use [Blank Events](event_listener.md/#using-blank-events) when your `EventEmitter` is specifically mono-behaviour or if you've got a main *(really overly important)* event somewhere.
    
    === "EventEmitter Usage"
        ```typescript
        import Foo from `./Foo`;

        const obj = new Foo(); // (1)!

        obj.when("bar") // (2)!
            .do((state: number /* (3)! */) => {

            });
        ```

        3.  :material-information-outline: EventEmitters can never be static, they need an object in order to work.
        4.  :material-information-outline: We need to fetch a new EventListener for this event, `.when(name)` will deliver one.
        5.  :material-information-outline: We defined this event as `#!typescript bar: [ newState: number ]`, so we can use this parameter in our first `.do()`