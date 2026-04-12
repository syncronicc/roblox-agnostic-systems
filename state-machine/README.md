# StateMachine

A lightweight finite state machine for Roblox, written in Luau.

---

## Installation

Drop `StateMachine.luau` into your project and require it.

```luau
local StateMachine = require(path.to.StateMachine)
```

---

## State Modules

Each state is a ModuleScript that returns an object implementing three methods:

```luau
local State = {}
State.__index = State

function State:Startup()
    -- called once on machine construction
end

function State:Entered()
    -- called when transitioning into this state
end

function State:Exited()
    -- called when transitioning out of this state
end

return State
```

---

## Usage

### Creating a machine

```luau
local states = StateMachine.LoadDirectory(workspace.States)
local machine = StateMachine.new(states, "Idle")
```

`LoadDirectory` requires all `ModuleScript` children of the given `Folder` and validates that each implements `:Startup()`, `:Entered()`, and `:Exited()`. Invalid modules are skipped with a warning.

`StateMachine.new` calls `:Startup()` on every state, then calls `:Entered()` on the default state.

---

### Adding transitions

```luau
-- named transition
machine:AddTransition("IdleToRun", "Idle", "Run", function(speed)
    return speed > 0
end)

-- anonymous transition (auto-generates a GUID key)
machine:AddTransition(nil, "Run", "Idle", function(speed)
    return speed == 0
end)
```

`condition` receives whatever arguments are passed to `:Update(...)`.

---

### Updating the machine

```luau
RunService.Heartbeat:Connect(function()
    local speed = humanoid.WalkSpeed
    machine:Update(speed)
end)
```

`:Update` iterates all transitions, fires the first one whose `from` matches the current state and whose condition returns `true`, then returns `true`. Returns `nil` if no transition fired.

---

### Removing a transition

```luau
machine:RemoveTransition("IdleToRun")
```

---

### Reading the current state

```luau
local state = machine:GetState() -- returns string
```

---

## Types

```luau
export type StateObject = {
    Startup: (self: StateObject) -> (),
    Entered: (self: StateObject) -> (),
    Exited:  (self: StateObject) -> (),
}

export type TransitionRule = (...any) -> boolean

export type StateMachineObject = {
    CurrentState:      string,
    States:            { [string]: StateObject },
    ChangeState:       (self: StateMachineObject, state: string) -> (),
    GetState:          (self: StateMachineObject) -> string,
    AddTransition:     (self: StateMachineObject, id: string?, base_state: string, next_state: string, rule: TransitionRule) -> (),
    RemoveTransition:  (self: StateMachineObject, id: string) -> (),
    Update:            (self: StateMachineObject, ...any) -> boolean?,
    LoadDirectory:     (directory: Folder) -> { [string]: StateObject },
}
```

---

## License

MIT — kts
