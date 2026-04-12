# ObjectPool

A typed, free-list object pool for `BasePart` instances in Roblox. Reduces clone/destroy overhead by reusing objects across their lifetime.

---

## Installation

Copy `ObjectPool.luau` into your project and require it:

```lua
local ObjectPool = require(path.to.ObjectPool)
```

---

## Quick Start

```lua
local part = workspace.MyPart
local pool = ObjectPool.new(part, 20)

local obj = pool:GetObject()
obj.Position = Vector3.new(0, 10, 0)

-- later
pool:ReturnObject(obj)
```

---

## API

### `ObjectPool.new(object: BasePart, max_limit: number)`

Creates a new pool and pre-fills it asynchronously up to `max_limit`.

```lua
local pool = ObjectPool.new(workspace.Bullet, 50)
```

- `object` — the `BasePart` template to clone. Must be a `BasePart`.
- `max_limit` — maximum pool capacity. Must be a positive integer.

---

### `pool:GetObject(): BasePart`

Returns a free object from the pool. If none are available, creates and returns a new one.

```lua
local obj = pool:GetObject()
```

---

### `pool:ReturnObject(object: BasePart)`

Returns an object back to the pool. If the pool is at capacity, the object is destroyed instead.

```lua
pool:ReturnObject(obj)
```

- Panics if `object` does not belong to this pool.

---

### `pool:CreateObject(object: BasePart): BasePart`

Manually clones and registers a new object into the pool.

```lua
local extra = pool:CreateObject(workspace.Bullet)
```

- Useful when you need to pre-warm beyond the initial fill.

---

### `pool:GetSize(): number`

Returns the current number of objects tracked by the pool, after reconciling any size overflows.

```lua
print(pool:GetSize()) -- e.g. 20
```

---

### `pool:Cleanup()`

Destroys all pooled objects and marks the pool as destroyed. All subsequent method calls will panic.

```lua
pool:Cleanup()
```

---

## Error Reference

| Message | Cause |
|---|---|
| `Presented object template is not a BasePart!` | Passed a non-`BasePart` as the template |
| `SizeLimit must be a positive integer!` | `max_limit` is zero, negative, or fractional |
| `Returned object does not belong to this pool!` | `ReturnObject` called with a foreign object |
| `Cannot call X on a destroyed pool!` | Method called after `Cleanup()` |
| `Pool is already destroyed!` | `Cleanup()` called twice |

---

## Notes

- Objects are moved to `(0, -10000, 0)` when idle, not unparented.
- Pool pre-fill runs on a separate thread via `task.spawn` — `GetObject` may create new objects if called before fill completes.
- Free-list selection is O(1). `ReturnObject` ownership check is O(n).
