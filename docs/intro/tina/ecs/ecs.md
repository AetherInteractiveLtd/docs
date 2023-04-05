
# Using the ECS

The ECS (Entity Component System) is a core feature of Tina, that allows a user to represent "things in the world", in a data-driven approach. Rather than representing objects through the typical use of practices such as OOP and inheritance, we take a pure composition approach that gives us the ability to build abstract instances through the sum of their components alone.

## What is an ECS?

> ECS ("Entity Component System") describes a design approach which
> promotes code reusability by separating data from behavior. Data is
> often stored in cache-friendly ways which benefit performance. An ECS
> has the following characteristics:
>   
> - It has entities, which are unique identifiers
> - It has components, which are plain datatypes without behavior
> - Entities can contain zero or more components
> - Entities can change components dynamically
> - It has systems, which are functions matched with entities that have a certain set of components.  
> 
> The ECS design pattern is often enabled by a framework. The term
> "Entity Component System" is often used to indicate a specific
> implementation of the design pattern.

For more in-depth information, check out [this FAQ](https://github.com/SanderMertens/ecs-faq) from the creator of [Flecs](https://github.com/SanderMertens/flecs).

## Getting Started

To get started with the ECS, first, you need to create a new `world`. The world is the main entry point for the ECS, and nearly all functionality will be accessed through it. The world is considered a container for your data. 

Typically you will only need one world per game, but there is no restriction on the number of worlds that can be created.

<h5 a><strong><code>main.server.ts</code></strong></h5>

```ts
/* We provide default options to the world, which we will cover later. */
const world = Tina.createWorld({...});
```

From here, you can schedule systems to run, add entities, and more.

### Entities

An entity is a unique thing in your game. It could represent a player, a projectile, a tree, etc.

To create our first entity, we can use the following method:

<h5 a><strong><code>main.server.ts</code></strong></h5>
```ts
const entityId = world.add();
```

We now have our entity id. But what is an entity id, and how does it allow us to represent our player? The neat part is that an entity is just a number! A plain old number (1, 2, 3...?)! This number is always unique for every entity in your game. This is where the **component** part of an ECS comes in!

### Components
Components are singleton containers for data. They are plain data types, and typically should not contain any behavior(1).
{ .annotate }

1. Sometimes it can be useful to have functions such as getters and setters within components, but since we shouldn't be using these in Roblox-TS anyway, this point is kind of moot.

Each field that a component has is internally represented as an array of values, where the index of the array corresponds to the entity to which it belongs. E.g. index 1 of the Position component array would be the position of entity 1 (assuming that entity 1 has a position component).

<h5 a><strong><code>components/example-component.ts</code></strong></h5>

```ts
import { ComponentTypes, Tina } from "@rbxts/tina";

export const Position = Tina.createComponent({
    value: ComponentTypes.Vector3,
});
```

We can then `add` this component to our entity.

<h5 a><strong><code>main.server.ts</code></strong></h5>

```ts
world.addComponent(entityId, Position, {
    value: new Vector3(100, 5, 100),
});

/* This is syntactic sugar for the below. */
world.addComponent(entityId, Position); /* (1)! */
Position.value[entityId] = new Vector3(100, 5, 100);
```
{ .annotate }

1. If an entity already has the given component, the component will not be added to the entity again, and the data will not be updated. If you need to ensure that the component data changes, check first if the entity has the component. 


!!! Note

    Unlike some other ECS implementations, Tina does not require you to register components with the underlying world. Tina stores component and entity ids as global identifiers, (e.g. entity 1 will only ever exist in one world at a time) therefore there are no collisions between worlds. 

!!! Tip

    If you only have one value on a component, it can be tempting to name it the same thing as the component name, e.g. a value `position` on the Position component. We recommend in these cases naming it `value`. This is so that indexing a certain property does not look like `Position.position`, and instead `Position.value`.


### Tags

Tags are a special type of component that do not contain any data. Internally they are treated as a component but do not have the overhead of data storage. These should be used over components when all you wish to do is mark an entity as having a certain property. E.g. if you want to mark an entity as being an "enemy", you could use a tag instead of a component.

<h5 a><strong><code>tags/example-tag.ts</code></strong></h5>

```ts
import { Tina } from "@rbxts/tina";

export const EnemyTag = Tina.createTag();

world.addTag(enemyEntityId, EnemyTag);
```

### Flyweight Components

A flyweight is a design pattern that is useful for minimizing memory usage when similar objects should share the same data.

Flyweight components are an abstraction over this idea. Rather than the component object holding an array of values for each entity, the flyweight component has only a singular storage object.

This component can then be added to any entity, and easily use the data where required.

<h5 a><strong><code>tags/example-flyweight.ts</code></strong></h5>

```ts
import { Tina } from "@rbxts/tina"

export const Zombie = Tina.createFlyweight({
    Texture: "rbxassetid://11688231106"
    Sound: "rbxassertid://1838569831"
})

world.addComponent(entityId, Zombie);
```

### Systems

Systems are the main way to schedule your code with the ECS. They are functions that run on a given execution group (e.g. `PostSimulation`) and allow you to easily manage the execution of your code.

Systems in Tina are set up using a class.

<h5 a><strong><code>systems/example-system.ts</code></strong></h5>

```ts
import { System } from "@rbxts/tina";

export class ExampleSystem extends System {
    constructor() {
        // You can set system options in the super call, or below. We'd
        // recommend sticking to one or the other.
        super({
            executionGroup: RunService.PostSimulation,
        });
        
        /* The execution group that this system will run on. */
        this.executionGroup = RunService.PostSimulation,
    }

    /** onUpdate will be called every time the execution group fires. */
    public onUpdate(world: World) {
        print("Hello, world!");
    }
};
```

Systems have a few lifecycle methods that you can opt into.

#### Prepare

This is the only asynchronous lifecycle method allowed in a system. It is called when the world is first created and allows you to perform any setup logic. The world is not accessible at this point, so you should not attempt to use it. The system will not be scheduled until this method resolves.


#### Initialize

This is called when the system is first scheduled. This is useful for setting up any data that you need to use in the system, such as creating entities that you will use later on.


#### Configure Queries

This is called when the system is first scheduled. This is useful for setting up any queries that you need to use in the system (see [Queries](#queries)).


#### OnUpdate

This is called every time the execution group fires. This is where you should perform any logic that you want to run regularly, and is where the majority of your gameplay logic should be.


#### Cleanup

This is called when the system is unscheduled. This is useful for cleaning up any data that you created in the system. Typically most systems will never be unscheduled and run for the lifetime of the world, so this function will not typically be used.

### Queries

Queries are used to match entities that have a certain set of components. They are typically used within systems to allow us to easily batch iterate over entities that match a certain set of components.

<h5 a><strong><code>systems/example-system.ts</code></strong></h5>

```ts
import { System } from "@rbxts/tina";
import { Position, Velocity } from "components";
import { RunService } from "@rbxts/services";

/* It can be beneficial to store a reference to these values. */
const position = Position.value;
const velocity = Velocity.value;

export class QuerySystem extends System {
    private movementQuery!: Query;

    constructor() {
        super();
    }

    /**
     * Configure queries is automatically called when the system is scheduled.
     * We can use this to create any queries that we need.
     */
    public configureQueries(world: World) {
        this.movementQuery = world.createQuery(ALL(Position, Velocity));
    }

    /** onUpdate will be called every time the execution group fires. */
    public onUpdate(world: World) {
        for (const entityId of this.movementQuery.iter()) {
            // Every time we enter this system, we will get every entityId that
            // has both a Position and Velocity component.
            position[entityId] = position[entityId].add(velocity[entityId]);
        };
    }
})();
```

## Putting it all together

```ts
import { RunService } from "@rbxts/services";

import { World } from "@rbxts/tina";
import { ExampleSystem } from "./systems/example-system";

const world = new World({
    defaultExecutionGroup: RunService.PostSimulation,
    name: "MyWorld",    
});

world.scheduleSystem(new ExampleSystem());
await world.start()

const id = world.add();
world
	.addComponent(entityId, Position, { value: new Vector3(5, 0, 5) })
	.addComponent(entityId, Velocity, { value: new Vector3(1, 2, 3) });

```
