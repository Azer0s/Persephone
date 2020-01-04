# 💃🏻🍃 Persephone

## Variable declaration

`v_<datatype> variable_name` 

ASCII and Unicode strings can be loaded as 8 bit uints and vice versa. Dynamic variables (declared with `v_dyn`) can be used to store any data.

### Commands:

|       Datatype                |   Command   |
| :---------------------------: | :---------: |
|       Dynamic variable        |   `v_dyn`   | 
|         8bit integer          |  `v_int8`   |
|        16bit integer          |  `v_int16`  |
|        32bit integer          |  `v_int32`  |
|        32bit integer          |   `v_int`   |
|        64bit integer          |  `v_int64`  |
|     8bit unsigned integer     |  `v_uint8`  |
|    16bit unsigned integer     |  `v_uint16` |
|    32bit unsigned integer     |  `v_uint32` |
|    32bit unsigned integer     |   `v_uint`  |
|    64bit unsigned integer     |  `v_uint64` |
|    32bit floating point       | `v_float32` |
|    32bit floating point       |  `v_float`  |
|    64bit floating point       | `v_float64` |
|    64bit floating point       | `v_double`  |
|        ASCII string           | `v_stringa` |
|       Unicode string          | `v_stringu` |
|            Bit                |   `v_bit`   |
|          Pointer              |   `v_ptr`   |

When storing data, loading constants or variables, implicit conversion is possible.

```
u8 -> ASCII string
ASCII string -> u8

u8 -> all unsigned & signed int sizes & ptr
u16 -> all unsigned & signed int sizes & ptr
u32 -> all unsigned & signed int sizes & ptr
u64 -> all unsigned & signed int sizes & ptr

ptr (u32) -> all unsigned & signed int sizes & ptr

i8 -> all unsigned & signed int sizes & ptr
i16 -> all unsigned & signed int sizes & ptr
i32 -> all unsigned & signed int sizes & ptr
i64 -> all unsigned & signed int sizes & ptr
```

## Variable destruction

`delete variable_name`

## Constant declaration

`dc<datatype> value`

### Commands:

|            Datatype           | Command |
| :---------------------------: | :-----: |
|         8bit integer          | `dci8`  |
|        16bit integer          | `dci16` |
|        32bit integer          | `dci32` |
|        32bit integer          |  `dci`  |
|        64bit integer          | `dci64` |
|     8bit unsigned integer     | `dcu8`  |
|    16bit unsigned integer     | `dcu16` |
|    32bit unsigned integer     | `dcu32` |
|    32bit unsigned integer     |  `dcu`  |
|    64bit unsigned integer     | `dcu64` |
|     32bit floating point      | `dcf32` |
|     32bit floating point      |  `dcf`  |
|     64bit floating point      | `dcf64` |
|     64bit floating point      |  `dcd`  |
|         ASCII string          | `dcsa`  |
|        Unicode string         | `dcsu`  |
|             Bit               |  `dcb`  |

## Load values from variable

`ld<datatype>v variable_name`

To load a variable of unknown datatype, one can use `lddynv`.

### Commands:

|       Datatype       | Command  |
| :------------------: | :------: |
|     Dynamic variable    | `lddynv`  |
|     8bit integer     | `ldi8v`  |
|    16bit integer     | `ldi16v` |
|    32bit integer     | `ldi32v` |
|    32bit integer     |  `ldiv`  |
|    64bit integer     | `ldi64v` |
|     8bit unsigned integer     | `ldu8v`  |
|    16bit unsigned integer     | `ldu16v` |
|    32bit unsigned integer     | `ldu32v` |
|    32bit unsigned integer     |  `lduv`  |
|    64bit unsigned integer     | `ldu64v` |
| 32bit floating point | `ldf32v` |
| 32bit floating point |  `ldfv`  |
| 64bit floating point | `ldf64v` |
| 64bit floating point |  `lddv`  |
|     ASCII string     | `ldsav`  |
|    Unicode string    | `ldsuv`  |
|         Bit          |  `ldbv`  |
|         Ptr          | `ldptrv` |

## Load pointer

One can load the pointer of any function, label or variable. In Persephone, labels, functions and variables are stored in one address space. This means that if you have 4 labels, the pointer (without memory offset) to the first variable is going to be 0x4.

```
+-----------------+-----------------+-----------------+-----------------+-------------- ~ ~ --------------+
|  Label 1 (0x0)  |  Label 2 (0x1)  |  Label 3 (0x2)  |  Label 4 (0x3)  |  Var 1 (0x4)  ~ ~  Var n (0xn)  |
+-----------------+-----------------+-----------------+-----------------+-------------- ~ ~ --------------+
```

| Datatype | Command |
| :------: | :-----: |
|  Label   | `ldptr` |
| Variable | `ldptr` |

## Load values from constant

`ld<datatype>c variable_name`

### Commands:

|       Datatype       | Command  |
| :------------------: | :------: |
|     8bit integer     | `ldi8c`  |
|    16bit integer     | `ldi16c` |
|    32bit integer     | `ldi32c` |
|    32bit integer     |  `ldic`  |
|    64bit integer     | `ldi64c` |
|     8bit unsigned integer     | `ldu8c`  |
|    16bit unsigned integer     | `ldu16c` |
|    32bit unsigned integer     | `ldu32c` |
|    32bit unsigned integer     |  `lduc`  |
|    64bit unsigned integer     | `ldu64c` |
| 32bit floating point | `ldf32c` |
| 32bit floating point |  `ldfc`  |
| 64bit floating point | `ldf64c` |
| 64bit floating point |  `lddc`  |
|     ASCII string     | `ldsac`  |
|    Unicode string    | `ldsuc`  |
|         Bit          |  `ldbc`  |

## Pop value from stack

`pop` removes the top most value from the stack

## Return from label

`ret` returns from a function and jumps to the address of where the function was called +1.

## Noop

`nop` does nothing.

## Constant pointer base

`cbase` sets the offset for the pointer to the constant map for a specific code region.

```coffeescript
a:

#        <---+
dcsa "Foo" # |
dcsa "Bar" # | Constant offset is 0, `ldsac 0` would put "Foo" on the stack
dcsa "Baz" # |
#        <---+
   
cbase

#        <---+
dcsa "abc" # |
dcsa "def" # | Constant offset is 3, `ldsac 0` would put "a" on the stack
dcsa "ghi" # |
#        <---+

jmp a # when jumping, the constant offset gets set according to the region
```

## Len

For strings, `len` puts the number of characters of the string on the stack. For all other values, their bit size is put on the stack. `len` puts an 32 bit unsigned int on the stack.

## Type

`type` puts the datatype of a variable as u8 on the stack.

|            Datatype             | Code  |
| :------------------: | :------: |
|  `uint`   |     0x0     |
|  `int`   |     0x1     |
|  `float`  |    0x2     |
|  `ASCII string`  |    0x3     |
|  `Unicode string`  |    0x4     |
|  `Bit`  |    0x5     |
|  `Ptr`  |    0x6     |

## Compiler directives

### Include

`%include nameOfFile.psph`

The name of the file must be relative to the file you are including.

### Region
`%region nameOfRegion`
<br>
`%endregion`

Regions help with formatting your code.

## Operators

### Arithmetic operators

The lower stack value is the left hand side, the upper is the right hand side of any operation. After an operation, both sides of arguments are popped from the stack and the result is pushed onto it.

#### Commands:

|            Operation             | Command  |
| :------------------: | :------: |
|     Add two integers     | `add`  |
|    Subtract two integers     | `sub` |
|    Multiply two integers     | `mul` |
|    Divide two integers     |  `div`  |
|    Modulo operation on two integers     | `mod` |
| Greater or equal | `ge` |
| Less or equal | `le` |
| Greater than | `gt` |
| Less than | `lt` |
|     And two ints     | `andi`  |
| Or two ints | `ori` |
| Xor two ints | `xori` |
| Flip int bits |  `noti`  |
|    Left shift     |  `shl`  |
|    Right shift     | `shr` |
| Increment by one | `inc` |
| Decrement by one | `dec` |
|     Add two floats     | `addf`  |
|    Subtract two floats     | `subf` |
|    Multiply two floats     | `mulf` |
|    Divide two floats     |  `divf`  |
| Greater or equal (float) | `gef` |
| Less or equal (float) | `lef` |
| Greater than (float) | `gtf` |
| Less than (float) | `ltf` |

The size of the result value is determined by the size of the operands.
The result value will have the smallest size possible.

E.g.: Left value is i8, right value is i16 => result value will be i16

The result value will be unsigned only of both operands are unsigned.

### Logical operators

#### Commands:

|       Operation       | Command  |
| :------------------: | :------: |
|  Equality operator | `eq` |
|     And two bits     | `and`  |
| Or two bits | `or` |
| Xor two bits | `xor` |
| Flip a bit |  `not`  |

### String operations

#### Commands:

|                   Operation                    |     Command     |
| :--------------------------------------------: | :-------------: |
|            Concatenate two strings             |     `conc`      |
| Get a character (stack: position) |     `getc <nameOfVar>`      |
| Set a character (lower: position, upper: char) |     `setc <nameOfVar>`      |

## Jump to a label

`jmp <labelname>`

## Conditional jump

### Jump if top of stack is 1/true

`jmpt <labelname>`

### Jump if top of stack is 0/false

`jmpf <labelname>`

## call vs jmp

`call` puts an address into the return stack while `jmp` does not. If you want to return from a label, use `call`. If you just want to jump to a label, use `jmp`.

## Bytecode

At the beginning of a binary file, you have to declare labels. The first 16 bits of the file declares how long the label section is (size is the number of labels).

A label consists of two 64 bit ints. The first one is the label name (or a hex representation/placeholder for it). The second int is the position of the byte the label jumps to.

### Examples

```
0x1 0xFA63 0x12
```

This means that there is only one label. The single label has the name 0xFA63 and points to the 18th byte of the program (after the label section).

### Commands

A command is always an unsigned 16 bit integer. Commands with parameters are followed by an unsigned 8 bit integer that determines the type of the following parameter.

### Parameters

|       Type       |   Opcode   |
| :------------------: | :---------: |
|  `uint`   |     0x0     |
|  `int`   |     0x1     |
|  `float`  |    0x2     |
|  `ASCII string`  |    0x3     |
|  `Unicode string`  |    0x4     |
|  `Bit`  |    0x5     |
|  `Ptr`  |    0x6     |
| Labelname | 0xE|
|  Variable  |    0xF     |

`int` and `uint` is followed by the size of the integer. This can either be 0x8 (u/int8), 0x10 (u/int16), 0x20 (u/int32) or 0x40 (u/int64). The actual value comes after the size. Leading zeroes are required if the value doesn't fill all bits.

`float` is also followed by a size. This can be 0x21 (float32) or 0x41 (float64). The actual value comes after the size. Leading zeroes are required if the value doesn't fill all bits.

`ASCII string` and `Unicode string` are followed by the size of the string (total bytes as unsigned 64 bit integer). The actual value comes after the size. Persephone should infer the type of the string (if the length of the byte array is the same length as the string, it's an ASCII string).

`Bit` is followed by either a 0x0 or a 0x1.

`Ptr` is followed by a 64 bit unsigned integer variable name.

Commands that require a varname, are followed by 0xF and a 64 bit unsigned integer value which stands for the variable name.

#### Example: Int

42 is 0x2A in hex and 00101010 in binary. If we want to put 42 into a 16 bit integer, we need to frontpad it. That means that it would look like so: 0000000000101010 or 0x002A in hex.

```
0x1 0x10 0x002A
```

By default, Persephone should infer the int size from the value.

|   Range   |   Bits   |
| :---------: | :---------: |
|-128 to 127  |int8|
| 0 to 255  |uint8|
|-32768 to 32767     |int16|
| 0 to 65535     |uint16|
|-2147483648 to 2147483647    |int32|
| 0 to 4294967295    |uint32|
|-9223372036854775808 to 9223372036854775807      |int64|
| 0 to 18446744073709551615      |uint64|

### Syscalls

Load parameter onto stack, call `syscall` with opcode or ptr as parameter.

|   Command   |   Opcode   |
| :---------: | :---------: |
|Print      |0x01|
|Println     |0x10|
|Read char    |0x02|
|Read line    |0x20|


### Command opcodes 

|   Command   |   Opcode   |
| :---------: | :---------: |
|`type`       |0xEEEE|
|`nop`        |0x1000|
|`store`      |0x0000|
|`v_dyn`      |0x0101|
|`v_int8`     |0x0108|
|`v_int16`    |0x0110|
|`v_int32`    |0x0120|
|`v_int`      |0x0120|
|`v_int64`    |0x0140|
|`v_uint8`    |0x1108|
|`v_uint16`   |0x1110|
|`v_uint32`   |0x1120|
|`v_uint`     |0x1120|
|`v_uint64`   |0x1140|
|`v_float32`  |0x0121|
|`v_float`    |0x0121|
|`v_float64`  |0x0141|
|`v_double`   |0x0141|
|`v_stringa`  |0x0131|
|`v_stringu`  |0x0132|
|`v_ptr`      |0x0150|
|`v_bit`      |0x0100|
|`delete`     |0x0099|
|`dci8`       |0x0218|
|`dci16`      |0x0210|
|`dci32`      |0x0220|
|`dci`        |0x0220|
|`dci64`      |0x0240|
|`dcu8`       |0x1218|
|`dcu16`      |0x1210|
|`dcu32`      |0x1220|
|`dcu`        |0x1220|
|`dcu64`      |0x1240|
|`dcf32`      |0x0221|
|`dcf`        |0x0221|
|`dcf64`      |0x0241|
|`dcd`        |0x0241|
|`dcsa`       |0x0231|
|`dcsu`       |0x0232|
|`dcb`        |0x0200|
|`lddynv`     |0x0301|
|`ldi8v`      |0x0318|
|`ldi16v`     |0x0310|
|`ldi32v`     |0x0320|
|`ldiv`       |0x0320|
|`ldi64v`     |0x0340|
|`ldu8v`      |0x1318|
|`ldu16v`     |0x1310|
|`ldu32v`     |0x1320|
|`lduv`       |0x1320|
|`ldu64v`     |0x1340|
|`ldf32v`     |0x0321|
|`ldfv`       |0x0321|
|`ldf64v`     |0x0341|
|`lddv`       |0x0341|
|`ldsav`      |0x0331|
|`ldsuv`      |0x0332|
|`ldptrv`     |0x0350|
|`ldbv`       |0x0300|
|`ldi8c`      |0x0418|
|`ldi16c`     |0x0410|
|`ldi32c`     |0x0420|
|`ldic`       |0x0420|
|`ldi64c`     |0x0440|
|`ldu8c`      |0x1418|
|`ldu16c`     |0x1410|
|`ldu32c`     |0x1420|
|`lduc`       |0x1420|
|`ldu64c`     |0x1440|
|`ldf32c`     |0x0421|
|`ldfc`       |0x0421|
|`ldf64c`     |0x0441|
|`lddc`       |0x0441|
|`ldsac`      |0x0431|
|`ldsuc`      |0x0432|
|`ldbc`       |0x0400|
|`cbase`      |0xFFFF|
|`pop`        |0x0001|
|`ret`        |0x0002|
|`eq`         |0x0030|
|`add`        |0x0003|
|`sub`        |0x0004|
|`mul`        |0x0005|
|`div`        |0x0006|
|`mod`        |0x0007|
|`ge`         |0x0008|
|`le`         |0x0009|
|`gt`         |0x000A|
|`lt`         |0x000B|
|`andi`       |0x000C|
|`ori`        |0x000D|
|`xori`       |0x000E|
|`noti`       |0x000F|
|`shl`        |0x0010|
|`shr`        |0x0011|
|`inc`        |0x0012|
|`dec`        |0x0013|
|`addf`       |0x0014|
|`subf`       |0x0015|
|`mulf`       |0x0016|
|`divf`       |0x0017|
|`gef`        |0x0018|
|`lef`        |0x0019|
|`gtf`        |0x001A|
|`ltf`        |0x001B|
|`and`        |0x001C|
|`or`         |0x001D|
|`xor`        |0x001E|
|`not`        |0x001F|
|`conc`       |0x0020|
|`len`        |0x0021|
|`getc`       |0x0022|
|`setc`       |0x0023|
|`syscall`    |0x0024|
|`extern`     |0x0025|

### jmp/call

The `jmp` and `call` commands are followed by the hex value 0xE (labelname type) and a 64 bit unsigned int (the label name), a pointer value or an int value.

|   Command   |   Opcode   |
| :---------: | :---------: |
| `call`| 0xF000 |
|  `jmp`   |0xF001|
|  `jmpt`  |0xF002|
|  `jmpf`  |0xF003|

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
le
jmpt fib # jump to fib if var_01 is smaller or equal to 10000
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
syscall 0x10 # print var_01

ldi32v var_02
syscall 0x10 # print var_02

jmp loop

exit: # exit program
ldi8c 4
store RETURN_CODE
```

### Boolean

```coffeescript
dcb false
dcb true

ldbc 0
ldbc 1
xor
syscall 0x10

ldbc 1
ldbc 1
and
pop
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
syscall 0x10 # print var_01
```

### Pointers

A pointer can either contain the address of a jump label, the address of a function or the address of a variable. Functions are called with `call` while labels are called with `jmp`.

### Pointer to function label

```coffeescript
dcsa "Hello world"

print:
    syscall 0x10
    ret

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
syscall 0x10

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
syscall 0x10
```

### Fixed address pointer

```coffeescript
dci8 0x10

v_ptr print
ldi8c 0
store print

syscall [print]
```

## Functions

```coffeescript
v_int32 status
v_stringa text

call return_status_and_text

store status
store text

# status and text now have values returned by function_that_returns_status_and_text
```

```coffeescript
return_status_and_text:
    ...
    ldi32v return_status
    ldsav return_text
    
    ret
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
store arr_ptr # arr_ptr is 0x0

ldptrv arr_ptr
ldi32c 0
add
store arr_ptr # arr_ptr is 0x2

ldi32v [arr_ptr] # load var2 onto stack
syscall 0x10
```
