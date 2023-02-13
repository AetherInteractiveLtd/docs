---
icon: material/cube
---

## :material-cube: Component { #component }

A component is a datatype that can be added or removed from entities. Components are typically plain data, which are defined by creating a schema using [`#!typescript ComponentTypes`](#componentTypes "ComponentTypes")

Internally this creates an array for each property in the schema, where the index of the array matches an entity id. This allows for fast lookups of component data.

The array is pre-allocated to the given size, so it is important to ensure that you do not access the component data for an entity that does not exist, or that does not have the component. This is because the array could hold data for a given entity, even though the entity would be invalid. You can use [`#!typescript world.hasComponent()`](../world/#hasComponent "world.hasComponent()") to ensure that the entity has the given component before accessing the component data if required.

Components are singletons and should be created once per component type. Components also persist between worlds, therefore you do not need more than one component per world. EntityIds are global, therefore the index of a given entity will always match the index of the component data.

#### Usage

```typescript
const Position = Tina.createComponent({
    value: ComponentTypes.Vector3,
})

const PositionOfEntity = Position.value[entityId];
```

<!-- todo: ComponentTypes -->

### :material-function-variant: **`#!typescript createComponent<T extends Tree<Type>>(schema: T): Component<T>`** { #createComponent data-toc-label='createComponent' }

Creates a component singleton instance that matches the given schema.

Internally this adds a `componentId` to the component, and setups the component data array.

#### Parameters
`#!typescript schema: Tree<Type>`

 : The schema of the component. This can match any type, but it is recommended to use [`#!typescript ComponentTypes`](#componentTypes "ComponentTypes") to ensure that the component is automatically serializable.

#### Returns

`#!typescript Component<T>`

 : The [`#!typescript Component`](#component "Component") that was created.

### :material-function-variant: **`#!typescript createTag(): TagComponent`** { #createTag data-toc-label='createTag' }

Creates a tag component; a component that has no data.

Tags are useful for marking entities as having a certain property, without the overhead **of** storing any data. For example, you could use a tag component to mark an entity as being a player, and then use a system to query for all entities that have the player tag.

Tags are singletons and should be created once per component type. Tags also persist between worlds, therefore you do not need more than one Tag per world.
