---
icon: material/cube
---

## :material-cube: World

The world is the main access point for ECS functionality. It is responsible for creating and managing entities, components, and systems.

Typically there is only a single world, but there is no limit on the number of worlds an experience can create.

#### Usage

```typescript
import { World } from "@rbxts/tina";

const world = new World({...});
```

#### Parameters

`#!typescript id?: string`

 : A unique identifier (UUID) for the world.

`#!typescript readonly options?: WorldOptions`

 : The [WorldOptions](../api/worldoptions.md/#markdown "WorldOptions") that were passed into the constructor when the world was created.


### :material-function-variant: **`#!typescript public add(): EntityId`** { #add data-toc-label='add' }

Creates a new entity in the world.

When the entity is added to the world, the id will be assigned immediately, but as all other operations are deferred, the entity will not be added to any queries until the system has finished executing.

#### Returns

`#!typescript EntityId`

 : The id of the newly created entity.


### :material-function-variant: **`#!typescript public addComponent<T extends Tree<Type>>(entityId: EntityId, component: Component<T>): this`** { #addComponent data-toc-label='addComponent' }

Adds a given component to the entity. If the entity does not exist, then an error will be thrown. No error will be thrown if the entity already has the component.

#### Parameters
`#!typescript entityId: EntityId`

 : The id of the entity to add the component.

`#!typescript component: Component<T>`

 : The component to add to the entity, which must have been defined previously with [`#!typescript createComponent()`](../api/component.md/#markdown "createComponent()").

`#!typescript data?: Partial<OptionalKeys<GetComponentSchema<C>>`

 : The optional data to initialize the component with. This must match keys of the original component schema.

#### Returns

`#!typescript this`

 : The world instance to allow for method chaining.


### :material-function-variant: **`#!typescript public addTag(entityId: EntityId, tag: TagComponent): this`** { #addTag data-toc-label='addTag' }

Adds a tag component to an entity.

#### Parameters
`#!typescript entityId: EntityId`

 : The id of the entity to add the tag.

`#!typescript tag: TagComponent`

 : The tag component to add to the entity, which must have been defined previously with [`#!typescript createTag()`](../api/component.md/#markdown "createTag()").

#### Returns

`#!typescript this`

 : The world instance to allow for method chaining.


### :material-function-variant: **`#!typescript public clear(): void`** { #clear data-toc-label='clear' }

Removes all entities from the world.

!!! warning "This is currently not implemented and will throw an error if called."


### :material-function-variant: **`#!typescript public createQuery(...raw: Array<RawQuery>): Query`** { #createQuery data-toc-label='createQuery' }

Creates a new query to filter entities based on the given components.

#### Usage Example:

```typescript
import { ALL, ANY, NOT } from "@rbxts/tina";
import { Position, Velocity, Acceleration } from "./components";

const query = world.createQuery(Position, ANY(Velocity, NOT(Acceleration)));
```

#### Parameters
`#!typescript ...raw: Array<RawQuery>`

 : A query using components, and optionally the provided query helper functions [`#!typescript ALL()`](../api/query.md/#markdown "ALL()"), [`#!typescript ANY()`](../api/query.md/#markdown "ANY()"), and [`#!typescript NOT()`](../api/query.md/#markdown "NOT()").

#### Returns

`#!typescript Query`

 : A new [Query](../api/query.md/#markdown "Query").

### :material-function-variant: **`#!typescript public destroy(): void`** { #destroy data-toc-label='destroy' }

Halts the current execution of the world, and destroys the world.

!!! warning "Depending on the number of entities a world has, this can be a costly operation as we have to return all the entities to the pool."

### :material-function-variant: **`#!typescript public disableSystem(): this`** { #disableSystem data-toc-label='disableSystem' }

Disables the given system. This will prevent the system from being executed, but will not remove it from the scheduler.

As scheduling a system can be a potentially expensive operation, this should be used for systems that are expected to be re-enabled in the future.

#### Parameters
`#!typescript system: System`

 : The system to disable.

#### Returns

`#!typescript this`

 : The world instance to allow for method chaining.

### :material-function-variant: **`#!typescript public enableSystem(): this`** { #enableSystem data-toc-label='enableSystem' }

Enables the given system.

This will not error if a system that is already enabled is enabled again.

#### Parameters
`#!typescript system: System`

 : The system that should be enabled.

#### Returns
    
`#!typescript this`
    
: The world instance to allow for method chaining.

### :material-function-variant: **`#!typescript public flush(): void`** { #flush data-toc-label='flush' }

Flushes any pending entity removal, or deferred component changes in the world.

This is called automatically whenever a system has finished executing, and should not typically be called manually.

If you are not using the inbuilt scheduler, you should call this method at regular intervals to ensure that any pending changes are applied.

### :material-function-variant: **`#!typescript public has(entityId: EntityId): boolean`** { #has data-toc-label='has' }

Checks if a given entity is currently alive in the world.

#### Parameters
`#!typescript entityId: EntityId`

 : The id of the entity to check.

#### Returns

`#!typescript boolean`

 : True if the entity is alive, false otherwise.


### :material-function-variant: **`#!typescript public hasAllOf(entityId: EntityId, ...components: Array<AnyComponent>): boolean`** { #hasAllOf data-toc-label='hasAllOf' }

Checks if a given entity has all of the given components.

#### Parameters
`#!typescript entityId: EntityId`

 : The id of the entity to check.

`#!typescript ...components: Array<AnyComponent>`

 : Any number of components to check against.


#### Returns

`#!typescript boolean`

 : True if the entity has all of the given components, false otherwise.


### :material-function-variant: **`#!typescript public hasAnyOf(entityId: EntityId, ...components: Array<AnyComponent>): boolean`** { #hasAnyOf data-toc-label='hasAnyOf' }

Checks if a given entity has any of the given components.

#### Parameters
`#!typescript entityId: EntityId`

 : The id of the entity to check.

`#!typescript ...components: Array<AnyComponent>`

 : Any number of components to check against.


#### Returns

`#!typescript boolean`

 : True if the entity has at least one of the given components, false otherwise.


### :material-function-variant: **`#!typescript public hasComponent(entityId: EntityId, component: AnyComponent): boolean`** { #hasComponent data-toc-label='hasComponent' }

Returns whether or not the given entity has the given component.

#### Parameters
`#!typescript entityId: EntityId`

 : The id of the entity to check.

`#!typescript component: AnyComponent`

 : Any number of components to check against.

#### Returns

`#!typescript boolean`

 : True if the entity has the given component, false otherwise.


### :material-function-variant: **`#!typescript public hasNoneOf(entityId: EntityId, ...components: Array<AnyComponent>): boolean`** { #hasNoneOf data-toc-label='hasNoneOf' }

Checks if a given entity has none of the given components.

#### Parameters
`#!typescript entityId: EntityId`

 : The id of the entity to check.

`#!typescript ...components: Array<AnyComponent>`

 : Any number of components to check against.


#### Returns
    
`#!typescript boolean`
    
 : True if the entity has none of the given components, false otherwise.


### :material-function-variant: **`#!typescript public hasTag(entityId: EntityId, tag: TagComponent): boolean`** { #hasTag data-toc-label='hasTag' }

Returns whether or not the given entity has the given tag.

#### Parameters
`#!typescript entityId: EntityId`

 : The id of the entity to check.

`#!typescript tag: TagComponent`

 : The tag to check against.

#### Returns

`#!typescript boolean`

 : True if the entity has the given tag, false otherwise.


### :material-function-variant: **`#!typescript public pause(): void`** { #pause data-toc-label='pause' }

Pauses the execution of the world.

!!! warning "This is currently not implemented and will throw an error if called."


### :material-function-variant: **`#!typescript public play(): void`** { #play data-toc-label='play' }

Continues the execution of the world from its current state.

!!! warning "This is currently not implemented and will throw an error if called."


### :material-function-variant: **`#!typescript public remove(entityId: EntityId): this`** { #remove data-toc-label='remove' }

Removes the given entity from the world, including all of its components.

The entity will be removed from the world upon the next call to [`#!typescript flush()`](../api/world.md/#markdown "flush()").

#### Parameters
`#!typescript entityId: EntityId`

 : The id of the entity to remove.

#### Returns

`#!typescript this`

 : The world instance to allow for method chaining.


### :material-function-variant: **`#!typescript public removeComponent(entityId: EntityId, component: AnyComponent): this`** { #removeComponent data-toc-label='removeComponent' }

Removes the given component from the given entity.

The component will be removed from the entity upon the next call to [`#!typescript flush()`](../api/world.md/#markdown "flush()").

#### Parameters
`#!typescript entityId: EntityId`

 : The id of the entity to remove the component.

`#!typescript component: AnyComponent`

 : The component to remove.

#### Returns

`#!typescript this`

 : The world instance to allow for method chaining.


### :material-function-variant: **`#!typescript public removeQuery(query: Query): this`** { #removeQuery data-toc-label='removeQuery' }

Removes a query from the world.

Queries typically do not need to be removed and should last the lifetime of the world. However, this method is provided for cases in which this statement proves false.

#### Parameters
`#!typescript query: Query`

 : The query to remove.

#### Returns

`#!typescript this`

 : The world instance to allow for method chaining.


### :material-function-variant: **`#!typescript public removeTag(entityId: EntityId, tag: TagComponent): this`** { #removeTag data-toc-label='removeTag' }

Removes the given tag from the given entity.

#### Parameters
`#!typescript entityId: EntityId`

 : The id of the entity to remove the tag.

`#!typescript tag: TagComponent`

 : The tag to remove.

#### Returns

`#!typescript this`

 : The world instance to allow for method chaining.


### :material-function-variant: **`#!typescript public scheduleSystem(system: System): this`** { #scheduleSystem data-toc-label='scheduleSystem' }

Schedules an individual system to be executed in the world.

Calling this function is a potentially expensive operation and should be avoided if possible. It is best advised to use [`#!typescript scheduleSystems()`](../api/world.md/#markdown "scheduleSystems()") instead, and add multiple systems at once - this is to avoid the unnecessary overhead of sorting systems.

#### Parameters
`#!typescript system: System`

 : The system to schedule.

#### Returns

`#!typescript this`

 : The world instance to allow for method chaining.


### :material-function-variant: **`#!typescript public scheduleSystems(...systems: Array<System>): this`** { #scheduleSystems data-toc-label='scheduleSystems' }

Schedules a given set of systems to be executed in the world.

#### Parameters
`#!typescript ...systems: Array<System>`

 : Any number of systems to schedule.

#### Returns

`#!typescript this`

 : The world instance to allow for method chaining.


### :material-function-variant: **`#!typescript public size(): number`** { #size data-toc-label='size' }

#### Returns

`#!typescript number`

 : Returns the number of entities currently in the world.


### :material-function-variant: **`#!typescript public start(): this`** { #start data-toc-label='start' }

Starts the execution of the world.

This should only be called after all components, and preferably all systems have been registered.


### :material-function-variant: **`#!typescript public toString(): string`** { #toString data-toc-label='toString' }

#### Returns

`#!typescript string`

 : The name of the world. This defaults to "World", unless specified by the WorldOptions.


### :material-function-variant: **`#!typescript public unscheduleSystem(system: System): this`** { #unscheduleSystem data-toc-label='unscheduleSystem' }

Unschedule an individual system from the world.

If a system needs to be re-scheduled at a later time, then it is recommended instead to disable it using [`#!typescript disableSystem()`](../api/world.md/#markdown "disableSystem()"), as scheduling a system will require the systems to be sorted again.

#### Parameters
`#!typescript system: System`

 : The system to unschedule.

#### Returns

`#!typescript this`

 : The world instance to allow for method chaining.


### :material-function-variant: **`#!typescript public unscheduleSystems(...systems: Array<System>): this`** { #unscheduleSystems data-toc-label='unscheduleSystems' }

Unschedule a given set of systems from the world.

If a system needs to be re-scheduled at a later time, then it is recommended instead to disable it using [`#!typescript disableSystem()`](../api/world.md/#markdown "disableSystem()"), as scheduling a system will require the systems to be sorted again.


#### Parameters
`#!typescript ...systems: Array<System>`

 : Any number of systems to unschedule.

#### Returns

`#!typescript this`

 : The world instance to allow for method chaining.****