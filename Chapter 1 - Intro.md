# What is SLua?
SLua is a scripting language being developed by Linden Lab for Second Life. It's based on **Luau** (developed by Roblox), which is itself based on **Lua 5.1** with some features from later versions.

It's important to know that SLua scripts can only be run in-world in Second Life, you cannot run scripts from an avatar's inventory or your local computer.

# Getting started
Before anything else, find a sandbox or another area where building is allowed. Once you've rezzed a new object, edit its contents and click the **New Script** button. This will add a premade script into the object's contents, which you can edit and observe right away.

# Your first script
The default SLua script should look something like this:

```lua
function touch_start(NumberOfTouches)
   ll.Say(0, "Touched.")
end

ll.Say(0, "Hello, Avatar!")
```

The first line of the above script declares a *function* called touch_start with one *parameter*. This function simply calls another function called `ll.Say`, which prints a message in public chat (channel 0). The function definition ends with the *keyword* `end`.

The code within `touch_start` is only executed when the function is *called* somehow, which we'll cover next. On the other hand, the last line will be executed as soon as the script starts. So you will see "Hello, Avatar!" after saving the script, followed by "Touched." only after someone clicks the object.

# Events and handlers
Scripting in second life is *event-driven*, meaning that most scripts will react to *events* caused by their environment and may cause events themselves.

The default script has a function called `touch_start`, which is actually an *event handler* for an event of the same name, which is triggered *(or called)* when someone begins touching the object containing the script. There are many built-in events in Second Life, see {this page} for the full list!
