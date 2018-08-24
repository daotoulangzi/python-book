### Python dis 模块
---
- dis 通过反汇编支持Cpython字节码的分析；
- 字节码是CPython解释器的实现细节。 不保证不会在Python版本之间添加，删除或更改字节码；
- 在版本3.6中更改：为每条指令使用2个字节。 以前字节数因指令而异。

Example:
```
>>> def myfunc(alist):
    	return len(alist)
```
```
>>> from dis import dis
>>> dis(myfunc)
	2	0 LOAD_GLOBAL		0 (len)  # 2是行号
		2 LOAD_FAST			0 (alist)
		4 CALL_FUNCTION		1
		6 RETURN_VALUE

```
#### 常用的指令 General instruction
- NOP: `Do nothing code. Used as a placeholder by the bytecode optimizer.`
- TOP: `Removes the top-of-stack (TOS) item.(删除栈顶项)`
- ROT_TWO: `Swaps the two top-most stack items.(交换栈顶的两项)`
- ROT_THREE: `Lifts second and third stack item one position up, moves top down to position three.(第二位和第三位上移，第一位移至第三位)`
- DUP_TOP: `Duplicates the reference on top of the stack.(复制栈顶的引用)`
- DUP_TOP_TWO: `Duplicates the two references on top of the stack, leaving them in the same order(复制栈顶的引用，并使他们保持相同的顺序)`
#### 一元引用 Unary operations
```
Unary operations take the top of the stack, apply the operation, and push the result back on the stack.
(栈顶部数据应用，并将数据返回给栈顶)
```
- UNARY_POSITIVE: `mplements TOS = +TOS`
- UNARY_NEGATIVE: `Implements TOS = -TOS`
- UNARY_NOT: `Implements TOS = not TOS`
- UNARY_INVERT: `Implements TOS = ~TOS`
- GET_ITER： `Implements TOS = iter(TOS)`
- GET_YIELD_FROM_ITER: `If TOS is a generator iterator or coroutine object it is left as is. Otherwise, implements TOS = iter(TOS)`
#### 二进制操作 Binary operations
```
Binary operations remove the top of the stack (TOS) and the second top-most stack item (TOS1) from the stack. 
They perform the operation, and put the result back on the stack.
(二进制操作从栈中删除栈顶部（TOS）和次栈顶（TOS1）。 
它们执行操作，并将结果放回栈。)
```
- BINARY_POWER: `Implements TOS = TOS1 ** TOS.(求次方，如：a = a ** 2)`
- BINARY_MULTIPLY: `Implements TOS = TOS1 * TOS.(求乘积)`
- BINARY_MATRIX_MULTIPLY: `Implements TOS = TOS1 @ TOS.`
- BINARY_FLOOR_DIVIDE: `Implements TOS = TOS1 // TOS.`
- BINARY_TRUE_DIVIDE: `Implements TOS = TOS1 / TOS.`
- BINARY_MODULO: `Implements TOS = TOS1 % TOS.`
- BINARY_ADD: `Implements TOS = TOS1 + TOS.`
- BINARY_SUBTRACT: `Implements TOS = TOS1 - TOS.`
- BINARY_SUBSCR: `Implements TOS = TOS1[TOS].`
- BINARY_LSHIFT: `Implements TOS = TOS1 << TOS.`
- BINARY_RSHIFT: `Implements TOS = TOS1 >> TOS.`
- BINARY_AND: `Implements TOS = TOS1 & TOS.`
- BINARY_XOR: `Implements TOS = TOS1 ^ TOS.
- BNARY_OR: `Implements TOS = TOS1 | TOS.`
#### 就地操作 In-place operations
```
In-place operations are like binary operations, in that they remove TOS and TOS1, and push the result back on the stack, 
but the operation is done in-place when TOS1 supports it, and the resulting TOS may be (but does not have to be) the original TOS1.
(就地操作类似于二进制操作，因为它们删除了TOS和TOS1，并将结果推回到堆栈上，
但是当TOS1支持时，操作就地完成，并且生成的TOS可能是原始的TOS1。)
```
- INPLACE_POWER: `Implements in-place TOS = TOS1 ** TOS.`
- INPLACE_MULTIPLY: `Implements in-place TOS = TOS1 * TOS.`
- INPLACE_MATRIX_MULTIPLY: `Implements in-place TOS = TOS1 @ TOS.`
- INPLACE_FLOOR_DIVIDE: `Implements in-place TOS = TOS1 // TOS.`
- INPLACE_TRUE_DIVIDE: `Implements in-place TOS = TOS1 / TOS.`
- INPLACE_MODULO: `Implements in-place TOS = TOS1 % TOS.`
- INPLACE_ADD: `Implements in-place TOS = TOS1 + TOS.`
- INPLACE_SUBTRACT: `Implements in-place TOS = TOS1 - TOS.`
- INPLACE_LSHIFT: `Implements in-place TOS = TOS1 << TOS.`
- INPLACE_RSHIFT: `Implements in-place TOS = TOS1 >> TOS.`
- INPLACE_AND: `Implements in-place TOS = TOS1 & TOS.`
- INPLACE_XOR: `Implements in-place TOS = TOS1 ^ TOS.`
- INPLACE_OR: `Implements in-place TOS = TOS1 | TOS.`
#### Coroutine操作码
- GET_AWAITABLE: `Implements TOS = get_awaitable(TOS), where get_awaitable(o) returns o if o is a coroutine object or a generator object with the CO_ITERABLE_COROUTINE flag, or resolves o.__await__.`

- GET_AITER: `Implements TOS = TOS.__aiter__().`

- GET_ANEXT: `Implements PUSH(get_awaitable(TOS.__anext__())). See GET_AWAITABLE for details about get_awaitable`
- BEFORE_ASYNC_WITH: `Resolves __aenter__ and __aexit__ from the object on top of the stack. Pushes __aexit__ and result of __aenter__() to the stack.`
- SETUP_ASYNC_WITH: `Creates a new frame object.`
#### 其他操作码
- PRINT_EXPR: `Implements the expression statement for the interactive mode. TOS is removed from the stack and printed. In non-interactive mode, an expression statement is terminated with POP_TOP.`
- BREAK_LOOP: `Terminates a loop due to a break statement.`
- CONTINUE_LOOP(target): `Continues a loop due to a continue statement. target is the address to jump to (which should be a FOR_ITER instruction).`
- SET_ADD(i): `Calls set.add(TOS1[-i], TOS). Used to implement set comprehensions.`
- LIST_APPEND(i): `Calls list.append(TOS[-i], TOS). Used to implement list comprehensions.`
- MAP_ADD(i): `Calls dict.setitem(TOS1[-i], TOS, TOS1). Used to implement dict comprehensions.`
```
For all of the SET_ADD, LIST_APPEND and MAP_ADD instructions, 
while the added value or key/value pair is popped off, the container object remains on the stack 
so that it is available for further iterations of the loop.
```

- RETURN_VALUE: `Returns with TOS to the caller of the function.`
- YIELD_VALUE: `Pops TOS and yields it from a generator.`
- YIELD_FROM: `Pops TOS and delegates to it as a subiterator from a generator.`
- SETUP_ANNOTATIONS: `Checks whether __annotations__ is defined in locals(), if not it is set up to an empty dict. This opcode is only emitted if a class or module body contains variable annotations statically.`
- IMPORT_STAR: `Loads all symbols not starting with '_' directly from the module TOS to the local namespace. The module is popped after loading all names. This opcode implements from module import *.`
- POP_BLOCK: `Removes one block from the block stack. Per frame, there is a stack of blocks, denoting nested loops, try statements, and such.`
- POP_EXCEPT: `Removes one block from the block stack. The popped block must be an exception handler block, as implicitly created when entering an except handler. In addition to popping extraneous values from the frame stack, the last three popped values are used to restore the exception state.`
- END_FINALLY: `Terminates a finally clause. The interpreter recalls whether the exception has to be re-raised, or whether the function returns, and continues with the outer-next block.`
- LOAD_BUILD_CLASS: `Pushes builtins.__build_class__() onto the stack. It is later called by CALL_FUNCTION to construct a class.`
- SETUP_WITH(delta): `This opcode performs several operations before a with block starts. First, it loads __exit__() from the context manager and pushes it onto the stack for later use by WITH_CLEANUP. Then, __enter__() is called, and a finally block pointing to delta is pushed. Finally, the result of calling the enter method is pushed onto the stack. The next opcode will either ignore it (POP_TOP), or store it in (a) variable(s) (STORE_FAST, STORE_NAME, or UNPACK_SEQUENCE).`
- WITH_CLEANUP_START: `Cleans up the stack when a with statement block exits. TOS is the context manager’s __exit__() bound method. Below TOS are 1–3 values indicating how/why the finally clause was entered:
SECOND = None
(SECOND, THIRD) = (WHY_{RETURN,CONTINUE}), retval
SECOND = WHY_*; no retval below it
(SECOND, THIRD, FOURTH) = exc_info()
In the last case, TOS(SECOND, THIRD, FOURTH) is called, otherwise TOS(None, None, None). Pushes SECOND and result of the call to the stack.`
- WITH_CLEANUP_FINISH: `Pops exception type and result of ‘exit’ function call from the stack.
If the stack represents an exception, and the function call returns a ‘true’ value, this information is “zapped” and replaced with a single WHY_SILENCED to prevent END_FINALLY from re-raising the exception. (But non-local gotos will still be resumed.)`

#### 使用参数的操作码
- STORE_NAME(namei): `Implements name = TOS. namei is the index of name in the attribute co_names of the code object. The compiler tries to use STORE_FAST or STORE_GLOBAL if possible.`
- DELETE_NAME(namei): `Implements del name, where namei is the index into co_names attribute of the code object.`
- UNPACK_SEQUENCE(count): `Unpacks TOS into count individual values, which are put onto the stack right-to-left.`
- UNPACK_EX(counts): `Implements assignment with a starred target: Unpacks an iterable in TOS into individual values, where the total number of values can be smaller than the number of items in the iterable: one of the new values will be a list of all leftover items.
The low byte of counts is the number of values before the list value, the high byte of counts the number of values after it. The resulting values are put onto the stack right-to-left.`
- STORE_ATTR(namei): `Implements TOS.name = TOS1, where namei is the index of name in co_names.`
- DELETE_ATTR(namei): `Implements del TOS.name, using namei as index into co_names.`
- STORE_GLOBAL(namei): `Works as STORE_NAME, but stores the name as a global.`
- DELETE_GLOBAL(namei): `Works as DELETE_NAME, but deletes a global name.`
- LOAD_CONST(consti): `Pushes co_consts[consti] onto the stack.`

- LOAD_NAME(namei): `Pushes the value associated with co_names[namei] onto the stack.`

- BUILD_TUPLE(count): `Creates a tuple consuming count items from the stack, and pushes the resulting tuple onto the stack.`

- BUILD_LIST(count): `Works as BUILD_TUPLE, but creates a list.`

- BUILD_SET(count): `Works as BUILD_TUPLE, but creates a set.`

- BUILD_MAP(count): `Pushes a new dictionary object onto the stack. Pops 2 * count items so that the dictionary holds count entries: {..., TOS3: TOS2, TOS1: TOS}.
Changed in version 3.5: The dictionary is created from stack items instead of creating an empty dictionary pre-sized to hold count items.`

- BUILD_CONST_KEY_MAP(count): `The version of BUILD_MAP specialized for constant keys. count values are consumed from the stack. The top element on the stack contains a tuple of keys.`

- BUILD_STRING(count): `Concatenates count strings from the stack and pushes the resulting string onto the stack.`


- BUILD_TUPLE_UNPACK(count): `Pops count iterables from the stack, joins them in a single tuple, and pushes the result. Implements iterable unpacking in tuple displays (*x, *y, *z).`

- BUILD_TUPLE_UNPACK_WITH_CALL(count): `This is similar to BUILD_TUPLE_UNPACK, but is used for f(*x, *y, *z) call syntax. The stack item at position count + 1 should be the corresponding callable f.`

- BUILD_LIST_UNPACK(count): `This is similar to BUILD_TUPLE_UNPACK, but pushes a list instead of tuple. Implements iterable unpacking in list displays [*x, *y, *z].`

- BUILD_SET_UNPACK(count): `This is similar to BUILD_TUPLE_UNPACK, but pushes a set instead of tuple. Implements iterable unpacking in set displays {*x, *y, *z}.`

- BUILD_MAP_UNPACK(count): `Pops count mappings from the stack, merges them into a single dictionary, and pushes the result. Implements dictionary unpacking in dictionary displays {**x, **y, **z}.`

- BUILD_MAP_UNPACK_WITH_CALL(count): `This is similar to BUILD_MAP_UNPACK, but is used for f(**x, **y, **z) call syntax. The stack item at position count + 2 should be the corresponding callable f.
Changed in version 3.6: The position of the callable is determined by adding 2 to the opcode argument instead of encoding it in the second byte of the argument.`

- LOAD_ATTR(namei): `Replaces TOS with getattr(TOS, co_names[namei]).`

- COMPARE_OP(opname): `Performs a Boolean operation. The operation name can be found in cmp_op[opname].`

- IMPORT_NAME(namei): `Imports the module co_names[namei]. TOS and TOS1 are popped and provide the fromlist and level arguments of __import__(). The module object is pushed onto the stack. The current namespace is not affected: for a proper import statement, a subsequent STORE_FAST instruction modifies the namespace.`

- IMPORT_FROM(namei): `Loads the attribute co_names[namei] from the module found in TOS. The resulting object is pushed onto the stack, to be subsequently stored by a STORE_FAST instruction.`

- JUMP_FORWARD(delta): `Increments bytecode counter by delta.`

- POP_JUMP_IF_TRUE(target): `If TOS is true, sets the bytecode counter to target. TOS is popped.`

- POP_JUMP_IF_FALSE(target)
If TOS is false, sets the bytecode counter to target. TOS is popped.

- JUMP_IF_TRUE_OR_POP(target): `If TOS is true, sets the bytecode counter to target and leaves TOS on the stack. Otherwise (TOS is false), TOS is popped.`

- JUMP_IF_FALSE_OR_POP(target): `If TOS is false, sets the bytecode counter to target and leaves TOS on the stack. Otherwise (TOS is true), TOS is popped.`

- JUMP_ABSOLUTE(target): `Set bytecode counter to target.`

- FOR_ITER(delta): `TOS is an iterator. Call its __next__() method. If this yields a new value, push it on the stack (leaving the iterator below it). If the iterator indicates it is exhausted TOS is popped, and the byte code counter is incremented by delta.`

- LOAD_GLOBAL(namei): `Loads the global named co_names[namei] onto the stack.`

- SETUP_LOOP(delta): `Pushes a block for a loop onto the block stack. The block spans from the current instruction with a size of delta bytes.`

- SETUP_EXCEPT(delta): `Pushes a try block from a try-except clause onto the block stack. delta points to the first except block.`

- SETUP_FINALLY(delta): `Pushes a try block from a try-except clause onto the block stack. delta points to the finally block.`

- LOAD_FAST(var_num): `Pushes a reference to the local co_varnames[var_num] onto the stack.`

- STORE_FAST(var_num): `Stores TOS into the local co_varnames[var_num].`

- DELETE_FAST(var_num): `Deletes local co_varnames[var_num].`

- LOAD_CLOSURE(i): `Pushes a reference to the cell contained in slot i of the cell and free variable storage. The name of the variable is co_cellvars[i] if i is less than the length of co_cellvars. Otherwise it is co_freevars[i - len(co_cellvars)].`

- LOAD_DEREF(i): `Loads the cell contained in slot i of the cell and free variable storage. Pushes a reference to the object the cell contains on the stack.`

- LOAD_CLASSDEREF(i): `Much like LOAD_DEREF but first checks the locals dictionary before consulting the cell. This is used for loading free variables in class bodies.`
- STORE_DEREF(i): `Stores TOS into the cell contained in slot i of the cell and free variable storage.`

- DELETE_DEREF(i): `Empties the cell contained in slot i of the cell and free variable storage. Used by the del statement.`

- RAISE_VARARGS(argc): `Raises an exception. argc indicates the number of parameters to the raise statement, ranging from 0 to 3. The handler will find the traceback as TOS2, the parameter as TOS1, and the exception as TOS.`

- CALL_FUNCTION(argc): `Calls a function. argc indicates the number of positional arguments. The positional arguments are on the stack, with the right-most argument on top. Below the arguments, the function object to call is on the stack. Pops all function arguments, and the function itself off the stack, and pushes the return value.`

- CALL_FUNCTION_KW(argc): `Calls a function. argc indicates the number of arguments (positional and keyword). The top element on the stack contains a tuple of keyword argument names. Below the tuple, keyword arguments are on the stack, in the order corresponding to the tuple. Below the keyword arguments, the positional arguments are on the stack, with the right-most parameter on top. Below the arguments, the function object to call is on the stack. Pops all function arguments, and the function itself off the stack, and pushes the return value.`

- CALL_FUNCTION_EX(flags): `Calls a function. The lowest bit of flags indicates whether the var-keyword argument is placed at the top of the stack. Below the var-keyword argument, the var-positional argument is on the stack. Below the arguments, the function object to call is placed. Pops all function arguments, and the function itself off the stack, and pushes the return value. Note that this opcode pops at most three items from the stack. Var-positional and var-keyword arguments are packed by BUILD_TUPLE_UNPACK_WITH_CALL and BUILD_MAP_UNPACK_WITH_CALL.`

- LOAD_METHOD(namei): `Loads a method named co_names[namei] from TOS object. TOS is popped and method and TOS are pushed when interpreter can call unbound method directly. TOS will be used as the first argument (self) by CALL_METHOD. Otherwise, NULL and method is pushed (method is bound method or something else).`

- CALL_METHOD(argc): `Calls a method. argc is number of positional arguments. Keyword arguments are not supported. This opcode is designed to be used with LOAD_METHOD. Positional arguments are on top of the stack. Below them, two items described in LOAD_METHOD on the stack. All of them are popped and return value is pushed.`

- MAKE_FUNCTION(argc): `Pushes a new function object on the stack. From bottom to top, the consumed stack must consist of values if the argument carries a specified flag value
0x01 a tuple of default argument objects in positional order
0x02 a dictionary of keyword-only parameters’ default values
0x04 an annotation dictionary
0x08 a tuple containing cells for free variables, making a closure
the code associated with the function (at TOS1)
the qualified name of the function (at TOS)`
- BUILD_SLICE(argc): `Pushes a slice object on the stack. argc must be 2 or 3. If it is 2, slice(TOS1, TOS) is pushed; if it is 3, slice(TOS2, TOS1, TOS) is pushed. See the slice() built-in function for more information.`

- EXTENDED_ARG(ext): `Prefixes any opcode which has an argument too big to fit into the default two bytes. ext holds two additional bytes which, taken together with the subsequent opcode’s argument, comprise a four-byte argument, ext being the two most-significant bytes.`

- FORMAT_VALUE(flags): `Used for implementing formatted literal strings (f-strings). Pops an optional fmt_spec from the stack, then a required value. flags is interpreted as follows:
(flags & 0x03) == 0x00: value is formatted as-is.
(flags & 0x03) == 0x01: call str() on value before formatting it.
(flags & 0x03) == 0x02: call repr() on value before formatting it.
(flags & 0x03) == 0x03: call ascii() on value before formatting it.
(flags & 0x04) == 0x04: pop fmt_spec from the stack and use it, else use an empty fmt_spec.
Formatting is performed using PyObject_Format(). The result is pushed on the stack.`

- HAVE_ARGUMENT: `This is not really an opcode. It identifies the dividing line between opcodes which don’t use their argument and those that do (< HAVE_ARGUMENT and >= HAVE_ARGUMENT, respectively).
Changed in version 3.6: Now every instruction has an argument, but opcodes < HAVE_ARGUMENT ignore it. Before, only opcodes >= HAVE_ARGUMENT had an argument.		`









