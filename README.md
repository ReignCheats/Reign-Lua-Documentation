
# Table of contents
- [Filesystem](#Filesystem)
- [Memory](#Memory)
- [Menu](#Menu)
- [Native Invoker](#Native Invoker)
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
void memory.write_int(userdata address, int value)
```
Writes a 32-bit integer to the givena address.
```lua
void memory.write_float(userdata address)
```

```lua
void memory.write_byte(userdata address)
```
Writes a 8-bit integer to the givena address.

# Menu

# Native Invoker
```lua
int native_invoker.invoke_int(Hash hash, Any ...args)


-- Example
const hash = 0xD80958FC74E988A6 -- PLAYER_PED_ID
let player_ped_id = native_invoker.invoke_int(hash);
```
Invokes a native and returns its return as type int, ...args is the arguments you supply to the native.
```lua
float native_invoker.invoke_float(Hash hash, Any ...args)
```

```lua
void native_invoker.invoke_void(Hash hash, Any ...args)
```

```lua
bool native_invoker.invoke_bool(Hash hash, Any ...args)
```

```lua
Vector3 native_invoker.invoke_vector3(Hash hash, Any ...args)
```

```lua
string native_invoker.invoke_string(Hash hash, Any ...args)
```
# Script

# Util 

# Types
