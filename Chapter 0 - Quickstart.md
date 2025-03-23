## Requirements
1. You already know how programming works.
2. You're already fluent in LSL and _preferably_ at least one other language.
3. You're resourceful and know how to find more information on your own.

Here are some quick stumbling blocks to be aware of, so you can start on your blind rush into SLua.

## Pointers
Most relevant documentation can be found within these links:
- Luau documentation: https://luau.org/getting-started
- Lua 5.1 documentation: https://www.lua.org/manual/5.1/
- Compiler source code: https://github.com/luau-lang/luau/tree/master/VM/src
    - The SLua compiler will be open-sourced later.
- Definitions for IDE: https://github.com/WolfGangS/sl_lua_types

## Stumbling blocks
- Typecasting is done through functions `tostring(anything)` and `tonumber(string, base)`
    - Typecasting between other types (like `uuid` to `integer`) is not implemented
- Values of some types must be created through functions, like `vector(x, y, z)`
    - LSL-style `<x, y, z>` syntax is not supported
- In addition to Luau's `number`, SLua provides `integer` for compatibility with LSL functions
    - Luau's `number` is a 64-bit float. It can exactly represent integers up to 2^53 (9007199254740992)
- `vector` and `rotation` are immutable
    - Technically, they were immutable in LSL too, but `vec.x = 10` would transparently create a new copy of `vec` with `x` set to the new value, and replace the original. In Luau this must be done explicitly, because the members are read-only.
- LSL `key` is now called `uuid`
