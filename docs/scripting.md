
# Scripting

The simulation offers the option to write custom scripts which can interact
dynamically with the 3d visualization of the calculator. The [Lua](https://www.lua.org/home.html)
scripting language was chosen due to its lightweight nature, simple syntax
and shallow learning curve. Additionally, it is very simple to integrate the
language into others while allowing to hide nasty details about the underlying
architecture through abstraction.

## Lua basics

## Limitations

## Brunsviga 13 RK API

The following is compilation of all the functions provided by the `api` module
used to interact with the 3d visualization. This chapter is structured logically
by register interaction and utility functions.

The source of the Lua module can be found at:
[api.lua](https://github.com/brunsviga13rk/emulator/blob/main/src/api/api.lua).
For all the given example usages below it is assumed the API has been setup
by creating a local reference to the API module:

```lua
local api = require'api'

-- Do something with the API.
```

### Input register

#### Set a value

Sets the input registers to have a specified value. Waits for the animation of the
3d visualization to complete. The input value is saturated towards the minimum
and maximum bounds of the input registers value range. No indicator is provided
whether the given value is saturated by the underlying API calls.
Example usage:

``` lua
-- Load the number 567 to input register.
api.load(567)
```

#### Reset

Sets the input registers value to zero. Waits for the animation of the
3d visualization to complete. Example usage:

``` lua
api.reset_input()
```

#### Get current value

Returns the current value of the input register as displayed by the sprocket
wheel. This function does not account for negative numbers displayed in
complement. Returns immediately after retrieving the desired value.

``` lua
local input_register_value = api.get_input()
```

### Counter register

#### Reset

Sets the counter registers value to zero. Waits for the animation of the
3d visualization to complete. Example usage:

``` lua
api.reset_counter()
```

#### Get current value

Returns the current value of the counter register as displayed by the sprocket
wheel. This function does not account for negative numbers displayed in
complement. Returns immediately after retrieving the desired value.

``` lua
local counter_register_value = api.get_counter()
```

### Result register

#### Reset

Sets the result registers value to zero. Waits for the animation of the
3d visualization to complete. Example usage:

``` lua
api.reset_result()
```

#### Get current value

Returns the current value of the result register as displayed by the sprocket
wheel. This function does not account for negative numbers displayed in
complement. Returns immediately after retrieving the desired value.

``` lua
local result_register_value = api.get_result()
```

### Operators

#### Addition

Performs a single addition. This will cause the operation crank to rotate
clock-wise by 360°. After completing the input registers value will have been
added onto the result register.
Waits for the animation of the 3d visualization to complete. Example usage:

``` lua
api.add()
```

#### Subtraction

Performs a single subtraction. This will cause the operation crank to rotate
counter clock-wise by 360°. After completing the input registers value will have been
subtracted from the result register.
Waits for the animation of the 3d visualization to complete. Example usage:

``` lua
api.subtract()
```

#### Shift result register left

Shifts the sled housing the result register by one digit to the left.
Waits for the animation of the 3d visualization to complete. Example usage:
This has the effect of all subsequent arithmetic operations being shifted by
one additional digits to the right.

``` lua
api.shift_left()
```

#### Shift result register right

Shifts the sled housing the result register by one digit to the right.
Waits for the animation of the 3d visualization to complete. Example usage:
This has the effect of all subsequent arithmetic operations being shifted by
one additional digits to the left.

``` lua
api.shift_right()
```

### Utilities

The following are functions providing quality of life utility functionalities.

#### Reset all registers

Sets all three registers of the machine to zero. Waits for the animation of the
3d visualization to complete. Example usage:

``` lua
api.reset()
```

## Typescript API

The API designed to be used by the end user is not the API interfacing directly
with the simulation. Instead, it is a wrapper written in Lua around a class
interface written in Typescript. This serves the purpose of only letting
the user access a clean language native API with well-defined behavior.
It also removes the need to call `:await()` manually on all the TS API
since these are asynchronous by design and waiting for their completion
guarantees a functional state.

!!! danger "Breaking state"

    It is not recommended to use this API as incorrect usage may lead to a broken
    state of the 3d visualization. Due to this no documentation for this API
    is provided on this page.

However, the raw typescript API can still be accessed from within Lua.
The Module can be accessed by calling the according get function which
is defined for the API version currently used in the build, which as of now
is v1.4.1:

```lua
local raw_ts_api = ts_api_get_1_4_1()
```
