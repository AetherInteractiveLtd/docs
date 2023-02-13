---
icon: material/cube
---

## :material-cube: Query

A query is used to filter entities from the world based on their components.

To create a query, use the [`#!typescript createQuery()`](../world/#createQuery "createQuery()") method, which takes a set of components, and will return a query that matches all the entities in the owning world that have the given components.

Queries can be created using the helper functions [`#!typescript ALL()`](#all "ALL()"), [`#!typescript ANY()`](#any "ANY()"), and [`#!typescript NOT()`](#not "NOT()"), which can be used to create complex queries.


#### Usage

```typescript
import { World, ALL, ANY, NOT } from '@rbxts/tina';
import { Position, Velocity, Acceleration } from './components';

const world = new World({...});
const query = world.createQuery(Position, ANY(Velocity, NOT(Acceleration)));
query.forEach(entityId => {
    // ...
});

// or

for (const entityId of query) {
    // ...
}
```

!!! note "The order of iteration for a query is not guaranteed."


### :material-function-variant: **`#!typescript public match(target: Array<ComponentId>, mask: QueryMask): boolean`** { #match data-toc-label='match' }

Traverses the query mask, and returns true if the archetype mask matches the given query.

This function is not typically used directly but is used internally by the world to determine if an entity matches a query.

#### Parameters
`#!typescript target: Array<ComponentId>`

 : The archetype mask to match against.

`#!typescript mask: QueryMask`

 : The query mask to match against.

#### Returns

`#!typescript boolean`

 : True if the query mask matches the archetype mask.



### :material-function-variant: **`#!typescript public forEach(callback: (entityId: EntityId) => void): void`** { #forEach data-toc-label='forEach' }

Runs a callback for each entity that matches the query.

If the callback returns `false`, the iteration will stop, and no other entities in the query will be iterated over.

#### Usage:
```typescript
query.forEach(entityId => {
    // ...
});
```

#### Parameters
`#!typescript callback: (entityId: EntityId) => boolean | void`

 : The callback ran for each entity in the query.


## Query Helper Functions

Below are a set of helper functions that can be used to create queries.

These functions are used in conjunction with the [`#!typescript createQuery()`](../world/#createQuery "createQuery()") method, and should not be used directly.

### :material-function-variant: **`#!typescript ALL(...components: Array<RawQuery | AnyComponent>): RawQuery`** { #all data-toc-label='ALL' }

Matches to all provided components.

#### Usage:
```typescript
// An entity must have components A, B and C.
ALL(ComponentA, ComponentB, ComponentC)

// An entity must have component A, and either component B or C.
ALL(ComponentA, ANY(ComponentB, ComponentC))
```

#### Parameters
`#!typescript ...components: Array<RawQuery | AnyComponent>`

 : Any number of components to match against.


### :material-function-variant: **`#!typescript ANY(...components: Array<RawQuery | AnyComponent>): RawQuery`** { #any data-toc-label='ANY' }

Matches to any of the provided components.

#### Usage:
```typescript
// An entity must have either component A, B or C.
ANY(ComponentA, ComponentB, ComponentC)

// An entity must have either component A, or both component B and C.
ANY(ComponentA, ALL(ComponentB, ComponentC))
```

#### Parameters
`#!typescript ...components: Array<RawQuery | AnyComponent>`

 : Any number of components to match against.


### :material-function-variant: **`#!typescript NOT(...components: Array<RawQuery | AnyComponent>): RawQuery`** { #not data-toc-label='NOT' }

Matches to entities that do not have any of the provided components.

#### Usage:
```typescript
// An entity must not have component A, B or C.
NOT(ComponentA, ComponentB, ComponentC)

// An entity must not have component A, or both component B and C.
NOT(ComponentA, ALL(ComponentB, ComponentC))
```

#### Parameters
`#!typescript ...components: Array<RawQuery | AnyComponent>`

 : Any number of components to match against.


