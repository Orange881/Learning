### 指令笔记

### PUSH 
将值推入栈顶

### MSTORE （offset, value）: (Stack(0), Stack(1))

MSTORE将**Stack(1)的值存储在内存中的Stack(0)**位置中

### CALLVALUE

将msg.value 推入栈顶

### SWAP1(a) - SWAP16(b)
交换第a 和 b 的位置

### DUP1 - DUP16 

将Stack(n-1)的值推入堆栈

### ISZERO
验证Stack(0)是否==0，如果是在第一个槽位推入 1（true）

### JUMPI

如果Stack(1) 是 1，EVM直接进入Stack(0)所在的位置，否则继续执行

### REVERT

停止当前上下文的执行，恢复状态更改，并将未使用的gas返回给调用者

### JUMPDEST

此操作在执行期间对计算机状态没有影响,只表示一个JUMP/JUMPI指令指向了这里，如果EVM跳到一个没有标记为 "JUMPDEST"的地址，它就会自动回退。


### CALLDATASIZE
等于msg.data.size，以太坊msg.data的数据字段的大小

### LT
比较堆栈上的两个值，Stack(0) < Stack(1),则返回 1， 否则 0

### EQ
比较两个值是否相等， if Stack(0) == Stack(1) return 1 else 0


### CALLDATALOAD (i)

接受1个参数Stack(0)作为偏移量，并将msg.data之后的在参数位置（这里是Stack(0)）的下一个32字节存储在堆栈中Stack(0)

### SHR (shift, value)

- shift: 要向右移位的位数.
- value: 要移位的32字节值.
- 向右（>>）执行二进制移位

### SSTORE (key, value)

- key: 32-byte key in storage.
- value: 32-byte value to store.
- 将Stack(1)存到 Stack(0)的位置

### CODECOPY (destOffset, offset, length)

将智能合约的代码从Stack(2)的字节复制到Stack(2) + Stack(1)的字节到内存 Stack(0)的位置


### RETURN (offset, size)

停止执行代码并返回Memory[Stack(0): Stack(0) + Stack(1)] <font color=#FF000> **重点： 该返回值存储在区块链中** </font>

### MLOAD (offset)

从内存中将Stack(0)地址中的值加载到堆栈中


