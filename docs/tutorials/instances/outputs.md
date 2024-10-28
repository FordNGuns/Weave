`Out` is a function that returns keys to use when hydrating or creating an
instance. Those keys let you output a property's value to a `Value` object.

```luau
local name = Value.new()

local thing = New "Part" {
    [Out "Name"] = name
}

print(name:get()) --> Part

thing.Name = "Jimmy"
print(name:get()) --> Jimmy
```

---

## Usage

To use `Out` in your code, you first need to import it from the Fusion module,
so that you can refer to it by name:

```luau linenums="1"
local Weave = require(ReplicatedStorage.Weave)
local Out = Weave.Out
```

When you call `Out` with a property name, it will return a special key:

```luau
local key = Out("Activated")
```

When used in a property table, you can pass in a `Value` object. It will be set
to the value of the property, and when the property changes, it will be set to
the new value:

```luau
local name = Value.new()

local thing = New "Part" {
    [Out("Name")] = name
}

print(name:get()) --> Part

thing.Name = "Jimmy"
print(name:get()) --> Jimmy
```

If you're using quotes `'' ""` for the event name, the extra parentheses `()`
are optional:

```luau
local thing = New "Part" {
    [Out "Name"] = name
}
```

---

## Two-Way Binding

By default, `Out` only _outputs_ changes to the property. If you set the value
to something else, the property remains the same:

```luau
local name = Value.new()

local thing = New "Part" {
    [Out "Name"] = name -- When `thing.Name` changes, set `name`
}

print(thing.Name, name:get()) --> Part Part
name:set("NewName")
task.wait()
print(thing.Name, name:get()) --> Part NewName
```

If you want the value to both _change_ and _be changed_ by the property, you
need to explicitly say so:

```luau hl_lines="4 11"
local name = Value.new()

local thing = New "Part" {
    Name = name -- When `name` changes, set `thing.Name`
    [Out "Name"] = name -- When `thing.Name` changes, set `name`
}

print(thing.Name, name:get()) --> Part Part
name:set("NewName")
task.wait()
print(thing.Name, name:get()) --> NewName NewName
```

This is known as two-way binding. Most of the time you won't need it, but it can
come in handy when working with some kinds of UI - for example, a text box that
users can write into, but which can also be modified by your scripts.
