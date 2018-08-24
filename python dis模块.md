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











