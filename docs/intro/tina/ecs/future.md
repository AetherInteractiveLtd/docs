# The future of Tina ECS
 
This document is a work in progress and will be updated as the project progresses. It is intended to be an overview of features that are likely to be added to Tina in the future. It is not a guarantee that these features will be added, and is subject to change.

## Flyweight Support

Flyweight components are components that are shared between entities. Rather than storing the data for a component in each entity, the data is stored in a single location and each entity is given a reference to the data. This allows for more efficient use of memory, as the data is only stored once.

