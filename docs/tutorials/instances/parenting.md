The `[Children]` key allows you to add children when hydrating or creating an
instance.

It accepts instances, arrays of children and state objects containing children.

```luau
local Children = Weave.Children
local folder = New "Folder" {
    [Children] = {
        New "Part" {
            Name = "Gregory",
            Color = Color3.new(1, 0, 0)
        },
        New "Part" {
            Name = "Sammy",
            Material = "Glass"
        }
    }
}
```

---

## Usage

To use `Children` in your code, you first need to import it from the Fusion
module, so that you can refer to it by name:

```luau linenums="1"
local Weave = require(ReplicatedStorage.Weave)
local Children = Weave.Children
```

When using `New` or `Hydrate`, you can use `[Children]` as a key in the property
table. Any instance you pass in will be parented:

```luau
local folder = New "Folder" {
    -- The part will be moved inside of the folder
    [Children] = workspace.Part
}
```

Since `New` and `Hydrate` both return their instances, you can nest them:

```luau
-- Makes a Folder, containing a part called Gregory
local folder = New "Folder" {
    [Children] = New "Part" {
        Name = "Gregory",
        Color = Color3.new(1, 0, 0)
    }
}
```

If you need to parent multiple children, arrays of children are accepted:

```luau
-- Makes a Folder, containing parts called Gregory and Sammy
local folder = New "Folder" {
    [Children] = {
        New "Part" {
            Name = "Gregory",
            Color = Color3.new(1, 0, 0)
        },
        New "Part" {
            Name = "Sammy",
            Material = "Glass"
        }
    }
}
```

Arrays can be nested to any depth; all children will still be parented:

```luau
local folder = New "Folder" {
    [Children] = {
        {
            {
                {
                    New "Part" {
                        Name = "Gregory",
                        Color = Color3.new(1, 0, 0)
                    }
                }
            }
        }
    }
}
```

Similarly, state objects containing children (or `nil`) are also allowed:

```luau
local value = Value.new()

local folder = New "Folder" {
    [Children] = value
}

value:set(
    New "Part" {
        Name = "Clyde",
        Transparency = 0.5
    }
)
```

You may use any combination of these to parent whichever children you need:

```luau
local modelChildren = workspace.Model:GetChildren()
local includeModel = Value.new(true)

local folder = New "Folder" {
    -- array of children
    [Children] = {
        -- single instance
        New "Part" {
            Name = "Gregory",
            Color = Color3.new(1, 0, 0)
        },
        -- state object containing children (or nil)
        Computed(function()
            return if includeModel:get()
                then modelChildren:GetChildren() -- array of children
                else nil
        end)
    }
}
```
