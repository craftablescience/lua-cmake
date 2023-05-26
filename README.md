# lua-cmake
Download lua at cmake build time and add it as a library. Loosely based on https://github.com/lubgr/lua-cmake.

## Usage
Add the following to your cmake file:

```cmake
add_subdirectory(path/to/lua-cmake)
target_link_libraries(your-target <access modifier> lua::lua)
```
