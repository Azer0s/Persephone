# 💃🏻🍃 Persephone

## Variable declaration

`v_datatype variable_name` 

All variables in Persephone are signed

### Commands:

|       Datatype       |   Command   |
| :------------------: | :---------: |
|     8bit integer     |  `v_int8`   |
|    16bit integer     |  `v_int16`  |
|    32bit integer     |  `v_int32`  |
|    32bit integer     |   `v_int`   |
|    64bit integer     |  `v_int64`  |
| 32bit floating point | `v_float32` |
| 32bit floating point |  `v_float`  |
| 64bit floating point | `v_float64` |
| 64bit floating point | `v_double`  |
|     ASCII string     | `v_stringa` |
|    Unicode string    | `v_stringu` |
|         Bit          |   `v_bit`   |
|   Dynamic variable   |   `v_dyn`   |
|       Pointer        |   `v_ptr`   |

## Constant declaration

`dcdatatype value`

### Commands:

|       Datatype       | Command |
| :------------------: | :-----: |
|     8bit integer     | `dci8`  |
|    16bit integer     | `dci16` |
|    32bit integer     | `dci32` |
|    32bit integer     |  `dci`  |
|    64bit integer     | `dci64` |
| 32bit floating point | `dcf32` |
| 32bit floating point |  `dcf`  |
| 64bit floating point | `dcf64` |
| 64bit floating point |  `dcd`  |
|     ASCII string     | `dcsa`  |
|    Unicode string    | `dcsu`  |
|         Bit          |  `dcb`  |

## Load values from variable

`lddatatypev variable_name`

### Commands:

|       Datatype       | Command  |
| :------------------: | :------: |
|     8bit integer     | `ldi8v`  |
|    16bit integer     | `ldi16v` |
|    32bit integer     | `ldi32v` |
|    32bit integer     |  `ldiv`  |
|    64bit integer     | `ldi64v` |
| 32bit floating point | `ldf32v` |
| 32bit floating point |  `ldfv`  |
| 64bit floating point | `ldf64v` |
| 64bit floating point |  `lddv`  |
|     ASCII string     | `ldsav`  |
|    Unicode string    | `ldsuv`  |
|         Bit          |  `ldbv`  |
|       Dynamic        | `lddyn`  |

## Load pointer

`ldptr name`

`ldptr` can load the pointer of any function, label or variable.

| Datatype | Command |
| :------: | :-----: |
| Function | `ldptr` |
|  Label   | `ldptr` |
| Variable | `ldptr` |

## Load values from constant

`lddatatypec variable_name`

### Commands:

|       Datatype       | Command  |
| :------------------: | :------: |
|     8bit integer     | `ldi8c`  |
|    16bit integer     | `ldi16c` |
|    32bit integer     | `ldi32c` |
|    32bit integer     |  `ldic`  |
|    64bit integer     | `ldi64c` |
| 32bit floating point | `ldf32c` |
| 32bit floating point |  `ldfc`  |
| 64bit floating point | `ldf64c` |
| 64bit floating point |  `lddc`  |
|     ASCII string     | `ldsac`  |
|    Unicode string    | `ldsuc`  |
|         Bit          |  `ldbc`  |

## Pop value from stack

`pop` removes the top most value from the stack

## Prepare a variable for function return

`prep variable` prepares a variable for return. Since Persephone has first grade support for multiple return values, you can prepare multiple variables. 

## Function commands

### put

`put` returns the top most value from the stack into a prepared variable

### ret

`ret` returns from a function and jumps to the address of where the function was called +1.

## Compiler directives

### Include

`%include nameOfFile.psph`

The name of the file must be relative to the file you are including.

### Region
`%region nameOfRegion`
<br>
`%endregion nameOfRegion`

Regions help with formatting your code.

## Operators

### Arithmetic operators

The lower stack value is the right hand side, the upper is the left hand side of any operation. After an operation, both sides of arguments are popped from the stack and the result is pushed onto it.

#### Commands:

|       Datatype       | Command  |
| :------------------: | :------: |
|     Add two integers     | `add`  |
|    Subtract two integers     | `sub` |
|    Multiply two integers     | `mul` |
|    Divide two integers     |  `div`  |
|    Modulo operation on two integers     | `mod` |
|    Left shift     |  `shl`  |
|    Right shift     | `shr` |
|     Add two floats     | `addf`  |
|    Subtract two floats     | `subf` |
|    Multiply two floats     | `mulf` |
|    Divide two floats     |  `divf`  |
|    Modulo operation on two floats     | `modf` |

### Logical operators

#### Commands:

|       Datatype       | Command  |
| :------------------: | :------: |
|     And two bits     | `and`  |
|    Or two bits     | `or` |
|    Xor two bits     | `xor` |
|    Flip a bit     |  `not`  |

## Jump to a label

`jmp labelname`

## Persephone code sample

### fibonacci

```coffeescript
# declare extern variable (optional, if you want to modify the program response)
extern RETURN_CODE
# Persephone exposes many extern variables like the pc or other runtime information

dci32 0 # declare int32 constant
dci32 1 # declare int32 constant
dci32 10000 # declare int32 constant
dci8 0 # declare int8 constant

v_int32 var_01 # declare int32 variable
v_int32 var_02 # declare int32 variable

ldi32c 0 # load 0th constant onto stack
store var_01 # initialize var_01 with 0

ldi32c 1 # load constant of index 1 onto stack
store var_02 # initialize var_02 with 1

loop:
ldi32v var_01 # load int32 variable onto stack
ldi32c 2 # load constant of index 2 onto stack
jmple fib # jump to fib if var_01 is smaller or equal to 10000
jmp exit

fib:

ldi32v var_01 # load var_01 onto stack
ldi32v var_02 # load var_02 onto stack
add # add and put result onto stack
store var_01 # store top stack value into var_01

ldi32v var_01
ldi32v var_02
add
store var_02

ldi32v var_01
call 0x01 # print var_01

ldi32v var_02
call 0x01 # print var_02

jmp loop

exit: # exit program
ldi8c 4
store RETURN_CODE
```

### Shift

```coffeescript
dci32 4
dci32 16

v_int32 var_01

ldi32c 1
store var_01

ldi32c 0
ldi32v var_01
shl # left shift var_01 (16) by 4
store var_01 # assign shift result to var_01

ldi32v var_01
call 0x01 # print var_01
```

### Pointers

A pointer can either contain the address of a jump label, the address of a function or the address of a variable. Functions are called with `call` while labels are called with `jmp`.

### Pointer to function

```coffeescript
dcsa "Hello world"

print {
    v_dyn to_print
    store to_print # store value from stack into temporary variable
    
    lddyn to_print
    call 0x01
}

v_ptr print_ptr
ldptr print # loads the function ptr
store print_ptr

ldsac 0
call [print_ptr] # calls the function at the address of print_ptr
```

### Pointer to label

```coffeescript
dcsa "Hello"

loop:
ldsac 0
call 0x1

v_ptr loop_ptr
ldptr loop
store loop_ptr

jmp [loop_ptr]
```

### Pointer to variable

```coffeescript
dci32 42

v_int32 var0
ldi32c 0
store var0

v_ptr var0_ptr
ldptr var0
store var0_ptr

ldi32v [var0_ptr]
call 0x01
```

### Fixed address pointer

```coffeescript
dci8 0x01

v_ptr print
ldi8c 0
store print

call [print]
```

## Functions

```coffeescript
v_int32 status
v_stringa text

prep status
prep text

call function_that_returns_status_and_text

# status and text now have values returned by function_that_returns_status_and_text

pop
pop
```

```coffeescript
function_that_returns_status_and_text {
    ...
    ldi32v return_status
    put
    
    ldsav return_text
    put
    
    ret
}
```

## Creating arrays

```coffeescript
dci32 2

v_int32 var0
v_int32 var1
v_int32 var2
v_int32 var3
v_int32 var4

...

v_ptr arr_ptr
ldptr var0
store arr_ptr # arr_ptr is 0x0 on stack

ldptr arr_ptr
ldi32c 0
add # arr_ptr is 0x2 on stack

ldi32v [arr_ptr] # load var2 onto stack
call 0x01
```



