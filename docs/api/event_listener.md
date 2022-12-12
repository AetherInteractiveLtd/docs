A Process should operate as any core feature.

## Using blank events

A blank event is used just like any other event, but the `.when()` is simply empty instead of specifying an event name, this is used for simplicity when an object *can* only emit a single event.

??? tip "Creating your Own"
    In your `EventEmitter` interface, simply specify a `_default` key.

    !!! example
        ```typescript linenums="1"
        interface MyEvents {
            genericEvent: [ change: string ];
            _default: [ time: number ];
        }

        class Foo extends EventEmitter<MyEvents> {}

        const object = new Foo();

        object.when("genericEvent"); // This listens to the `genericEvent` event.

        object.when(); // This listens to the `_default` event.
        ```

## :material-cube-outline: EventListener

### :material-function-variant: **`#!typescript do(callback: (args: ...unknown[]) => unknown): this`** { #markdown data-toc-label='do' }

Runs a function in sequence, see [Chainability](#chainability).

#### Parameters

`#!typescript callback: (args: ...unknown[]) => unknown`

:   The chained callback, it should receive the last returned value(s) and can return anything you want it to.

    Please note that whatever you return is what will be given to the next point in the chain.

#### Returns

[`#!typescript EventListener` :octicons-link-16:](#eventlistener)

:   The same [`EventListener`](#eventlistener).

---

### :material-function-variant: **`#!typescript condition(cond: Condition): this`** { #markdown data-toc-label='condition' }

Runs a condition in sequence, see [Chainability](#chainability). If the condition fails, the chain will simply stop, if it succeeds, the chain will continue.

#### Parameters

[`#!typescript cond: Condition` :octicons-link-16:](condition.md)

:   The chained [`Condition`](condition.md), it should receive the last returned value(s) and can return anything you want it to.

#### Returns

[`#!typescript EventListener` :octicons-link-16:](#eventlistener)

:   The same [`EventListener`](#eventlistener).


--- 

## Chainability

Chainability is one of those more frustrating concepts, `EventListener`s are objects that are obtained from an [`EventEmitter`](event_emitter.md). Each EventListener is its own chain, this chain is interruptable only by a `.condition()` failing on the latest value, and will continue running `.do()`s until the chain ends or until the condition fails.


!!! example "Chain Example"
    === "Code"

        ```typescript linenums="1"
        interface MyEvents {
            foo: [ name: string, thing: number ];
        }

        const events = new EventEmitter<MyEvents>(); // (1)!

        events.when('foo')
            .do((name: string, thing: number) => {
                return thing > 10;
            })
            .do((correct: boolean) => {
                return correct ? 10 : 0;
            })
            .condition(COND.create((n: number) => n === 10))
            /* This will only continue if the number (4) is equal to 10.  */
            .do((n: number /* (2)! */) => {
                return number.toString();
            })
            .do((final: string) => {
                console.log(final);
                // no return! (3)
            });

        events.call('foo', "Mark", 3); // The console will see nothing.
        events.call('foo', "Taylor", 15); // The console will see "10".
        ```

        1. This isn't technically valid code as it isn't possible to instantiate an EventEmitter.
        2. This number will be whatever was returned by the last `.do()`, this is because conditions do not return anything, they only halt or continue simulation.
        3. It's perfectly fine to return nothing, EventListeners don't pass anything anywhere except within their specific chain, every time you call `.when()` on an EventEmitter, you get a completely clean chain.
        4. The last returned number. (line 12)

    === "Graph"

        ``` mermaid
        flowchart LR
            R("EventEmitter.call('foo', string, number)") --> Y
            subgraph Y["EventListener"]
            direction TB
            A(".when('foo')") ---|Whenever an event occurs| B(".do((string, number) => boolean)") & X["Propagation of .await() resuming"]
            B --> C(".do((boolean) => number)")
            C --> D{{".condition((number) => boolean)"}}
            D -->|True| E(".do((number) => string)")
            D ===>|False| F["END"]
            E --> G(".do((string) => any)")
            click X "#await" ".await() is documented above"
            end
        ```
!!! note "Difference to Promises"
    Whilst you might expect this to work like promises, each `.do()` call does **not** create a new `EventListener`; Promises differ in that they give you a new Promise with every `.when()`, but here we have full chaining, one object remains a single object.

    ???+ warning "Dangers of this Implementation"
        It is advised not to put EventListeners in variables, especially if you're trying to mutate their chain, this is a problem with typings and you will not receive the correct chain value return type if you do this more than once.

        ??? example "Fail-Case"
            ```typescript linenums="1"
            let event = EventEmitter.when("foo");

            event.do((v: number) => v.toString());

            event.do((x: /* (1)! */) => console.log(x))
            ```

            1. This fails, typechecking will assume that you're receiving a `number` as the `EventListener` was not reassigned. In fact, you're receiving a `string`.