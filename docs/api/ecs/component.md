---
icon: material/cube
---

## :material-cube: Component

// What is a component?

<!-- todo: ComponentTypes -->

### :material-function-variant: **`#!typescript createComponent<T extends Tree<Type>>(schema: T): Component<T>`** { #markdown data-toc-label='createComponent' }

Creates a component that matches the given schema.

Internally this creates an array for each property in the schema, where the index of the array matches an entity id. This allows for fast lookups of component data.

The array is pre-allocated to the given size, so it is important to ensure that you do not access the component data for an entity that does not exist, or that does not have the component. This is because the array could hold data for a given entity, even though the entity would be invalid. You can use [`#!typescript world.hasComponent()`](../api/entityid.md/#markdown "EntityId.isValid()") to ensure that the entity has the given component before accessing the component data if required.

Components are singletons and should be created once per component type. Components also persist between worlds, therefore you do not need more than one component per world. EntityIds are global, therefore the index of a given entity will always match the index of the component data.

#### Parameters
`#!typescript schema: Tree<Type>`

 : The schema of the component. This can match any type, but it is recommended to use [`#!typescript ComponentTypes`](../api/tree.md/#markdown "ComponentTypes") to ensure that the component is automatically serializable.

#### Returns

`#!typescript Component<T>`

 : The [`#!typescript Component`](../api/component.md/#markdown "Component") that was created.

### :material-function-variant: **`#!typescript createTag(): TagComponent`** { #markdown data-toc-label='tagComponent' }

Creates a tag component; a component that has no data.

Tags are useful for marking entities as having a certain property, without the overhead **of** storing any data. For example, you could use a tag component to mark an entity as being a player, and then use a system to query for all entities that have the player tag.

Tags are singletons and should be created once per component type. Tags also persist between worlds, therefore you do not need more than one Tag per world.
