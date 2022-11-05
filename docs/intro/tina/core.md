# Main Usage

## Game

Tina requires certain initialization on the Server and the Client.

`index.server.ts`

```typescript
import Tina from '@rbxts/tina';

import 'lib/game'; // Initialize Game.
import 'lib/net'; // Initalize Networking.
import 'lib/ecs'; // Initalize Entity Component System.

import { User } from 'shared/lib/game/user';

/* Starts the Game Processes */
Tina.startGame("GuessTheNumber");
Tina.setUserClass(User); // Sets the custom user class.
```

`index.client.ts`

```typescript
import Tina from '@rbxts/tina';

import 'lib/game'; // Initialize Game.
import 'lib/net'; // Initialize Networking.
import 'lib/ecs'; // Initialize Entity Component System.

import { User } from 'shared/lib/game/user';

// TODO: Decide what client-side initialization needs to look like.
```

## Events

```typescript

```
