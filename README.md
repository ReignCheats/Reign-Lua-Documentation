
# Table of contents
- [Filesystem](#Filesystem)

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



# Memory

# Menu

# Native Invoker

# Script

# Util 

# Types
