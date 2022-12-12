## :material-cube-scan: Condition

`Condition` is a type defined as:
```ts
export declare type Condition<T extends unknown[] = unknown[]> = ConditionCallback<T> | boolean;
export declare type ConditionCallback<T extends unknown[] = unknown[]> = (...args: [...T]) => boolean;
```
To simplify this, we can just say that a `Condition` is a `#!typescript (boolean) | ((...unknown) => boolean)`.

---

## :material-package-variant-closed: COND

### :material-function-variant: **`#!typescript create(evaluable: (...args: unknown[]) => boolean): Condition`** { #markdown data-toc-label='create' }

Turns a function into an evaluable condition.

#### Parameters
[`#!typescript a: Condition` :octicons-link-16:](#condition)

: The [`Condition`](#condition) you want to evaluate.

#### Returns

[`#!typescript Condition` :octicons-link-16:](#condition)

: The Resulting [`Condition`](#condition) callback.

??? tip "Psst, I've got a secret"

    The function is defined as:

    ```typescript linenums="6"
	public static create(callback: ConditionCallback) {
		return callback;
	}
    ```

---

### :material-function-variant: **`#!typescript eval(condition: Condition, ...vars: unknown[]): boolean`** { #markdown data-toc-label='eval' }

Evaluates a full condition and turns it into a boolean.

#### Parameters
[`#!typescript a: Condition` :octicons-link-16:](#condition)

: The [`Condition`](#condition) you want to evaluate.

`#!typescript ...vars: unknown[]`

: The variables that any of the conditions might accept, this one is sketchy, be warned.

#### Returns

[`#!typescript Condition` :octicons-link-16:](#condition)

: The Resulting [`Condition`](#condition) callback.

---

### :material-function-variant: **`#!typescript NOT(a: Condition): Condition`** { #markdown data-toc-label='NOT' }

#### Parameters
[`#!typescript a: Condition` :octicons-link-16:](#condition)

: The [`Condition`](#condition) that you want to perform this operation on.

#### Returns

[`#!typescript Condition` :octicons-link-16:](#condition)

: The Resulting [`Condition`](#condition) callback.

---

### :material-function-variant: **`#!typescript AND(a: Condition, b: Condition): Condition`** { #markdown data-toc-label='AND' }

#### Parameters
[`#!typescript a: Condition` :octicons-link-16:](#condition)

: The first [`Condition`](#condition) that you want to use for this operation.

[`#!typescript a: Condition` :octicons-link-16:](#condition)

: The second [`Condition`](#condition) that you want to use for this operation.

#### Returns

[`#!typescript Condition` :octicons-link-16:](#condition)

: The Resulting [`Condition`](#condition) callback.

---

### :material-function-variant: **`#!typescript OR(a: Condition, b: Condition): Condition`** { #markdown data-toc-label='OR' }

#### Parameters
[`#!typescript a: Condition` :octicons-link-16:](#condition)

: The first [`Condition`](#condition) that you want to use for this operation.

[`#!typescript a: Condition` :octicons-link-16:](#condition)

: The second [`Condition`](#condition) that you want to use for this operation.

#### Returns

[`#!typescript Condition` :octicons-link-16:](#condition)

: The Resulting [`Condition`](#condition) callback.

---

### :material-function-variant: **`#!typescript NAND(a: Condition, b: Condition): Condition`** { #markdown data-toc-label='NAND' }

#### Parameters
[`#!typescript a: Condition` :octicons-link-16:](#condition)

: The first [`Condition`](#condition) that you want to use for this operation.

[`#!typescript a: Condition` :octicons-link-16:](#condition)

: The second [`Condition`](#condition) that you want to use for this operation.

#### Returns

[`#!typescript Condition` :octicons-link-16:](#condition)

: The Resulting [`Condition`](#condition) callback.

---

### :material-function-variant: **`#!typescript NOR(a: Condition, b: Condition): Condition`** { #markdown data-toc-label='NOR' }

#### Parameters
[`#!typescript a: Condition` :octicons-link-16:](#condition)

: The first [`Condition`](#condition) that you want to use for this operation.

[`#!typescript a: Condition` :octicons-link-16:](#condition)

: The second [`Condition`](#condition) that you want to use for this operation.

#### Returns

[`#!typescript Condition` :octicons-link-16:](#condition)

: The Resulting [`Condition`](#condition) callback.

---

### :material-function-variant: **`#!typescript XOR(a: Condition, b: Condition): Condition`** { #markdown data-toc-label='XOR' }

#### Parameters
[`#!typescript a: Condition` :octicons-link-16:](#condition)

: The first [`Condition`](#condition) that you want to use for this operation.

[`#!typescript a: Condition` :octicons-link-16:](#condition)

: The second [`Condition`](#condition) that you want to use for this operation.

#### Returns

[`#!typescript Condition` :octicons-link-16:](#condition)

: The Resulting [`Condition`](#condition) callback.

---

### :material-function-variant: **`#!typescript XNOR(a: Condition, b: Condition): Condition`** { #markdown data-toc-label='XNOR' }

#### Parameters
[`#!typescript a: Condition` :octicons-link-16:](#condition)

: The first [`Condition`](#condition) that you want to use for this operation.

[`#!typescript a: Condition` :octicons-link-16:](#condition)

: The second [`Condition`](#condition) that you want to use for this operation.

#### Returns

[`#!typescript Condition` :octicons-link-16:](#condition)

: The Resulting [`Condition`](#condition) callback.

---






