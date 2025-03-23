# What is SLua?
SLua is a scripting language being developed by Linden Lab for Second Life. It's based on **Luau** (developed by Roblox), which is itself based on **Lua 5.1** with some features from later versions.

It's important to know that SLua scripts can only be run in-world in Second Life, you cannot run scripts from an avatar's inventory or your local computer.

# Getting started
Check the [Try SLua](https://wiki.secondlife.com/wiki/Try_SLua) page.

# Your first script
The default SLua script should look something like this:

```lua
function touch_start(total_number)
   ll.Say(0, "Touched.")
end

ll.Say(0, "Hello, Avatar!")
```

The first line of the above script declares a *function* named `touch_start` with one *parameter*. This function simply _calls_ another function named [ll.Say](https://wiki.secondlife.com/wiki/LlSay) with two parameters: chat channel and message. The function definition ends with the *keyword* `end`.

The code within `touch_start` is only executed when that function is _called_ somehow, which we'll cover next. On the other hand, the _function call_ on last line will be made immediately, because all code within the _global scope_ of a script is executed as soon as the script starts.

You should see "Hello, Avatar!" after saving the script, followed by "Touched." only after someone clicks the object.

# Events and handlers
Scripting in second life is *event-driven*, meaning that most scripts will react to *events* caused by their environment and may cause events themselves.

The default script's `touch_start` function is actually an *event handler* for a built-in event of the same name, which is triggered _(or called)_ when someone begins touching the object containing the script. There are many built-in events in Second Life, see [this page](https://wiki.secondlife.com/wiki/Category:LSL_Events/ID) for the full list!
