
# Table of contents
- [Introduction](#Introduction)
- [Libraries](#Libraries)
- [Types](#Types)
- [Filesystem](#Filesystem)
- [Memory](#Memory)
- [Menu](#Menu)
- [Native Invoker](#Native-Invoker)
- [Script](#Script)
- [Util](#Util)
- [Resources](#Resources)

# Introduction
First of all, thank you for taking interest in Reign's scripting API. Let's get into some details about the API. Reign uses Pluto as its scripting language of choice. It introduces a bunch of welcome additions and improvements to Lua which make the language a lot more interesting. You can find more info regarding Pluto [here](https://pluto-lang.org/docs/Introduction). If you're more comfortable with stock Lua, that's not a problem, you don't *need* to use Pluto syntax.

# Libraries
Reign loads the following libraries:

- base
- string
- math
- table
- os
- coroutine
- utf8
- bit32

Due to security concerns, certain library functions are disabled.

# Types

- **Vector3**
  - A table with x, y, z properties.

- **eNotificationType**
  - An enum representing notification states
    * NOTIFY_DEFAULT
    * NOTIFY_ERROR
    * NOTIFY_SUCCESS
    * NOTIFY_WARNING

# Filesystem
```lua
string filesystem.scripts_dir()
```
Example return value: C:\Users\User\AppData\Roaming\EquinoxASI\Lua\

```lua
string filesystem.appdata_dir()
```
Example return value: C:\Users\User\AppData\Roaming\

```lua
bool filesystem.exists(string path)
```

```lua
bool filesystem.mkdir(string path)
```

```lua
table<string> filesystem.list_files(string path)

-- Example
for filesystem.list_files(filesystem.scripts_dir()) as path do
    util.toast("Files", path);
end
```
Returns a table with the absolute paths of all listed files.

```lua
string filesystem.get_extension(string path)
```
Gets the file extension of the specified path.


# Memory
```lua
userdata memory.alloc(int size = 8)
```
Returns a block of memory that is 8 bytes by default, unless a different size is explicitly provided.

```lua
void memory.free(userdata address)
```
Frees the allocated block of memory.

```lua
int memory.read_int(userdata address)
```
Reads a 32-bit integer at the given address.

```lua
float memory.read_float(userdata address)
```
```lua
int memory.read_byte(userdata address)
```
Reads an 8-bit integer at the given address.

```lua
string memory.read_string(userdata address)
```

```lua
void memory.write_int(userdata address, int value)
```
Writes a 32-bit integer to the givena address.
```lua
void memory.write_float(userdata address, float value)
```

```lua
void memory.write_byte(userdata address, int value)
```
Writes a 8-bit integer to the givena address.

```lua
void memory.write_string(userdata address, string value)
```

# Menu

# Native Invoker

```lua
void native_invoker.init(int nativeHash)
```
Must be called at the start of every native call.

```lua
void native_invoker.push_arg_pointer(userdata arg)
void native_invoker.push_arg_int(int arg)
void native_invoker.push_arg_float(float arg)
void native_invoker.push_arg_bool(bool arg)
void native_invoker.push_arg_string(string arg)
```
These functions are used to push arguments to the native being called, always called after native_invoker.init.

```lua
int invoke_int()
float invoke_float()
string invoke_string()
bool invoke_bool()
void invoke_void()
Vector3 invoke_vector3()

-- Example
local function DrawRect(x, y, width, height, r, g, b, a)     
    native_invoker.init(0x3A618A217E5154F0)
    native_invoker.push_arg_float(x)
    native_invoker.push_arg_float(y)
    native_invoker.push_arg_float(width)
    native_invoker.push_arg_float(height)
    native_invoker.push_arg_int(r)
    native_invoker.push_arg_int(g)
    native_invoker.push_arg_int(b)
    native_invoker.push_arg_int(a)
    native_invoker.push_arg_int(0)
    native_invoker.invoke_void() -- DrawRect doesn't return anything, if it was a native like PLAYER_PED_ID, we would use invoke_int and return it.
end

while(true) do
  DrawRect(0.5, 0.5, 0.5, 0.5, 255, 255, 255, 255)
  util.yield()
end

```
Natives can be manually invoked like the above, or you can get natives.lua from [here](https://github.com/ReignCheats/ReignNatives) and put it in the ReignASI folder, it will be automatically required in Lua scripts, or you can require it manually.

# Script

```lua
void script.create_fiber(string name, function callback)

-- Example
-- Will draw a white rectangle on tick.
script.create_fiber("ExampleFiber", function()
    while(true) do
        Graphics.DrawRect(0.5, 0.5, 0.5, 0.5, 255, 255, 255, 255, 0)
        util.yield()
    end
end)
```

Registers a new fiber in which natives can be executed, similiar to the fiber your script is operating in. Take note that this fiber is destroyed when it's not in use. To prevent this from happening, run a while loop inside the callback. Remember to use util.yield() when doing so.

# Util 

```lua
int util.current_time_millis()
```
Returns the current OS time in milliseconds.

```lua
void util.yield(int ms = 0)
```
Yields the current thread until the next tick, or the specified ms value. Has to be called when using infinite loops.

```lua
bool util.is_key_pressed(int vk)
```
Returns true if the virtual key was pressed. See https://docs.microsoft.com/en-us/windows/win32/inputdev/virtual-key-codes

```lua
int util.stat_get_int(Hash hash)
```
Wrapper around STAT_GET_INT, internally retrieves the value from memory and returns it.

```lua
float util.stat_get_float(Hash hash)
```

```lua
bool util.stat_get_bool(Hash hash)
```

```lua
bool util.register_file(path)

-- Example
if(util.register_file(filesystem.scripts_dir() .. "Reign.ytd")) then
    script.create_fiber("YTD", function()
        while(true) do
            Graphics.DrawSprite("Reign", "Header", 0.5, 0.5, 0.5, 0.5, 0, 255, 255, 255, 255, 0, 0)
            util.yield()
        end
    end)
end
```
Registers the specified YTD file so that it can be used with natives, such as GRAPHICS::DRAW_SPRITE.

```lua
string util.get_clipboard_text()
```
Returns the string currently allocated in the user's clipboard.

```lua
void util.set_clipboard_text(string text)
```
Sets the string currently allocated in the user's clipboard.

```lua
int util.joaat(string text)
```
Returns a JOAAT hash, used for a variety of things in GTA, such as entity models.

```lua
void util.toast(string title, string body, eNotificationType type = NOTIFY_DEFAULT)
```
Registers a new notification to be displayed.

```lua
string util.get_keyboard_input(string title, int max_chars = 8, string placeholder = "")
```
Brings up the game's input box, returns whatever the user submitted.

```lua
bool, string util.get_input(string title, int min, int max)
```
Brings up the internal input box used by Reign. 

# Resources
- [Pluto](https://pluto-lang.org/docs/Introduction)
- [Natives](https://gta5.nativedb.dotindustries.dev/natives/)
- [Learning Lua](https://www.youtube.com/watch?v=iMacxZQMPXs)
- [Useful lists](https://forge.plebmasters.de/)
- [Lua Website](https://lua.org/)
