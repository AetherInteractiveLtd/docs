---
icon: material/cube
---

## :material-cube: System

A system is a collection of logic that is executed on a set of entities. Nearly all ECS actions should be implemented during the execution of a system. Queries should be used inside of system functions to filter entities based on their components and then perform actions on those entities.

A system should be exported as a singleton class, and can be registered with multiple worlds should be desired.

```typescript
import { Query, System, World } from '@rbxts/tina';
import { Position, Velocity } from './components';

export const SomeSystem = new (class SomeSystem extends System {
    private query: Query;

    constructor() {
        this.executionGroup = PreAnimation;
        this.name = "SomeSystem";
    }

    public configureQueries(world: World) {
        this.query = world.createQuery(Position, Velocity);
    }

    public onUpdate(world: World) {
        for (const entityId of this.query) {
            // ...
        }
    }
});
```

#### System Parameters

`#!typescript after?: Array<System>`

 : An optional set of systems must be executed before this system. This is useful for ensuring that a system is executed after another system (e.g. when a system relies on the data generated by another system).

`#!typescript dt?: number`

 : The delta time, in seconds, since the last call of this system. This is useful for systems that need to update based on time, such as physics systems.

`#!typescript enabled?: boolean`

 : Whether or not a system should be called. This should not be called directly. Instead, you should use the [`#!typescript world.enableSystem()`](../world/#enableSystem "World.enableSystem()") and [`#!typescript world.disableSystem()`](../world/#disableSystem "World.disableSystem()") functions respectively.

`#!typescript executionGroup?: ExecutionGroup`

 : The group that this system will be executed on, e.g. PostSimulation, PreAnimation, etc. The only requirement for an execution group is that the group has a `Connect` method.

`#!typescript name?: string`

 : The name of the system. This is primarily used for debugging purposes.

`#!typescript priority?: number`

 : The priority of the system. Systems with a higher priority will be executed before systems with a lower priority. This number defaults to `0`, and therefore can be either positive or negative.

### :material-function-variant: **`#!typescript public abstract onUpdate(world: World): void`** { #markdown data-toc-label='onUpdate' }

The onUpdate method is called on every execution of this systems execution group. This should not typically be called manually, and instead, the system should be scheduled according to an execution group using the [`#!typescript world.scheduleSystem()`](../world/#scheduleSystem "World.scheduleSystem()") function.

#### Parameters
`#!typescript world: World`

 : The world that this system is being executed on. This will be automatically passed in by the owning world on each system execution.


### :material-function-variant: **`#!typescript public prepare(): Promise<void>`** { #markdown data-toc-label='prepare' }

The prepare method is optionally called when the system is first run. If this method returns a promise, the system will not be scheduled until the promise resolves.

This hooks into the world's lifecycle and the world will not be started until all systems have been prepared.

This is typically used to perform any asynchronous setup logic such as loading external data or setting up external context useful for the system. The world is not accessible at this point so you should not attempt to use it.

#### Returns
`#!typescript Promise<void>`

 : A promise that resolves when the system has been prepared.

### :material-function-variant: **`#!typescript public initialize(world: World): void`** { #markdown data-toc-label='initialize' }

An optional hook that can be used to initialize the system. Here we could set up initial entities with components, or perform any other setup logic.

This is useful as this will be called at the start of the world but before the first update. This means we can do any initial setup while ensuring that the first update has all the data required.

#### Parameters
`#!typescript world: World`

 : The world that this system belongs to. This will be passed in automatically.

### :material-function-variant: **`#!typescript public configureQueries(world: World): void`** { #markdown data-toc-label='configureQueries' }

The configureQueries method is optionally called when the system is first run. This is a good place to set up any queries that the system will use.

#### Parameters
`#!typescript world: World`

 : The world that this system is being executed on. This will be automatically passed in by the owning world.


### :material-function-variant: **`#!typescript public cleanup(): void`** { #markdown data-toc-label='cleanup' }

An optional hook that can be used to clean up the system. This is useful for cleaning up any external context that was created during the system's lifecycle, or for cleaning up queries that were created.

!!! note "This will only be called if a system is unscheduled from a world."

    You do not typically need to use this function unless you know that a system will be removed from a world, or that the world itself could be destroyed.