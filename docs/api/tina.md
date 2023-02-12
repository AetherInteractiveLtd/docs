---
icon: material/package-variant-closed
---

# 

## :material-package-variant-closed: Tina

### :material-function-variant: **`#!typescript registerGame(name: string, manifest: Manifest): void`** { #markdown data-toc-label='registerGame()' }

This initializes and registers your game.

#### Parameters
`#!typescript name: string`

: Usually the name of whatever game you're making; if you haven't got one yet, just mash your keyboard! This can always be changed in the future.

[`#!typescript manifest: Manifest` :octicons-link-16:](/docs/api/types/manifest.md)

: A [`Manifest`](/docs/api/types/manifest.md "Manifest Definition assistance") is a file that specifies some important information about your project

#### Returns

`#!typescript void`

!!! warning "Order of Initialization"

    It is important that functions like `Tina.setUserClass()` are called **before** `Tina.startGame()`.
    
    Please also consider that processes will only start running once you've called `Tina.startGame()`.

!!! tip "Well Done!"

    Congrats on starting your latest project, we're glad to see you using Tina and we wish you all the best! :smile:

---

### :material-function-variant: **`#!typescript setUserClass(userClass: new (ref: Player | number) => User): void`** { #markdown data-toc-label='setUserClass' }

#### Parameters
`#!typescript userClass: new (ref: Player | number) => User`

: The `userClass` needs to be the constructor (raw class) of your custom `User` class, instructions on how to implement a custom `User` can be viewed [here](/docs/intro/tina/user  "Custom User Class").

#### Returns

`#!typescript void`

---

### :material-function-variant: **`#!typescript process(name: string): Process`** { #markdown data-toc-label='process' }

Create a [`Process`](/docs/api/process.md), this is the main utility for your so-called "Game Loop".

#### Parameters
`#!typescript name: string`

: The name of your process, make this short but memorable.

#### Returns

[`#!typescript Process` :octicons-link-16:](/docs/api/process.md)

: If a [`Process`](/docs/api/process.md) with this `name` already exists, you'll be given that one, otherwise a new one will be created.