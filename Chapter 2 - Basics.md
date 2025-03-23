# Types
SLua has a handful of built-in types, many of them are inherited from Luau:
| name | value | description | origin |
| --- | --- | --- | --- |
| nil | `nil` | Represents "nothing" or "unset" | Luau |
| boolean | `true`, `false` | Result of logical expression | Luau |
| number | `1`, `3.14` | 64-bit float | Luau |
| integer | `integer(1)`, `integer(3.14)` | Whole numbers, always floored | SLua |
| string | `"text!"` | UTF-8 text, supports [interpolation](https://luau.org/syntax#string-interpolation) | Luau |
| uuid | `uuid("12345678-1234-1234-1234-123456789ABC")` | see [LSL Key](https://wiki.secondlife.com/wiki/Category:LSL_Key) | SLua |
| rotation | `rotation(x, y, z, s)` | see [LSL Rotation](https://wiki.secondlife.com/wiki/Rotation) | SLua |
| vector | `vector(x, y, z)` | see [documentation](https://luau.org/library#vector-library) and [LSL Vector](https://wiki.secondlife.com/wiki/Category:LSL_Vector) | Luau |
| function | any _function_ | see [documentation](https://luau.org/typecheck#function-types) | Luau |
| thread | any _coroutine_ | see [documentation](https://luau.org/library#coroutine-library) | Luau |
| buffer | `buffer.create(size)` | see [documentation](https://luau.org/library#buffer-library) | Luau |
| userdata | arbitrary C/C++ data | see [documentation](https://www.lua.org/pil/28.html) | Luau |
| table | `{true, pi=3.14, "text!"}` | Key-Value storage for mixed types, including itself | Luau |

# Syntax
You may refer to Luau documentation for more details: https://luau.org/syntax

## Comments
Comments in code aren't executed, their primary purpose is to clarify the meaning of our code. `--` begins a comment and continues until end of line:

```lua
-- error("No code is executed here!")
error("This causes the script to shout an error.") -- no lie
```

It's also possible to write a comment that spans multiple lines (or only part of a line) by surrounding the contents with `--[[` and `]]`:

```lua
--[[
    This is a multiline comment which
    you might expect in other languages.
]]

--[[The rest of this line still executes.]] error("This causes the script to shout an error.")
```

## Statements
A statement is a single logical action written by the programmer, such as _assigning a value_, or _calling a function_.
```lua
A = 2 -- assigning a value
ll.OwnerSay("Hello, Avatar!") -- calling a function
```

If you have experience with other programming languages, such as LSL, you'll notice that there were no semicolons in the above example.

Semicolons are optional in SLua, although their use might be needed in very specific cases like single-line if-statements. Generally speaking, it's best to avoid writing such code that needs semicolons because multiple statements on a single line are harder to read.

## Variables

Any value can be stored in _variables_ to be reused later.

```lua
A = 2               -- number
B = 3               -- number
C = A + B           -- number value from expression
text = tostring(C)  -- string value from expression (function call)
ll.OwnerSay(text)   -- prints "5" in your chat window
```

## Functions
A function is a reusable block of code that can accept inputs, perform tasks, and return results. Their primary purpose is to organize and structure our code.

There are two styles of function definition:
```lua
-- Named function definition
function SomeTask (OptionalParameter, AnotherParameter)
    -- function body
end
```
```lua
-- Anonymous function definition (i.e. no name was specified in the definition)
SomeTask = function (OptionalParameter, AnotherParameter)
    -- function body
end
```
Both of the above examples achieve the same result: a new variable named `SomeTask` is created, with its value being a function.

And when _calling_ a function, parentheses are used to pass any number of arguments to the function:
```lua
-- the first argument 42 will be assigned to the parameter named OptionalParameter
-- the second argument "life" will be assigned to AnotherParameter
SomeTask(42, "life")
```

The anonymous function definition can be useful when dealing with functions that accept other functions as parameters:
```lua
string.gsub(                                -- function call
    "hello, world",                         -- first argument, source string
    "%a+",                                  -- second argument, string pattern to find
    function (match) ll.OwnerSay(match) end -- anonymous function to call whenever match is found
)
```
Alternatively, with a named function:
```lua
function callback (match)
    ll.OwnerSay(match)
end

string.gsub("hello, world", "%a+", callback)
```

### Global variables
By default, all variables are global, meaning they can be accessed from anywhere in the script.

```lua
touches = 0 -- global variable always stays in memory

function touch_start (NumberOfTouches)
    touches += 1  -- increments global variable by 1
    ll.OwnerSay(tostring(touches)) -- prints 1, then 2, then 3, ...
end
```

### Local variables
We can limit the visibility _(or scope)_ of a variable with the `local` keyword.

```lua
touches = 0

function touch_start (NumberOfTouches)
    local touches = 42 -- set local to 42
    ll.OwnerSay(tostring(touches)) -- always prints 42
end
```

Another term for the above is _shadowing_, because the most-local variable name will temporarily override ("put in shadow") the previous variable. Once the script's execution returns to a higher _scope_, the previous variable is usable again with its correct value.

Shadowing may be useful in certain situations where you want to prevent a previous variable from being modified until you're finished with another (inner) task. The following example is a demonstration of how shadowing affects a script, but it isn't a very practical example.

```lua
local i = 10    -- local or global variable already has a value
for i = 1, 3 do -- new local variable `i` shadowing previous `i`
    ll.OwnerSay(tostring(i)) -- prints 1, 2, 3
end                          -- shadowing ends
ll.OwnerSay(tostring(i))     -- prints 10 (the previous `i` value)
```

## Control flow

### if statement

SLua uses an `if ... then ... elseif ... else ... end` structure.

When the initial condition is false, there may be zero or more additional `elseif` conditions, and lastly a single `else` condition which will always be chosen if no other condition matches.

```lua
value = nil

-- minimal `if` statement
if not value then
    ll.OwnerSay("value is false or nil!")
end
```
```lua
-- any amount of `elseif` allowed
if value == nil then
    ll.OwnerSay("value is nil!")
elseif value == false then
    ll.OwnerSay("value is false")
elseif value == "" then
    ll.OwnerSay("value is empty string")
end
```
```lua
-- only one `else` allowed
if not value then
    ll.OwnerSay("value is false or nil!")
elseif value == "" then
    ll.OwnerSay("value is empty string")
else
    ll.OwnerSay("value is " .. tostring(value))
end
```

### if expression
SLua also supports an `if ... then ... else` _expression_ as well. Pay attention to the difference between *if-statement* and *if-expression:*

The expression returns a value directly and doesn't use the keyword `end`. This kind of expression can be used assignments, function calls, and loops.

```lua
ll.OwnerSay(if value then value else "default")
-- prints "default" because value doesn't exist
```
```lua
value = "text"
local valueOrDefault = if value then value else "default"
ll.OwnerSay("value is " .. valueOrDefault)
-- prints "text" because value is truthy
```

## Loops
Loops are similar to the if-statement, except they can repeatedly execute the same code based on the condition. This is called _iteration_.

All loops support the `break` keyword, which causes the loop to end immediately, without executing more code or checking the condition.

All loops support the `continue` keyword, which causes the loop to move onto the next _iteration_ and handling the condition as usual.

### while loop
Constructured like `while ... do ... end`, the loop first checks a condition, then executes all of the code in the `do` _block_ before checking the condition again.

```lua
while ll.GetTime() < 5 do -- total script runtime is less than 5 seconds
    ll.OwnerSay("iteration")
    ll.Sleep(0.5)
end
```
### repeat-until loop
Constructed like `repeat ... until ...`, the loop first executes all of the code in the `repeat` block. If the `until` condition evaluates to `false`, the loop repeats again.

This type of loop is useful when you want to execute code **at least** once, even if the condition is fulfilled before the loop.

```lua
repeat
    ll.OwnerSay("iteration")
    ll.Sleep(0.5)
until ll.GetTime() >= 5 -- total script runtime is at least 5 seconds
```

### numeric for loop
Constructed like `for ..., ..., ... do ... end`, this loop provides _initialization_, _condition_, and _step_ per iteration.

```lua
-- set countdown to 3
-- after each iteration, compare countdown to 1
-- if they aren't equal, modify countdown's value by -1
for countdown = 3, 1, -1 do
    ll.OwnerSay(tostring(countdown))
    ll.Sleep(1)
end

ll.OwnerSay("Lift-off!")
```

Note that `countdown` is created as a new local variable, _shadowing_ any previous variables with the same name.

All local variables created during the loop are deleted when the loop ends. Reading them after the loop gives `nil`.

### generic for loop
Constructed like `for ... in ... do ... end`, this loop iterates through values provided by an _iterator function_.

```lua
translation = {
    monday = "maanantai",
    tuesday = "tiistai",
    wednesday = "keskiviikko",
    thursday = "torstai",
    friday = "perjantai",
    saturday = "lauantai",
    sunday = "sunnuntai"
}

for k, v in pairs(translation) do
    ll.OwnerSay(`key: {k}, value: {v}`)
end
```
In this example, the `pairs` function returns two values when called: a table entry's _key_ and corresponding _value_. (You may notice something odd when trying the above example. This will be covered later when talking about tables.)
