# Conditions

Conditions lets you perform and evualate boolean operations. Should be used whenever you need to evaluate any operation that utilizes booleans, whether it's wrapped on a `.condition()` or in an if-statement.

### Creating conditions

Conditions can be created as in `Conditions.create(f: (...args) => boolean)`. The function can take optional arguments when being evaluated.

```ts
Conditions.create((n: number) => (n < 5)), 0))
```

### Evaluating conditions

Conditions by itself aren't a bit thing, they need some way to be evaluated. To evaluate conditions we use `.eval(condition: (boolean | (...args) => boolean))` method.

```ts
/**
 * These are all valid ways to evaluate boolean operations.
 */

Conditions.eval(true);
Conditions.eval(Conditions.AND(false, true));

Conditions.eval(Conditions.create(() => true));
Conditions.eval(Conditions.create((s) => (s === "Hello, world!")), "Hello, world!");

Conditions.eval((n: number) => (n > 0), 0);
```

### Conditions & Events

Conditions true power starts to reveal for itself when we use Events and their `.condition()` method, it's easier to wrap it around this statement to ensure that next-in-queue callback runs. This let us have conditioning done in one place without polluting core functionality with unnecessary if-statements.

```ts
interface RoundEvents {
    startRound: (timer: number) => void;
}

class RoundService extends EventEmitter<RoundEvents> {
    private readonly duration = 180;

    constructor() {
        super();

        this.when("startRound")
            .condition(
                Conditions.create((currentTime: number) => {
                    return currentTime >= this.duration;
                })
            )
            .do(() => {
                // If this code is executed, that means that the last condition check has passed,
                // that way, this code ensures that the timer is correct.

                ...; // Finish round, initialise next
            });
    }
}
```

### Methods available

Conditions not only are useful for evaluating boolean operations, but it provides you an API for those common boolean operations that you may find yourself doing over and over again, such as `AND`, `OR` and `NOT`, but others like `NAND`, `NOR`, `XNOR` are included as well.

#### AND

```ts
Conditions.eval(Conditions.AND(true, false)); // false
```

#### OR

```ts
Conditions.eval(Conditions.OR(true, false)); // true
```

#### NOT

```ts
Conditions.eval(Conditions.NOT(false)); // true
```

#### NOR

```ts
Conditions.eval(Conditions.NOR(true, false)); // false
```

#### NAND

```ts
Conditions.eval(Conditions.NAND(false, false)); // true
```

#### XNOR

```ts
Conditions.eval(Conditions.XNOR(true, true)); // true
```